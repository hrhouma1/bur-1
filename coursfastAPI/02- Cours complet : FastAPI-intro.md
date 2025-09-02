# 📘 Cours complet : FastAPI -intro



## 1. C’est quoi FastAPI ?

👉 Imagine que tu veux construire un **restaurant** :

* Les **clients** = utilisateurs qui envoient des requêtes.
* Le **serveur** = ton application (FastAPI).
* Le **menu** = les routes/endpoints (`/hello`, `/users`, etc.).
* La **cuisine** = ton code Python qui prépare la réponse.
* La **commande** = une requête HTTP (GET, POST, PUT, DELETE).
* Le **plat servi** = la réponse JSON (ou HTML).

⚡ **FastAPI** est un **framework Python** (un outil) qui sert à :

* Écrire des **APIs web** rapidement.
* **Valider les données** automatiquement.
* Générer de la **documentation Swagger** toute seule.

---

## 2. Première étape : installer FastAPI

### Étape 1 : créer un dossier

```bash
mkdir projet_fastapi
cd projet_fastapi
```

### Étape 2 : créer un environnement virtuel

Linux / macOS :

```bash
python3 -m venv venv
source venv/bin/activate
```

Windows :

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

### Étape 3 : installer FastAPI + Uvicorn

```bash
pip install fastapi uvicorn
```

👉 **FastAPI = la cuisine**
👉 **Uvicorn = le serveur qui allume la lumière et sert les plats**

---

## 3. Premier fichier `main.py`

Crée un fichier `main.py` :

```python
from fastapi import FastAPI

# On crée l'application FastAPI
app = FastAPI()

# Route GET simple
@app.get("/")
def lire_racine():
    return {"message": "Bienvenue dans FastAPI !"}
```

### Lancer le serveur :

```bash
uvicorn main:app --reload
```

⚡ Ouvre ton navigateur → [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
Tu vois :

```json
{"message": "Bienvenue dans FastAPI !"}
```

👉 Bravo, tu viens de construire ton premier restaurant 🥳

---

## 4. La documentation automatique

FastAPI crée automatiquement deux pages :

* Swagger UI → [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
* ReDoc → [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

💡 C’est magique : pas besoin d’écrire la doc toi-même.

---

## 5. Comprendre les méthodes HTTP (très important)

* **GET** : lire (demander quelque chose)
* **POST** : créer (ajouter quelque chose)
* **PUT** : remplacer (mettre à jour)
* **PATCH** : modifier partiellement
* **DELETE** : supprimer

---

## 6. Exemple CRUD complet (mémoire)

Toujours dans `main.py` :

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

# Base de données "FAKE" en mémoire
items = {}
counter = 0

# Modèle d'entrée (ce que l'utilisateur envoie)
class ItemIn(BaseModel):
    name: str
    price: float
    quantity: int

# Modèle de sortie (ce que l'API renvoie)
class ItemOut(ItemIn):
    id: int

# GET - Lire tous les items
@app.get("/items", response_model=list[ItemOut])
def list_items():
    return list(items.values())

# POST - Créer un item
@app.post("/items", response_model=ItemOut, status_code=201)
def create_item(item: ItemIn):
    global counter
    counter += 1
    new_item = ItemOut(id=counter, **item.model_dump())
    items[counter] = new_item
    return new_item

# GET - Lire un item spécifique
@app.get("/items/{item_id}", response_model=ItemOut)
def get_item(item_id: int):
    if item_id not in items:
        raise HTTPException(404, "Item non trouvé")
    return items[item_id]

# PUT - Mettre à jour un item
@app.put("/items/{item_id}", response_model=ItemOut)
def update_item(item_id: int, item: ItemIn):
    if item_id not in items:
        raise HTTPException(404, "Item non trouvé")
    updated = ItemOut(id=item_id, **item.model_dump())
    items[item_id] = updated
    return updated

# DELETE - Supprimer un item
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    if item_id not in items:
        raise HTTPException(404, "Item non trouvé")
    del items[item_id]
    return {"ok": True}
```

---

## 7. Tester dans Swagger

1. Lance le serveur :

```bash
uvicorn main:app --reload
```

2. Va sur [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
   👉 Tu vois tous tes endpoints (GET, POST, PUT, DELETE).
   👉 Tu peux cliquer dessus, entrer des valeurs, et tester.

---

## 8. Résumé pédagogique (répétition volontaire)

* `app = FastAPI()` → créer le restaurant
* `@app.get("/")` → écrire le menu (route GET)
* `return {...}` → servir un plat (réponse JSON)
* `pydantic.BaseModel` → définir le format de la commande
* `/docs` → Swagger automatique, menu interactif

---

## 9. Petit exercice pour toi

1. Ajoute une route `@app.get("/ping")` qui renvoie `{"pong": True}`
2. Ajoute une route `@app.get("/hello/{name}")` qui dit bonjour en JSON
   Exemple : `/hello/Ana` → `{"message": "Bonjour Ana"}`

