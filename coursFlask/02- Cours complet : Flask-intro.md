# 📘 Cours complet : Flask pour grands débutants

---

## 1. C’est quoi Flask ?

👉 Imagine encore un **restaurant** :

* Les **clients** = les navigateurs ou applications qui envoient des requêtes.
* Le **serveur** = ton programme Flask.
* Le **menu** = les routes (`/`, `/hello`, `/users`).
* La **cuisine** = ton code Python qui prépare la réponse.
* Le **plat servi** = la réponse (du texte, du HTML ou du JSON).

⚡ **Flask** est un **framework Python très simple** qui permet de :

* créer des **sites web** (pages HTML),
* ou des **APIs** (réponses en JSON),
* avec très peu de code.

💡 FastAPI = moderne, validateur automatique.
💡 Flask = plus simple, plus libre, mais un peu plus manuel.

---

## 2. Installation de Flask

### Étape 1 : créer un dossier de projet

```bash
mkdir projet_flask
cd projet_flask
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

### Étape 3 : installer Flask

```bash
pip install Flask
```

---

## 3. Premier fichier `app.py`

Crée un fichier `app.py` :

```python
from flask import Flask

# On crée l’application Flask
app = Flask(__name__)

# Route GET sur "/"
@app.route("/")
def home():
    return "Bienvenue dans Flask !"
```

---

## 4. Lancer le serveur

Tape dans ton terminal :

```bash
flask --app app run --reload
```

⚡ Ouvre ton navigateur → [http://127.0.0.1:5000/](http://127.0.0.1:5000/)
👉 Tu vois :

```
Bienvenue dans Flask !
```

Félicitations 🎉 tu as ton premier site Flask.

---

## 5. Les routes Flask

Une **route** = une **adresse URL** + une **fonction**.

### Exemple 1 : route simple

```python
@app.route("/hello")
def hello():
    return "Bonjour Flask !"
```

👉 Va sur [http://127.0.0.1:5000/hello](http://127.0.0.1:5000/hello) → tu vois “Bonjour Flask !”

### Exemple 2 : route avec paramètre

```python
@app.route("/hello/<name>")
def hello_name(name):
    return f"Bonjour {name} !"
```

👉 [http://127.0.0.1:5000/hello/Ana](http://127.0.0.1:5000/hello/Ana) → “Bonjour Ana !”

---

## 6. Méthodes HTTP (GET, POST, etc.)

Par défaut, Flask accepte **GET**.
Mais tu peux autoriser d’autres méthodes :

```python
@app.route("/ping", methods=["GET", "POST"])
def ping():
    return {"pong": True}
```

---

## 7. Renvoyer du JSON

Avec Flask, on peut renvoyer un dictionnaire Python → il sera transformé en JSON.

```python
from flask import jsonify

@app.route("/api")
def api():
    data = {"status": "ok", "message": "Ceci est une API Flask"}
    return jsonify(data)
```

👉 [http://127.0.0.1:5000/api](http://127.0.0.1:5000/api) → tu vois du JSON.

---

## 8. Exemple CRUD complet (en mémoire)

Toujours dans `app.py` :

```python
from flask import Flask, request, jsonify, abort

app = Flask(__name__)

# Base de données "FAKE" en mémoire
items = {}
counter = 0

# GET - liste
@app.route("/items", methods=["GET"])
def list_items():
    return jsonify(list(items.values()))

# POST - création
@app.route("/items", methods=["POST"])
def create_item():
    global counter
    data = request.get_json()
    if not data or "name" not in data:
        abort(400, "Nom obligatoire")
    counter += 1
    item = {"id": counter, "name": data["name"], "price": data.get("price", 0), "quantity": data.get("quantity", 1)}
    items[counter] = item
    return jsonify(item), 201

# GET - détail
@app.route("/items/<int:item_id>", methods=["GET"])
def get_item(item_id):
    if item_id not in items:
        abort(404, "Item non trouvé")
    return jsonify(items[item_id])

# PUT - mise à jour
@app.route("/items/<int:item_id>", methods=["PUT"])
def update_item(item_id):
    if item_id not in items:
        abort(404, "Item non trouvé")
    data = request.get_json()
    item = items[item_id]
    item.update(data)
    return jsonify(item)

# DELETE - suppression
@app.route("/items/<int:item_id>", methods=["DELETE"])
def delete_item(item_id):
    if item_id not in items:
        abort(404, "Item non trouvé")
    del items[item_id]
    return jsonify({"ok": True})
```

---

## 9. Tester avec `curl` (optionnel)

```bash
# Créer un item
curl -X POST http://127.0.0.1:5000/items -H "Content-Type: application/json" -d '{"name":"Stylo","price":1.5,"quantity":10}'

# Lister
curl http://127.0.0.1:5000/items

# Lire
curl http://127.0.0.1:5000/items/1

# Supprimer
curl -X DELETE http://127.0.0.1:5000/items/1
```

---

## 10. Répétition pour que ça rentre

* `app = Flask(__name__)` → crée ton restaurant.
* `@app.route("/")` → écris un menu (une route).
* Une route = une adresse (`/items`) + une fonction (`def ...`).
* `return` → ce que tu sers au client (texte ou JSON).
* `request` → ce que le client envoie (paramètres, JSON, formulaires).
* CRUD = **Create, Read, Update, Delete** → POST, GET, PUT, DELETE.

---

## 11. Limite de Flask “pur”

👉 Flask **ne fournit pas** Swagger UI automatiquement (contrairement à FastAPI).
Mais il existe des extensions comme :

* `flask-restx`
* `flasgger`
* `flask-smorest`

---

# 🎯 Petit exercice pour toi

1. Ajoute une route `/ping` qui renvoie `{"pong": True}`
2. Ajoute une route `/hello/<name>` qui renvoie un JSON `{"message": "Bonjour <name>"}`
3. Ajoute une route `/status` qui renvoie un JSON `{"alive": True}`


