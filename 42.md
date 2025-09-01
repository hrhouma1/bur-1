# TRAVAIL PRATIQUE – APPLICATION FLASK (CRUD) & GESTION AVEC GIT — CORRECTION COMPLÈTE

- **Nom de l'étudiant** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ 
- Groupe : \_\_\_\_\_\_\_\_\_\_\_



## 1) INITIALISATION DU PROJET

### Question (rappel)

Créer un projet Flask, un environnement virtuel, un `requirements.txt`, initialiser Git et faire un premier commit avec `README.md`.

### Correction détaillée

#### 1.1. Créer la structure de base

```bash
# Créer le dossier du projet et s'y déplacer
mkdir projet_flask_crud
cd projet_flask_crud
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

#### 1.3. Fichier `requirements.txt` (versions stables et pédagogiques)

Créez `requirements.txt` :

```
Flask==3.0.0
Flask-SQLAlchemy==3.1.1
python-dotenv==1.0.1
pytest==8.2.0
```

Installez :

```bash
pip install -r requirements.txt
```

#### 1.4. Fichier `README.md`

```bash
printf "Application Flask CRUD (Items) + API + Tests + Git.\n" > README.md
```

#### 1.5. `.gitignore` minimal (à la racine)

Créez `.gitignore` :

```
venv/
.env
__pycache__/
*.pyc
instance/
data/*.db*
data/items_export.json
```

#### 1.6. Initialiser Git et premier commit

```bash
git init
git add README.md requirements.txt .gitignore
git commit -m "Initialisation du projet Flask CRUD"
```

#### 1.7. (Optionnel) Fichier modèle `git_commands.txt`

```bash
printf "git branch <nom_branche>\n\
git checkout <nom_branche>\n\
git add <chemin>\n\
git commit -m \"<message>\"\n\
git checkout main\n\
git merge <nom_branche>\n\
git branch -d <nom_branche>\n" > git_commands.txt
git add git_commands.txt
git commit -m "Ajout du modèle git_commands.txt"
```

> ✅ **Rappel règle Git** : **à chaque étape** ci-dessous, on suit **exactement** l’ordre (branch → checkout → add → commit → checkout main → merge → branch -d).

---

## 2) SQUELETTE FLASK MINIMAL

### Question (rappel)

Créer `app/`, `__init__.py`, `routes.py`, `run.py`. La route `/` doit afficher `Hello, Flask!`.

### Correction détaillée

#### Commandes Git (d’abord créer la branche)

```bash
git branch flask-init
git checkout flask-init
```

#### Arborescence

```
app/
  __init__.py
  routes.py
run.py
```

#### `app/__init__.py`

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# Objet SQLAlchemy partagé dans l'app
db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    # Config minimale par défaut (sera surchargée plus tard)
    app.config.from_object('config.Config')

    # Initialiser l'ORM
    db.init_app(app)

    # Enregistrer les blueprints et créer la base si besoin
    with app.app_context():
        # Import tardif pour éviter les import cycles
        try:
            from .models import Item  # sera créé plus tard
        except Exception:
            pass
        try:
            from .routes import main_bp
            app.register_blueprint(main_bp)
        except Exception:
            pass
        # La base sera effectivement créée à l'étape DB-setup
    return app
```

#### `app/routes.py`

```python
from flask import Blueprint

main_bp = Blueprint('main', __name__)

@main_bp.route("/")
def index():
    return "Hello, Flask!"
```

#### `run.py`

```python
from app import create_app

app = create_app()

if __name__ == "__main__":
    app.run(debug=True)  # http://127.0.0.1:5000/
```

#### Git : ajouter, valider, fusionner

```bash
git add app/__init__.py app/routes.py run.py
git commit -m "Squelette Flask minimal avec route /"
git checkout main
git merge flask-init
git branch -d flask-init
```

---

## 3) CONFIGURATION & BASE DE DONNÉES (SQLite + SQLAlchemy)

### Question (rappel)

Créer `config.py`, `app/models.py`, configurer `SQLALCHEMY_DATABASE_URI=sqlite:///data/app.db`, modèle `Item(id, name, price, quantity, created_at)`, création automatique des tables.

### Correction détaillée

#### Créer la branche

```bash
git branch db-setup
git checkout db-setup
```

#### Créer le dossier `data/`

```bash
mkdir -p data
```

#### `config.py` (à la racine)

```python
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
    SECRET_KEY = os.environ.get("SECRET_KEY", "dev-key")
    SQLALCHEMY_DATABASE_URI = os.environ.get("DATABASE_URL", "sqlite:///data/app.db")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

#### `app/models.py`

```python
from datetime import datetime
from . import db  # récupère l'objet db défini dans app/__init__.py

class Item(db.Model):
    __tablename__ = "items"
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(120), nullable=False, unique=True)
    price = db.Column(db.Float, nullable=False, default=0.0)
    quantity = db.Column(db.Integer, nullable=False, default=0)
    created_at = db.Column(db.DateTime, nullable=False, default=datetime.utcnow)

    def __repr__(self):
        return f"<Item {self.id} {self.name}>"
```

#### Mettre à jour `app/__init__.py` pour créer la base

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    app.config.from_object('config.Config')
    db.init_app(app)

    with app.app_context():
        from .models import Item
        db.create_all()  # crée data/app.db et la table items si absents

        from .routes import main_bp
        app.register_blueprint(main_bp)

        # L'API sera ajoutée plus tard
    return app
```

#### Git : ajouter, valider, fusionner

```bash
git add config.py app/models.py app/__init__.py data/
git commit -m "Config SQLAlchemy + modèle Item + création automatique de la base"
git checkout main
git merge db-setup
git branch -d db-setup
```

---

## 4) VUES HTML – CRUD COMPLET SUR ITEM

### Question (rappel)

Créer des routes HTML pour lister, créer, lire, modifier, supprimer un `Item` + templates Jinja.

### Correction détaillée

#### Créer la branche

```bash
git branch crud-html
git checkout crud-html
```

#### Arborescence

```
templates/
  base.html
  items_list.html
  item_create.html
  item_detail.html
  item_edit.html
static/
  style.css
```

Créez les dossiers :

```bash
mkdir -p templates static
```

#### `app/routes.py` (remplacer entièrement par)

```python
from flask import Blueprint, render_template, request, redirect, url_for, flash
from . import db
from .models import Item

main_bp = Blueprint('main', __name__)

@main_bp.route("/")
def index():
    # Rediriger vers la liste des items pour l'appli HTML
    return redirect(url_for('main.items_list'))

@main_bp.route("/items")
def items_list():
    items = Item.query.order_by(Item.id.desc()).all()
    return render_template("items_list.html", items=items)

@main_bp.route("/items/new")
def item_new():
    return render_template("item_create.html")

@main_bp.route("/items", methods=["POST"])
def item_create():
    name = request.form.get("name", "").strip()
    price = request.form.get("price", "0").strip()
    quantity = request.form.get("quantity", "0").strip()

    if not name:
        flash("Le nom est obligatoire.")
        return redirect(url_for("main.item_new"))

    try:
        price = float(price)
        quantity = int(quantity)
    except ValueError:
        flash("Prix ou quantité invalide.")
        return redirect(url_for("main.item_new"))

    item = Item(name=name, price=price, quantity=quantity)
    db.session.add(item)
    db.session.commit()
    flash("Article créé.")
    return redirect(url_for("main.items_list"))

@main_bp.route("/items/<int:item_id>")
def item_detail(item_id):
    item = Item.query.get_or_404(item_id)
    return render_template("item_detail.html", item=item)

@main_bp.route("/items/<int:item_id>/edit")
def item_edit_form(item_id):
    item = Item.query.get_or_404(item_id)
    return render_template("item_edit.html", item=item)

@main_bp.route("/items/<int:item_id>/edit", methods=["POST"])
def item_edit(item_id):
    item = Item.query.get_or_404(item_id)
    name = request.form.get("name", "").strip()
    price = request.form.get("price", "0").strip()
    quantity = request.form.get("quantity", "0").strip()

    if not name:
        flash("Le nom est obligatoire.")
        return redirect(url_for("main.item_edit_form", item_id=item_id))

    try:
        price = float(price)
        quantity = int(quantity)
    except ValueError:
        flash("Prix ou quantité invalide.")
        return redirect(url_for("main.item_edit_form", item_id=item_id))

    item.name = name
    item.price = price
    item.quantity = quantity
    db.session.commit()
    flash("Article modifié.")
    return redirect(url_for("main.item_detail", item_id=item_id))

@main_bp.route("/items/<int:item_id>/delete", methods=["POST"])
def item_delete(item_id):
    item = Item.query.get_or_404(item_id)
    db.session.delete(item)
    db.session.commit()
    flash("Article supprimé.")
    return redirect(url_for("main.items_list"))
```

#### `templates/base.html`

```html
<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>Inventaire - Flask CRUD</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
<header>
  <h1>Inventaire</h1>
  <nav>
    <a href="{{ url_for('main.items_list') }}">Liste</a> |
    <a href="{{ url_for('main.item_new') }}">Nouveau</a>
  </nav>
</header>
<main>
  {% with messages = get_flashed_messages() %}
    {% if messages %}
      <ul class="flash">
        {% for m in messages %}<li>{{ m }}</li>{% endfor %}
      </ul>
    {% endif %}
  {% endwith %}
  {% block content %}{% endblock %}
</main>
</body>
</html>
```

#### `templates/items_list.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Liste des articles</h2>
<table>
  <tr><th>ID</th><th>Nom</th><th>Prix</th><th>Quantité</th><th>Actions</th></tr>
  {% for it in items %}
  <tr>
    <td>{{ it.id }}</td>
    <td><a href="{{ url_for('main.item_detail', item_id=it.id) }}">{{ it.name }}</a></td>
    <td>{{ '%.2f'|format(it.price) }}</td>
    <td>{{ it.quantity }}</td>
    <td>
      <a href="{{ url_for('main.item_edit_form', item_id=it.id) }}">Modifier</a>
      <form method="post" action="{{ url_for('main.item_delete', item_id=it.id) }}" style="display:inline">
        <button type="submit">Supprimer</button>
      </form>
    </td>
  </tr>
  {% endfor %}
</table>
{% endblock %}
```

#### `templates/item_create.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Créer un article</h2>
<form method="post" action="{{ url_for('main.item_create') }}">
  <label>Nom</label><input name="name" required>
  <label>Prix</label><input name="price" type="number" step="0.01" required>
  <label>Quantité</label><input name="quantity" type="number" required>
  <button type="submit">Créer</button>
</form>
{% endblock %}
```

#### `templates/item_detail.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Détail de l'article #{{ item.id }}</h2>
<p><strong>Nom :</strong> {{ item.name }}</p>
<p><strong>Prix :</strong> {{ '%.2f'|format(item.price) }}</p>
<p><strong>Quantité :</strong> {{ item.quantity }}</p>
<p><a href="{{ url_for('main.item_edit_form', item_id=item.id) }}">Modifier</a></p>
{% endblock %}
```

#### `templates/item_edit.html`

```html
{% extends "base.html" %}
{% block content %}
<h2>Modifier l'article #{{ item.id }}</h2>
<form method="post" action="{{ url_for('main.item_edit', item_id=item.id) }}">
  <label>Nom</label><input name="name" value="{{ item.name }}" required>
  <label>Prix</label><input name="price" type="number" step="0.01" value="{{ item.price }}" required>
  <label>Quantité</label><input name="quantity" type="number" value="{{ item.quantity }}" required>
  <button type="submit">Enregistrer</button>
</form>
{% endblock %}
```

#### `static/style.css` (simple)

```css
body { font-family: system-ui, Arial, sans-serif; margin: 24px; }
header h1 { margin: 0 0 8px; }
nav a { margin-right: 8px; }
table { border-collapse: collapse; width: 100%; }
th, td { border: 1px solid #ddd; padding: 8px; }
.flash { background: #fff8c4; padding: 8px; }
label { display: block; margin-top: 8px; }
button { margin-top: 8px; }
```

#### Git : ajouter, valider, fusionner

```bash
git add app/routes.py templates/ static/style.css
git commit -m "CRUD HTML complet (list/new/detail/edit/delete) avec templates"
git checkout main
git merge crud-html
git branch -d crud-html
```

---

## 5) API JSON – ENDPOINTS CRUD

### Question (rappel)

Créer un Blueprint `api` avec endpoints JSON : `GET/POST /api/items`, `GET/PUT/DELETE /api/items/<id>`. Validation et erreurs 400/404.

### Correction détaillée

#### Créer la branche

```bash
git branch crud-api
git checkout crud-api
```

#### `app/api.py`

```python
from flask import Blueprint, jsonify, request
from . import db
from .models import Item

api_bp = Blueprint("api", __name__, url_prefix="/api")

def to_dict(item: Item):
    return {
        "id": item.id,
        "name": item.name,
        "price": item.price,
        "quantity": item.quantity,
        "created_at": item.created_at.isoformat()
    }

@api_bp.get("/items")
def api_list_items():
    items = Item.query.all()
    return jsonify([to_dict(i) for i in items]), 200

@api_bp.post("/items")
def api_create_item():
    data = request.get_json(silent=True) or {}
    name = (data.get("name") or "").strip()
    price = data.get("price", 0)
    quantity = data.get("quantity", 0)

    if not name:
        return jsonify({"error": "Le champ 'name' est requis."}), 400
    try:
        price = float(price)
        quantity = int(quantity)
    except (TypeError, ValueError):
        return jsonify({"error": "Champs 'price' ou 'quantity' invalides."}), 400

    item = Item(name=name, price=price, quantity=quantity)
    db.session.add(item)
    db.session.commit()
    return jsonify(to_dict(item)), 201

@api_bp.get("/items/<int:item_id>")
def api_get_item(item_id: int):
    item = Item.query.get_or_404(item_id)
    return jsonify(to_dict(item)), 200

@api_bp.put("/items/<int:item_id>")
def api_update_item(item_id: int):
    item = Item.query.get_or_404(item_id)
    data = request.get_json(silent=True) or {}

    if "name" in data:
        name = (data["name"] or "").strip()
        if not name:
            return jsonify({"error": "Le nom ne peut pas être vide."}), 400
        item.name = name

    if "price" in data:
        try:
            item.price = float(data["price"])
        except (TypeError, ValueError):
            return jsonify({"error": "Prix invalide."}), 400

    if "quantity" in data:
        try:
            item.quantity = int(data["quantity"])
        except (TypeError, ValueError):
            return jsonify({"error": "Quantité invalide."}), 400

    db.session.commit()
    return jsonify(to_dict(item)), 200

@api_bp.delete("/items/<int:item_id>")
def api_delete_item(item_id: int):
    item = Item.query.get_or_404(item_id)
    db.session.delete(item)
    db.session.commit()
    return jsonify({"deleted": True, "id": item_id}), 200
```

#### Enregistrer l’API dans `app/__init__.py`

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    app.config.from_object('config.Config')
    db.init_app(app)

    with app.app_context():
        from .models import Item
        db.create_all()

        from .routes import main_bp
        app.register_blueprint(main_bp)

        from .api import api_bp
        app.register_blueprint(api_bp)

    return app
```

#### Git : ajouter, valider, fusionner

```bash
git add app/api.py app/__init__.py
git commit -m "API JSON CRUD (GET/POST/GET/PUT/DELETE) pour Item"
git checkout main
git merge crud-api
git branch -d crud-api
```

---

## 6) SCRIPTS UTILITAIRES : SEED & EXPORT JSON

### Question (rappel)

Créer `scripts/seed.py` (insère des items) et `scripts/export_json.py` (export vers `data/items_export.json`).

### Correction détaillée

#### Créer la branche

```bash
git branch scripts-utils
git checkout scripts-utils
mkdir -p scripts
```

#### `scripts/seed.py`

```python
from app import create_app, db
from app.models import Item

app = create_app()

items_to_insert = [
    ("Stylo", 1.50, 100),
    ("Cahier", 3.90, 50),
    ("Clé USB", 12.50, 20),
]

with app.app_context():
    for name, price, qty in items_to_insert:
        if not Item.query.filter_by(name=name).first():
            db.session.add(Item(name=name, price=price, quantity=qty))
    db.session.commit()
    print("Seed terminé.")
```

Exécution :

```bash
python scripts/seed.py
```

#### `scripts/export_json.py`

```python
import json, os
from app import create_app
from app.models import Item

app = create_app()
os.makedirs("data", exist_ok=True)

with app.app_context():
    items = [
        {"id": it.id, "name": it.name, "price": it.price, "quantity": it.quantity}
        for it in Item.query.all()
    ]
    with open("data/items_export.json", "w", encoding="utf-8") as f:
        json.dump(items, f, ensure_ascii=False, indent=2)
    print("Export effectué vers data/items_export.json")
```

Exécution :

```bash
python scripts/export_json.py
```

#### Git : ajouter, valider, fusionner

```bash
git add scripts/seed.py scripts/export_json.py
git commit -m "Scripts utilitaires: seed + export JSON"
git checkout main
git merge scripts-utils
git branch -d scripts-utils
```

---

## 7) TESTS AUTOMATISÉS (pytest)

### Question (rappel)

Créer `tests/test_api.py` testant la séquence CRUD via l’API.

### Correction détaillée

#### Créer la branche

```bash
git branch tests-api
git checkout tests-api
mkdir -p tests
```

#### `tests/test_api.py`

```python
import pytest
from app import create_app, db
from app.models import Item

@pytest.fixture
def client():
    app = create_app()
    # Base en mémoire pour les tests
    app.config.update(TESTING=True, SQLALCHEMY_DATABASE_URI="sqlite:///:memory:")
    with app.app_context():
        db.create_all()
    return app.test_client()

def test_crud_cycle(client):
    # CREATE
    resp = client.post("/api/items", json={"name": "Test", "price": 9.9, "quantity": 3})
    assert resp.status_code == 201
    item = resp.get_json()
    iid = item["id"]

    # LIST
    resp = client.get("/api/items")
    assert resp.status_code == 200
    assert any(x["id"] == iid for x in resp.get_json())

    # DETAIL
    resp = client.get(f"/api/items/{iid}")
    assert resp.status_code == 200

    # UPDATE
    resp = client.put(f"/api/items/{iid}", json={"price": 11.0})
    assert resp.status_code == 200
    assert resp.get_json()["price"] == 11.0

    # DELETE
    resp = client.delete(f"/api/items/{iid}")
    assert resp.status_code == 200
    assert resp.get_json()["deleted"] is True
```

Exécution :

```bash
pytest -q
```

#### Git : ajouter, valider, fusionner

```bash
git add tests/test_api.py
git commit -m "Tests pytest: cycle CRUD JSON complet"
git checkout main
git merge tests-api
git branch -d tests-api
```

---

## 8) CONFLIT GIT VOLONTAIRE (TEMPLATES)

### Question (rappel)

Créer deux branches qui modifient **la même** partie de `templates/base.html` (bouton/texte), fusionner pour provoquer un conflit, le résoudre, expliquer dans `analyse.txt`.

### Correction détaillée

#### Branche 1

```bash
git branch ux-bouton-vert
git checkout ux-bouton-vert
```

Modifier **une même ligne** dans `templates/base.html` (par exemple le texte du bouton « Nouveau ») :

```html
<!-- Variante VERT -->
<a class="btn btn-vert" href="{{ url_for('main.item_new') }}">Nouveau (Vert)</a>
```

Commit :

```bash
git add templates/base.html
git commit -m "UX: bouton vert dans base.html"
git checkout main
```

#### Branche 2

```bash
git branch ux-bouton-bleu
git checkout ux-bouton-bleu
```

Modifier la **même zone** mais différemment :

```html
<!-- Variante BLEU -->
<a class="btn btn-bleu" href="{{ url_for('main.item_new') }}">Nouveau (Bleu)</a>
```

Commit :

```bash
git add templates/base.html
git commit -m "UX: bouton bleu dans base.html"
git checkout main
```

#### Fusion + Conflit

```bash
git merge ux-bouton-vert
git merge ux-bouton-bleu
# -> Conflit signalé par Git sur templates/base.html
```

Ouvrir `templates/base.html`, repérer les marqueurs :

```
<<<<<<< HEAD
... (version verte)
=======
... (version bleue)
>>>>>>> ux-bouton-bleu
```

**Résolution conseillée** (garder les deux styles avec une classe utilitaire) :

```html
<a class="btn" href="{{ url_for('main.item_new') }}">Nouveau</a>
```

Enregistrer, puis :

```bash
git add templates/base.html
git commit -m "Résolution conflit: unifie le bouton (classe .btn générique)"
git branch -d ux-bouton-vert
git branch -d ux-bouton-bleu
```

Dans `analyse.txt` (explication à fournir par l’étudiant) :

* Le conflit vient d’éditions concurrentes du **même bloc**.
* Stratégie : **supprimer les marqueurs**, choisir une **version unifiée** (classe `.btn`), **committer** la résolution.

---

## 9) VARIABLES D’ENVIRONNEMENT & CONFIG SÉCURISÉE

### Question (rappel)

Charger `.env` (non commité), lire `SECRET_KEY` dans `config.py`, ignorer `.env` dans `.gitignore`.

### Correction détaillée

#### Créer la branche

```bash
git branch env-config
git checkout env-config
```

#### `.env` (ne pas commiter)

```
FLASK_ENV=development
SECRET_KEY=change-moi-en-production
```

#### `.gitignore` (déjà présent, vérifier qu’il inclut `.env`)

```
venv/
.env
__pycache__/
*.pyc
instance/
data/*.db*
data/items_export.json
```

#### `config.py` (déjà prêt grâce à `dotenv` — rien à changer)

#### Git : ajouter, valider, fusionner

```bash
git add .gitignore
git commit -m "Sécu: .env ignoré par Git"
git checkout main
git merge env-config
git branch -d env-config
```

---

## 10) QUESTIONS FINALES (dans `analyse.txt`)

### Question (rappel)

1. Pourquoi séparer HTML (templates) et API (JSON) ?
2. Exemple de validation côté serveur lors d’un POST.
3. Commande Git pour **restaurer** un template supprimé par erreur.

### Correction (réponses modèles)

1. **Séparation HTML/API** :

   * **HTML (templates)** sert les pages pour les utilisateurs (navigateur).
   * **API (JSON)** sert les données pour d’autres clients (JS frontend, mobile, automatisation).
   * Séparer permet : responsabilités claires, tests plus simples, réutilisation (le même backend API peut servir plusieurs frontends), sécurité/quotas indépendants.

2. **Validation côté serveur (exemple POST /api/items)** :

   * Vérifier que `name` est **présent** et non vide.
   * Vérifier que `price` est un **float** et `quantity` un **int**.
   * En cas d’erreur → **HTTP 400** + message JSON explicite.
     *(C’est exactement ce qui est implémenté dans `api_create_item`.)*

3. **Restaurer un fichier supprimé depuis l’historique** (sans le recréer à la main) :

```bash
# Voir l'historique
git log --oneline -- templates/base.html

# Restaurer la dernière version connue du fichier depuis main (ou depuis un commit précis)
git checkout main -- templates/base.html
# ou (avec un hash précis)
git checkout <commit_hash> -- templates/base.html

git add templates/base.html
git commit -m "Restauration du template supprimé depuis l'historique"
```

---

## 11) VÉRIFICATION FINALE

### Question (rappel)

Exécuter :

```bash
git log --oneline --graph
```

et **coller le résultat**.

### Correction (guide)

* L’étudiant exécute la commande et **colle tel quel** la sortie finale.
* On doit y voir les branches fusionnées dans l’ordre des étapes.

---

## 12) SCÉNARIO RÉALISTE — PANNE API & RÉCUPÉRATION (hotfix)

### Question (rappel)

Une route `PUT /api/items/<id>` a été supprimée par erreur. Créer `hotfix-api`, faire échouer les tests, retrouver le commit où ça marchait, restaurer **uniquement** la portion manquante, commiter.

### Correction détaillée

#### 12.1. Créer la branche hotfix et constater l’échec

```bash
git branch hotfix-api
git checkout hotfix-api
pytest -q   # échec attendu si la route PUT manque
```

#### 12.2. Retrouver le commit où la route fonctionnait

```bash
# Afficher l'historique du fichier API
git log --oneline -- app/api.py

# Voir le contenu du fichier à un commit donné (remplacer <hash>)
git show <hash>:app/api.py
```

Repérez le bloc `@api_bp.put("/api/items/<int:item_id>") ...`.

#### 12.3. Restaurer **uniquement** la portion PUT (méthode sûre)

* Ouvrez `app/api.py` actuel dans votre éditeur.
* Ouvrez le contenu ancien via :

  ```bash
  git show <hash>:app/api.py > /tmp/api_old.py
  ```
* Copiez **uniquement** la route `PUT` depuis `/tmp/api_old.py` et **collez-la** au bon endroit dans `app/api.py`.

> Astuce Git avancée : vous pouvez aussi restaurer **temporairement** tout le fichier, récupérer la fonction, puis revenir :

```bash
# Restaurer le fichier complet depuis un commit (pour inspection)
git checkout <hash> -- app/api.py
# Copier la fonction PUT quelque part (éditeur), puis
git restore app/api.py   # annule la restauration complète
# Coller uniquement la fonction PUT au bon endroit
```

#### 12.4. Commit du correctif et re-tests

```bash
git add app/api.py
git commit -m "Hotfix: restauration de la route PUT /api/items/<id>"
pytest -q  # doit repasser
git checkout main
git merge hotfix-api
git branch -d hotfix-api
```

**Extrait attendu (à coller dans la copie) :**

```python
# Coller ici uniquement la portion restaurée (PUT /api/items/<id>) :

@api_bp.put("/api/items/<int:item_id>")
def api_update_item(item_id: int):
    item = Item.query.get_or_404(item_id)
    data = request.get_json(silent=True) or {}

    if "name" in data:
        name = (data["name"] or "").strip()
        if not name:
            return jsonify({"error": "Le nom ne peut pas être vide."}), 400
        item.name = name

    if "price" in data:
        try:
            item.price = float(data["price"])
        except (TypeError, ValueError):
            return jsonify({"error": "Prix invalide."}), 400

    if "quantity" in data:
        try:
            item.quantity = int(data["quantity"])
        except (TypeError, ValueError):
            return jsonify({"error": "Quantité invalide."}), 400

    db.session.commit()
    return jsonify({
        "id": item.id, "name": item.name, "price": item.price,
        "quantity": item.quantity, "created_at": item.created_at.isoformat()
    }), 200
```

---

# ANNEXES — FICHIERS FINAUX RÉFÉRENCE (pour vérification)

### `run.py`

```python
from app import create_app
app = create_app()
if __name__ == "__main__":
    app.run(debug=True)
```

### `app/__init__.py`

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    app.config.from_object('config.Config')
    db.init_app(app)

    with app.app_context():
        from .models import Item
        db.create_all()

        from .routes import main_bp
        app.register_blueprint(main_bp)

        from .api import api_bp
        app.register_blueprint(api_bp)
    return app
```

### `app/models.py`

```python
from datetime import datetime
from . import db

class Item(db.Model):
    __tablename__ = "items"
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(120), nullable=False, unique=True)
    price = db.Column(db.Float, nullable=False, default=0.0)
    quantity = db.Column(db.Integer, nullable=False, default=0)
    created_at = db.Column(db.DateTime, nullable=False, default=datetime.utcnow)
```

### `app/routes.py`

*(version CRUD HTML donnée plus haut)*

### `app/api.py`

*(version CRUD JSON donnée plus haut)*

### `templates/*.html` et `static/style.css`

*(versions données plus haut)*

### `config.py`

```python
import os
from dotenv import load_dotenv
load_dotenv()

class Config:
    SECRET_KEY = os.environ.get("SECRET_KEY", "dev-key")
    SQLALCHEMY_DATABASE_URI = os.environ.get("DATABASE_URL", "sqlite:///data/app.db")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

### `scripts/seed.py` et `scripts/export_json.py`

*(versions données plus haut)*

### `tests/test_api.py`

*(version donnée plus haut)*

---

## RÉSUMÉ DES SÉQUENCES GIT (modèle à réutiliser pour **chaque** étape)

```bash
git branch <nom_branche>
git checkout <nom_branche>

# ... créations / modifications de fichiers ...
git add <fichiers>
git commit -m "<message clair>"

git checkout main
git merge <nom_branche>
git branch -d <nom_branche>
```

