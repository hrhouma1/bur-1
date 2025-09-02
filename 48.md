# TRAVAIL PRATIQUE – APPLICATION FASTAPI (CRUD) & GESTION AVEC GIT — CORRECTION COMPLÈTE

- * **Nom de l'étudiant** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- * **Groupe** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_


## 1) INITIALISATION DU PROJET

### Rappel (question)

Créer un projet FastAPI, un environnement virtuel, un `requirements.txt`, initialiser Git et faire un premier commit avec `README.md`.

### Correction détaillée

#### 1.1. Créer le dossier et s’y rendre

```bash
mkdir projet_fastapi_crud
cd projet_fastapi_crud
```

#### 1.2. Créer et activer l’environnement virtuel

**macOS / Linux :**

```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows (PowerShell) :**

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

#### 1.3. Créer `requirements.txt` et installer

Créez un fichier `requirements.txt` à la racine, avec :

```
fastapi==0.115.0
uvicorn[standard]==0.30.6
Jinja2==3.1.4
python-multipart==0.0.12
SQLAlchemy==2.0.32
alembic==1.13.2
pydantic==2.9.1
python-dotenv==1.0.1
httpx==0.27.2
pytest==8.3.2
```

Installez :

```bash
pip install -r requirements.txt
```

#### 1.4. Fichiers de base

```bash
mkdir -p app templates static data scripts tests
printf "TP FastAPI CRUD (Items) + HTML + API + Tests + Git.\n" > README.md
```

#### 1.5. `.gitignore` minimal

Créez `.gitignore` :

```
venv/
.env
__pycache__/
*.pyc
data/*.db*
.data/
.alembic/
```

#### 1.6. Initialiser Git et premier commit

```bash
git init
git add README.md requirements.txt .gitignore
git commit -m "Initialisation du projet FastAPI CRUD"
```

*(Pensez à créer un `git_commands.txt` modèle si demandé par l’énoncé.)*

---

## 2) SQUELETTE FASTAPI MINIMAL

### Rappel (question)

Créer un squelette d’app qui répond “Hello, FastAPI!” à `/` et se lance avec uvicorn.

### Correction détaillée

#### Créer la branche

```bash
git branch fastapi-init
git checkout fastapi-init
```

#### Fichiers

`app/__init__.py` (vide ou commentaire)

```python
# permet de considérer app/ comme un package
```

`app/main.py`

```python
from fastapi import FastAPI
from fastapi.responses import PlainTextResponse

app = FastAPI(title="Inventaire - FastAPI")

@app.get("/", response_class=PlainTextResponse)
def root():
    return "Hello, FastAPI!"
```

Lancer localement (pendant le dev) :

```bash
uvicorn app.main:app --reload
# Ouvrir http://127.0.0.1:8000
```

#### Git (ajout → commit → merge → suppression de branche)

```bash
git add app/__init__.py app/main.py
git commit -m "Squelette FastAPI minimal + route / Hello"
git checkout main
git merge fastapi-init
git branch -d fastapi-init
```

---

## 3) CONFIGURATION & BASE DE DONNÉES (SQLite + SQLAlchemy)

### Rappel (question)

Configurer SQLite (`sqlite:///data/app.db`), créer un modèle `Item(id, name, price, quantity, created_at)` et initialiser la base.

### Correction détaillée

#### Créer la branche

```bash
git branch db-setup
git checkout db-setup
```

#### Fichiers de config et DB

`config.py` (à la racine)

```python
import os
from dotenv import load_dotenv

load_dotenv()

class Settings:
    DATABASE_URL = os.getenv("DATABASE_URL", "sqlite:///data/app.db")
    SECRET_KEY = os.getenv("SECRET_KEY", "dev-key")

settings = Settings()
```

`app/db.py`

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase
from config import settings

engine = create_engine(
    settings.DATABASE_URL,
    connect_args={"check_same_thread": False} if settings.DATABASE_URL.startswith("sqlite") else {}
)
SessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)

class Base(DeclarativeBase):
    pass

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

`app/models.py`

```python
from sqlalchemy import Column, Integer, String, Float, DateTime
from datetime import datetime
from .db import Base

class Item(Base):
    __tablename__ = "items"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(120), unique=True, nullable=False, index=True)
    price = Column(Float, nullable=False, default=0.0)
    quantity = Column(Integer, nullable=False, default=0)
    created_at = Column(DateTime, nullable=False, default=datetime.utcnow)
```

`app/schemas.py` (Pydantic pour l’API)

```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class ItemCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = 0.0
    quantity: int = 0

class ItemUpdate(BaseModel):
    name: Optional[str] = None
    price: Optional[float] = None
    quantity: Optional[int] = None

class ItemOut(BaseModel):
    id: int
    name: str
    price: float
    quantity: int
    created_at: datetime
```

`app/main.py` (mettre à jour pour créer les tables)

```python
from fastapi import FastAPI
from fastapi.responses import PlainTextResponse
from .db import engine, Base

app = FastAPI(title="Inventaire - FastAPI")

# Crée les tables à l'initialisation (simple et pédagogique)
Base.metadata.create_all(bind=engine)

@app.get("/", response_class=PlainTextResponse)
def root():
    return "Hello, FastAPI!"
```

#### Git (ajout → commit → merge → suppression de branche)

```bash
git add config.py app/db.py app/models.py app/schemas.py app/main.py data/
git commit -m "Config DB SQLite + SQLAlchemy, modèle Item, création des tables"
git checkout main
git merge db-setup
git branch -d db-setup
```

---

## 4) VUES HTML – CRUD COMPLET SUR ITEM (Jinja2)

> FastAPI permet de servir des templates via Starlette/Jinja2.

### Rappel (question)

Implémenter les pages HTML : liste, création, détail, édition, suppression (via formulaires).

### Correction détaillée

#### Créer la branche

```bash
git branch crud-html
git checkout crud-html
```

#### Mettre en place les templates

`app/views.py`

```python
from fastapi import APIRouter, Request, Depends, Form
from fastapi.responses import RedirectResponse
from fastapi.templating import Jinja2Templates
from sqlalchemy.orm import Session
from .db import get_db
from .models import Item

router = APIRouter()
templates = Jinja2Templates(directory="templates")

@router.get("/items")
def items_list(request: Request, db: Session = Depends(get_db)):
    items = db.query(Item).order_by(Item.id.desc()).all()
    return templates.TemplateResponse("items_list.html", {"request": request, "items": items})

@router.get("/items/new")
def item_new(request: Request):
    return templates.TemplateResponse("item_create.html", {"request": request})

@router.post("/items")
def item_create(
    name: str = Form(...), price: float = Form(...), quantity: int = Form(...),
    db: Session = Depends(get_db)
):
    name = name.strip()
    if not name:
        return RedirectResponse(url="/items/new", status_code=303)
    item = Item(name=name, price=price, quantity=quantity)
    db.add(item)
    db.commit()
    return RedirectResponse(url="/items", status_code=303)

@router.get("/items/{item_id}")
def item_detail(item_id: int, request: Request, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        return RedirectResponse(url="/items", status_code=303)
    return templates.TemplateResponse("item_detail.html", {"request": request, "item": item})

@router.get("/items/{item_id}/edit")
def item_edit_form(item_id: int, request: Request, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        return RedirectResponse(url="/items", status_code=303)
    return templates.TemplateResponse("item_edit.html", {"request": request, "item": item})

@router.post("/items/{item_id}/edit")
def item_edit(
    item_id: int,
    name: str = Form(...), price: float = Form(...), quantity: int = Form(...),
    db: Session = Depends(get_db)
):
    item = db.query(Item).get(item_id)
    if not item:
        return RedirectResponse(url="/items", status_code=303)

    name = name.strip()
    if not name:
        return RedirectResponse(url=f"/items/{item_id}/edit", status_code=303)

    item.name = name
    item.price = price
    item.quantity = quantity
    db.commit()
    return RedirectResponse(url=f"/items/{item_id}", status_code=303)

@router.post("/items/{item_id}/delete")
def item_delete(item_id: int, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if item:
        db.delete(item)
        db.commit()
    return RedirectResponse(url="/items", status_code=303)
```

`app/main.py` (enregistrer le routeur HTML et une route d’accueil qui redirige)

```python
from fastapi import FastAPI
from fastapi.responses import RedirectResponse
from .db import engine, Base
from .views import router as html_router

app = FastAPI(title="Inventaire - FastAPI")
Base.metadata.create_all(bind=engine)

@app.get("/")
def root():
    return RedirectResponse(url="/items")

app.include_router(html_router)
```

Templates :

`templates/base.html`

```html
<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>Inventaire - FastAPI</title>
  <link rel="stylesheet" href="/static/style.css">
</head>
<body>
<header>
  <h1>Inventaire</h1>
  <nav>
    <a href="/items">Liste</a> |
    <a href="/items/new">Nouveau</a>
  </nav>
</header>
<main>
  {% block content %}{% endblock %}
</main>
</body>
</html>
```

`templates/items_list.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Liste des articles</h2>
<table>
  <tr><th>ID</th><th>Nom</th><th>Prix</th><th>Quantité</th><th>Actions</th></tr>
  {% for it in items %}
  <tr>
    <td>{{ it.id }}</td>
    <td><a href="/items/{{ it.id }}">{{ it.name }}</a></td>
    <td>{{ '%.2f'|format(it.price) }}</td>
    <td>{{ it.quantity }}</td>
    <td>
      <a href="/items/{{ it.id }}/edit">Modifier</a>
      <form method="post" action="/items/{{ it.id }}/delete" style="display:inline">
        <button type="submit">Supprimer</button>
      </form>
    </td>
  </tr>
  {% endfor %}
</table>
{% endblock %}
```

`templates/item_create.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Créer un article</h2>
<form method="post" action="/items">
  <label>Nom</label><input name="name" required>
  <label>Prix</label><input name="price" type="number" step="0.01" required>
  <label>Quantité</label><input name="quantity" type="number" required>
  <button type="submit">Créer</button>
</form>
{% endblock %}
```

`templates/item_detail.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Détail de l'article #{{ item.id }}</h2>
<p><strong>Nom :</strong> {{ item.name }}</p>
<p><strong>Prix :</strong> {{ '%.2f'|format(item.price) }}</p>
<p><strong>Quantité :</strong> {{ item.quantity }}</p>
<p><a href="/items/{{ item.id }}/edit">Modifier</a></p>
{% endblock %}
```

`templates/item_edit.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Modifier l'article #{{ item.id }}</h2>
<form method="post" action="/items/{{ item.id }}/edit">
  <label>Nom</label><input name="name" value="{{ item.name }}" required>
  <label>Prix</label><input name="price" type="number" step="0.01" value="{{ item.price }}" required>
  <label>Quantité</label><input name="quantity" type="number" value="{{ item.quantity }}" required>
  <button type="submit">Enregistrer</button>
</form>
{% endblock %}
```

`static/style.css`

```css
body { font-family: system-ui, Arial, sans-serif; margin: 24px; }
header h1 { margin: 0 0 8px; }
nav a { margin-right: 8px; }
table { border-collapse: collapse; width: 100%; }
th, td { border: 1px solid #ddd; padding: 8px; }
label { display: block; margin-top: 8px; }
button { margin-top: 8px; }
```

**Servir les fichiers statiques** : ajoutez dans `app/main.py` :

```python
from fastapi.staticfiles import StaticFiles
app.mount("/static", StaticFiles(directory="static"), name="static")
```

#### Git

```bash
git add app/views.py app/main.py templates/ static/style.css
git commit -m "CRUD HTML complet (list/new/detail/edit/delete) via Jinja2"
git checkout main
git merge crud-html
git branch -d crud-html
```

---

## 5) API JSON – CRUD (Endpoints REST)

### Rappel (question)

Créer un routeur `api` avec les endpoints JSON : GET/POST `/api/items`, GET/PUT/DELETE `/api/items/{id}`, avec validations et erreurs.

### Correction détaillée

#### Créer la branche

```bash
git branch crud-api
git checkout crud-api
```

#### Fichier `app/api.py`

```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from .db import get_db
from .models import Item
from .schemas import ItemCreate, ItemUpdate, ItemOut

api = APIRouter(prefix="/api", tags=["api"])

def to_out(item: Item) -> ItemOut:
    return ItemOut(
        id=item.id, name=item.name, price=item.price,
        quantity=item.quantity, created_at=item.created_at
    )

@api.get("/items", response_model=list[ItemOut])
def list_items(db: Session = Depends(get_db)):
    items = db.query(Item).all()
    return [to_out(i) for i in items]

@api.post("/items", response_model=ItemOut, status_code=201)
def create_item(payload: ItemCreate, db: Session = Depends(get_db)):
    name = payload.name.strip()
    if not name:
        raise HTTPException(status_code=400, detail="Le champ 'name' est requis.")
    # Unicité simple
    if db.query(Item).filter_by(name=name).first():
        raise HTTPException(status_code=400, detail="Nom déjà utilisé.")
    item = Item(name=name, price=payload.price, quantity=payload.quantity)
    db.add(item)
    db.commit()
    db.refresh(item)
    return to_out(item)

@api.get("/items/{item_id}", response_model=ItemOut)
def get_item(item_id: int, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item non trouvé")
    return to_out(item)

@api.put("/items/{item_id}", response_model=ItemOut)
def update_item(item_id: int, payload: ItemUpdate, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item non trouvé")

    if payload.name is not None:
        name = payload.name.strip()
        if not name:
            raise HTTPException(status_code=400, detail="Le nom ne peut pas être vide.")
        # vérifier unicité si changé
        if name != item.name and db.query(Item).filter_by(name=name).first():
            raise HTTPException(status_code=400, detail="Nom déjà utilisé.")
        item.name = name
    if payload.price is not None:
        try:
            item.price = float(payload.price)
        except Exception:
            raise HTTPException(status_code=400, detail="Prix invalide.")
    if payload.quantity is not None:
        try:
            item.quantity = int(payload.quantity)
        except Exception:
            raise HTTPException(status_code=400, detail="Quantité invalide.")

    db.commit()
    db.refresh(item)
    return to_out(item)

@api.delete("/items/{item_id}")
def delete_item(item_id: int, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item non trouvé")
    db.delete(item)
    db.commit()
    return {"deleted": True, "id": item_id}
```

`app/main.py` (enregistrer l’API)

```python
from .api import api
app.include_router(api)
```

#### Git

```bash
git add app/api.py app/main.py
git commit -m "API JSON CRUD (GET/POST/GET/PUT/DELETE) pour Item"
git checkout main
git merge crud-api
git branch -d crud-api
```

---

## 6) SCRIPTS UTILITAIRES : SEED & EXPORT JSON

### Rappel (question)

Créer `scripts/seed.py` (insère des items) et `scripts/export_json.py` (export en `data/items_export.json`).

### Correction détaillée

#### Créer la branche

```bash
git branch scripts-utils
git checkout scripts-utils
```

`scripts/seed.py`

```python
from app.db import SessionLocal
from app.models import Item

items_to_insert = [
    ("Stylo", 1.50, 100),
    ("Cahier", 3.90, 50),
    ("Clé USB", 12.50, 20),
]

db = SessionLocal()
try:
    for name, price, qty in items_to_insert:
        if not db.query(Item).filter_by(name=name).first():
            db.add(Item(name=name, price=price, quantity=qty))
    db.commit()
    print("Seed terminé.")
finally:
    db.close()
```

`scripts/export_json.py`

```python
import json, os
from app.db import SessionLocal
from app.models import Item

os.makedirs("data", exist_ok=True)

db = SessionLocal()
try:
    items = db.query(Item).all()
    payload = [{"id": it.id, "name": it.name, "price": it.price, "quantity": it.quantity} for it in items]
    with open("data/items_export.json", "w", encoding="utf-8") as f:
        json.dump(payload, f, ensure_ascii=False, indent=2)
    print("Export effectué vers data/items_export.json")
finally:
    db.close()
```

Exécution :

```bash
python scripts/seed.py
python scripts/export_json.py
```

#### Git

```bash
git add scripts/seed.py scripts/export_json.py
git commit -m "Scripts utilitaires: seed + export JSON"
git checkout main
git merge scripts-utils
git branch -d scripts-utils
```

---

## 7) TESTS AUTOMATISÉS (pytest + httpx)

### Rappel (question)

Créer `tests/test_api.py` qui teste tout le cycle CRUD via l’API.

### Correction détaillée

#### Créer la branche

```bash
git branch tests-api
git checkout tests-api
```

`tests/test_api.py`

```python
import pytest
from httpx import AsyncClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from app.db import Base, get_db
from app.main import app

# DB de test en mémoire
TEST_SQLITE_URL = "sqlite+pysqlite:///:memory:"
engine = create_engine(TEST_SQLITE_URL, connect_args={"check_same_thread": False})
TestingSessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)

# Override de dépendance FastAPI
def override_get_db():
    db = TestingSessionLocal()
    try:
        yield db
    finally:
        db.close()

app.dependency_overrides[get_db] = override_get_db

@pytest.mark.asyncio
async def test_crud_cycle():
    # Créer les tables en mémoire
    Base.metadata.create_all(bind=engine)

    async with AsyncClient(app=app, base_url="http://test") as client:
        # CREATE
        r = await client.post("/api/items", json={"name":"Test","price":9.9,"quantity":3})
        assert r.status_code == 201, r.text
        item = r.json()
        iid = item["id"]

        # LIST
        r = await client.get("/api/items")
        assert r.status_code == 200
        assert any(x["id"] == iid for x in r.json())

        # DETAIL
        r = await client.get(f"/api/items/{iid}")
        assert r.status_code == 200

        # UPDATE
        r = await client.put(f"/api/items/{iid}", json={"price": 11.0})
        assert r.status_code == 200
        assert r.json()["price"] == 11.0

        # DELETE
        r = await client.delete(f"/api/items/{iid}")
        assert r.status_code == 200
        assert r.json()["deleted"] is True
```

Exécuter :

```bash
pytest -q
```

#### Git

```bash
git add tests/test_api.py
git commit -m "Tests: cycle CRUD JSON FastAPI (httpx + pytest)"
git checkout main
git merge tests-api
git branch -d tests-api
```

---

## 8) CONFLIT GIT VOLONTAIRE (TEMPLATES)

### Rappel (question)

Deux branches modifient la **même** portion de `templates/base.html`, on fusionne pour créer un conflit, on le résout, on explique dans `analyse.txt`.

### Correction (exemple)

```bash
git branch ux-bouton-vert
git checkout ux-bouton-vert
```

Modifier une même ligne dans `templates/base.html` :

```html
<a class="btn btn-vert" href="/items/new">Nouveau (Vert)</a>
```

Commit :

```bash
git add templates/base.html
git commit -m "UX: bouton vert"
git checkout main
```

Deuxième branche :

```bash
git branch ux-bouton-bleu
git checkout ux-bouton-bleu
```

Modifier le **même bloc** :

```html
<a class="btn btn-bleu" href="/items/new">Nouveau (Bleu)</a>
```

Commit :

```bash
git add templates/base.html
git commit -m "UX: bouton bleu"
git checkout main
```

Fusion + conflit :

```bash
git merge ux-bouton-vert
git merge ux-bouton-bleu
# Conflit sur templates/base.html
```

**Résolution conseillée** (unifier) :

```html
<a class="btn" href="/items/new">Nouveau</a>
```

Terminer :

```bash
git add templates/base.html
git commit -m "Résolution conflit: unifie le bouton (.btn)"
git branch -d ux-bouton-vert
git branch -d ux-bouton-bleu
```

Dans `analyse.txt`: expliquer que le conflit vient d’éditions concurrentes du même bloc ; stratégie d’unification.

---

## 9) VARIABLES D’ENVIRONNEMENT & CONFIG SÉCURISÉE

### Rappel (question)

Charger `.env` (non commité), lire `SECRET_KEY`/`DATABASE_URL` dans `config.py`, ignorer `.env` dans `.gitignore`.

### Correction détaillée

#### Créer la branche

```bash
git branch env-config
git checkout env-config
```

`.env` (ne pas committer)

```
SECRET_KEY=change-moi
DATABASE_URL=sqlite:///data/app.db
```

Vérifier `.gitignore` contient déjà `.env`. `config.py` lit déjà via `dotenv`.

#### Git

```bash
git add .gitignore
git commit -m "Sécurité: .env ignoré"
git checkout main
git merge env-config
git branch -d env-config
```

---

## 10) QUESTIONS FINALES (à mettre dans `analyse.txt`)

**1. Pourquoi séparer HTML (templates) et API (JSON) ?**

* HTML → pages utilisateurs (navigateurs).
* API JSON → données pour frontends JS, apps mobiles, automatisations.
* Avantages : séparation des responsabilités, réutilisation, tests plus simples.

**2. Exemple de validation côté serveur (POST /api/items)**

* Vérifier `name` non vide ; `price` est un `float` ; `quantity` un `int` ; `name` unique.
* En cas d’erreur : renvoyer `HTTP 400` avec un message explicite.

**3. Restaurer un template supprimé depuis l’historique**

```bash
git log --oneline -- templates/base.html
git checkout main -- templates/base.html
# ou
git checkout <commit_hash> -- templates/base.html
git add templates/base.html
git commit -m "Restauration du template depuis l'historique"
```

---

## 11) VÉRIFICATION FINALE

Exécuter :

```bash
git log --oneline --graph
```

Coller la sortie telle quelle dans le rapport.

---

## 12) SCÉNARIO RÉALISTE – PANNE API & RÉCUPÉRATION

### Rappel (question)

La route `PUT /api/items/{id}` a été supprimée. Créer `hotfix-api`, faire échouer les tests, restaurer **uniquement** la portion manquante depuis l’historique, committer.

### Correction détaillée

```bash
git branch hotfix-api
git checkout hotfix-api
pytest -q   # échec attendu si PUT manque
git log --oneline -- app/api.py
git show <hash>:app/api.py > /tmp/api_old.py   # visualiser ancienne version
# copier UNIQUEMENT la fonction PUT depuis /tmp/api_old.py vers app/api.py
git add app/api.py
git commit -m "Hotfix: restauration de la route PUT /api/items/{id}"
pytest -q   # doit repasser
git checkout main
git merge hotfix-api
git branch -d hotfix-api
```

**Extrait PUT attendu (à coller dans le rapport) :**

```python
@api.put("/items/{item_id}", response_model=ItemOut)
def update_item(item_id: int, payload: ItemUpdate, db: Session = Depends(get_db)):
    item = db.query(Item).get(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item non trouvé")

    if payload.name is not None:
        name = payload.name.strip()
        if not name:
            raise HTTPException(status_code=400, detail="Le nom ne peut pas être vide.")
        if name != item.name and db.query(Item).filter_by(name=name).first():
            raise HTTPException(status_code=400, detail="Nom déjà utilisé.")
        item.name = name
    if payload.price is not None:
        try:
            item.price = float(payload.price)
        except Exception:
            raise HTTPException(status_code=400, detail="Prix invalide.")
    if payload.quantity is not None:
        try:
            item.quantity = int(payload.quantity)
        except Exception:
            raise HTTPException(status_code=400, detail="Quantité invalide.")

    db.commit()
    db.refresh(item)
    return ItemOut(
        id=item.id, name=item.name, price=item.price,
        quantity=item.quantity, created_at=item.created_at
    )
```

---

## 13) LIVRABLES À FOURNIR

Un seul fichier :

```
tp_fastapi_crud_NOM_PRENOM.docx  (ou .pdf, .txt ou .py)
```

Inclure :

* **code clés** (app/main.py, app/models.py, app/api.py, app/views.py, templates/\*, etc.)
* **commandes Git** par étape (branch, checkout, add, commit, merge, branch -d)
* **résolution de conflit** (explication)
* **réponses aux questions finales**
* **sortie de** `git log --oneline --graph`

---

### Récapitulatif des séquences Git (modèle à répéter)

```bash
git branch <nom_branche>
git checkout <nom_branche>

# ... modifications ...
git add <fichiers>
git commit -m "<message>"

git checkout main
git merge <nom_branche>
git branch -d <nom_branche>
```

