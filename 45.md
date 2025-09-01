# TRAVAIL PRATIQUE – APPLICATION **FASTAPI** (CRUD) & GESTION AVEC GIT

* **Nom de l'étudiant** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
* **Groupe** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

<br/>

### Consignes générales importantes

* **Chaque modification doit être suivie exactement des commandes Git suivantes dans l’ordre :**

  1. `git branch nom_branche`
  2. `git checkout nom_branche`
  3. `git add chemin/fichier_modifié`
  4. `git commit -m "Message clair expliquant la modification"`
  5. Retour sur `main` : `git checkout main`
  6. Fusion de branche : `git merge nom_branche`
  7. Suppression branche fusionnée : `git branch -d nom_branche`

* Chaque commande Git utilisée doit être **clairement inscrite** dans `git_commands.txt`.

* Le code **FastAPI** ira dans le dossier `app/`.

* Les scripts utilitaires (ex. seed, export) seront dans `scripts/`.

* La base SQLite et autres données seront dans `data/`.

* Utilisez un **environnement virtuel** et un fichier `requirements.txt`.

* Démarrage local recommandé : `uvicorn app.main:app --reload` (port 8000 par défaut).

---

## 1. INITIALISATION DU PROJET

**Emplacement :** à la racine de votre espace de travail.

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

➤ Exemple 1 :

```bash
Commande 1 : ___________________________________________   # Créer un dossier nommé projet_fastapi_crud (sans espaces)
Commande 2 : ___________________________________________   # Se déplacer dans le dossier
Commande 3 : ___________________________________________   # Créer et activer un venv (Windows / macOS-Linux, au choix)
Commande 4 : ___________________________________________   # Créer requirements.txt (fastapi, uvicorn, sqlmodel, jinja2, python-dotenv, pytest, httpx)
Commande 5 : ___________________________________________   # Initialiser un dépôt Git
Commande 6 : ___________________________________________   # Créer README.md (description courte du projet)
Commande 7 : ___________________________________________   # Premier commit: "Initialisation du projet FastAPI CRUD"
```

➤ Exemple 2 :

* Allez voir `exemple.md` dans le même dossier ici.

---

## 2. SQUELETTE **FASTAPI** MINIMAL

**Branche Git à créer :** `fastapi-init`

**Arborescence à créer :**

```
app/
  main.py          # point d'entrée FastAPI
  routes_html.py   # routes HTML (Jinja)
templates/
  base.html
static/
  style.css
```

**Objectif :**

* Créez une application FastAPI minimale qui affiche `Hello, FastAPI!` à la route `/` (HTML simple via Jinja).
* Lancement : `uvicorn app.main:app --reload`.

➤ Fichiers à compléter :

`app/main.py`

```python
# Créez l'instance FastAPI, montez les static, configurez Jinja2Templates,
# et incluez le router HTML (depuis routes_html.py).
................................................................................
```

`app/routes_html.py`

```python
# Créez un APIRouter pour les pages HTML.
# Définissez la route GET "/" qui rend un template affichant "Hello, FastAPI!"
................................................................................
```

`templates/base.html`

```html
<!-- Layout minimal avec bloc de contenu -->
................................................................................
```

`static/style.css`

```css
/* Style très simple */
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 3. CONFIGURATION & BASE DE DONNÉES (SQLite + **SQLModel**)

**Branche Git à créer :** `db-setup`

**Fichiers :**

```
config.py
app/database.py   # moteur SQLModel + Session
app/models.py     # modèle Item (SQLModel)
app/main.py       # à modifier pour initialiser la DB au démarrage
data/             # dossier pour la base SQLite (ex: data/app.db)
```

**Objectif :**

* Configurer `DATABASE_URL` vers `sqlite:///data/app.db` via `.env` ou valeur par défaut.
* Déclarer un modèle `Item` (id, name, price, quantity, created\_at).
* Créer la base (table) au démarrage si absente.

`config.py`

```python
# Définir une classe Config avec DATABASE_URL (sqlite:///data/app.db par défaut)
# Charger .env (python-dotenv) pour surcharger au besoin.
................................................................................
```

`app/database.py`

```python
# Déclarer l'engine SQLModel, un SessionLocal, et une fonction get_session() (yield).
................................................................................
```

`app/models.py`

```python
# Déclarer le modèle Item en SQLModel (table=True):
# id:int pk, name:str (unique?), price:float, quantity:int, created_at:datetime
................................................................................
```

`app/main.py` (mise à jour)

```python
# Au démarrage de l'appli, créer les tables (SQLModel.metadata.create_all(engine))
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 4. VUES **HTML** – CRUD COMPLET SUR ITEM (FastAPI + Jinja)

**Branche Git à créer :** `crud-html`

**Arborescence :**

```
app/
  routes_html.py   # à compléter
templates/
  items_list.html
  item_create.html
  item_detail.html
  item_edit.html
static/
  style.css        # à enrichir si besoin
```

**Objectif :**

* Implémenter **5 pages HTML** (avec `Jinja2Templates`) :

  * `GET /items` : liste simple
  * `GET /items/new` & `POST /items` : création
  * `GET /items/{item_id}` : détail
  * `GET /items/{item_id}/edit` & `POST /items/{item_id}/edit` : mise à jour
  * `POST /items/{item_id}/delete` : suppression

`app/routes_html.py` (compléter la logique avec `Request`, `Form`, `RedirectResponse`)

```python
# Ajouter les routes HTML pour lister, créer, lire, mettre à jour, supprimer Item
# Utiliser get_session() pour manipuler la DB.
................................................................................
```

`templates/items_list.html`

```html
<!-- Boucle sur la liste des items -->
................................................................................
```

`templates/item_create.html`

```html
<!-- Formulaire name, price, quantity -->
................................................................................
```

`templates/item_detail.html`

```html
<!-- Afficher les champs d'un Item + bouton supprimer -->
................................................................................
```

`templates/item_edit.html`

```html
<!-- Formulaire prérempli pour mise à jour -->
................................................................................
```

`static/style.css`

```css
/* Styles simples */
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 5. **API JSON** – CRUD (Endpoints REST, **FastAPI**)

**Branche Git à créer :** `crud-api`

**Arborescence :**

```
app/
  api.py          # APIRouter pour /api
  main.py         # à mettre à jour pour inclure l'API
```

**Objectif :**

* Créer un `APIRouter` `api` avec les endpoints (réponses **JSON**) :

  * `GET /api/items` → liste
  * `POST /api/items` → création (body JSON via Pydantic)
  * `GET /api/items/{id}` → détail
  * `PUT /api/items/{id}` → mise à jour
  * `DELETE /api/items/{id}` → suppression
* Gestion d’erreurs propre (404, 400) et validation minimale (Pydantic).

`app/api.py`

```python
# Définir APIRouter(prefix="/api")
# Implémenter les 5 endpoints CRUD en JSON avec Pydantic (schemas Create/Update/Read)
................................................................................
```

`app/main.py` (enregistrer le router API)

```python
# Inclure le router API dans app
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 6. SCRIPTS UTILITAIRES – SEED & EXPORT

**Branche Git à créer :** `scripts-utils`

**Fichiers :**

```
scripts/seed.py         # insère quelques items de démonstration
scripts/export_json.py  # exporte tous les items en JSON -> data/items_export.json
```

`scripts/seed.py`

```python
# Insérer N items (name, price, quantity) via SQLModel Session.
................................................................................
```

`scripts/export_json.py`

```python
# Exporter toutes les lignes Item au format JSON vers data/items_export.json.
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 7. TESTS AUTOMATISÉS (pytest + httpx)

**Branche Git à créer :** `tests-api`

**Arborescence :**

```
tests/
  test_api.py
```

**Objectif :**

* Tester au minimum : création (POST), lecture (GET), mise à jour (PUT), suppression (DELETE) d’un item via l’API (client httpx/`TestClient`).

`tests/test_api.py`

```python
# Créer un TestClient FastAPI et tester la séquence CRUD JSON (POST/GET/PUT/DELETE).
# Vous pouvez configurer une DB en mémoire pour les tests.
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 8. CONFLIT GIT VOLONTAIRE (TEMPLATES)

* Branche `ux-bouton-vert` (depuis `main`) : dans `templates/base.html`, modifiez le style d’un bouton (classe CSS, texte). Committez.
* Branche `ux-bouton-bleu` (depuis `main`) : modifiez la **même** section d’`base.html` avec un choix différent. Committez.
* Fusionnez ces deux branches dans `main` pour **provoquer un conflit**.
* Résolvez le conflit manuellement et expliquez votre résolution dans `analyse.txt`.

**Commandes Git à effectuer :** `branch`, `checkout`, `add`, `commit`, `checkout main`, `merge`, résoudre conflit, `branch -d`

➤ Votre réponse

```python
# Écrire ici les commandes Git (et l’explication dans analyse.txt) :
................................................................................
```

---

## 9. VARIABLES D’ENVIRONNEMENT & CONFIG SÉCURISÉE

**Branche Git à créer :** `env-config`

**Objectif :**

* Utiliser `python-dotenv` pour charger `.env` (ex: `ENV=development`, `DATABASE_URL=...`).
* Ne pas committer le `.env` (ajouter au `.gitignore`).
* Lire `DATABASE_URL` (et autres) dans `config.py`.

`.env` (non commité)

```
ENV=development
DATABASE_URL=sqlite:///data/app.db
```

`.gitignore`

```
# Ignorer venv, .env, fichiers temporaires, DB journaux, __pycache__, etc.
................................................................................
```

`config.py` (MAJ pour lire environ)

```python
# Charger environ et définir DATABASE_URL (+ autres clés si besoin).
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 10. QUESTIONS FINALES (`analyse.txt` à compléter précisément)

1. Pourquoi séparons-nous **routes HTML (Jinja)** et **API JSON** dans une app FastAPI ?
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

2. Donnez un exemple concret de **validation côté serveur** lors d’un POST JSON (champ manquant / invalide) avec **Pydantic**.
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

3. Donnez la commande Git pour **restaurer** un fichier de template supprimé accidentellement depuis l’historique :
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

## 11. VÉRIFICATION FINALE

Exécutez cette commande exactement dans votre dépôt à la fin du TP :

```bash
git log --oneline --graph
```

Copiez le résultat exact dans l’espace suivant :

```
# Collez ici le résultat exact de git log --oneline --graph :
................................................................................
```

---

## 12. SCÉNARIO RÉALISTE – PANNE API & RÉCUPÉRATION

### 12.1. Contexte

Un commit a **supprimé par erreur** la route **PUT `/api/items/{id}`**. Les tests `tests/test_api.py` échouent.

### 12.2. Étapes

1. À partir de `main`, créez la branche `hotfix-api`.
2. Lancez les tests (`pytest`) → échec attendu.
3. **Identifiez le commit** où la route PUT fonctionnait (`git log`, `git show`).
4. **Restaurez** uniquement la portion manquante depuis l’historique (sans recréer à la main si possible).
5. Commitez le correctif avec un message clair.

➤ Script / extrait corrigé dans `app/api.py` :

```python
# Coller ici uniquement la portion restaurée (PUT /api/items/{id}) :
................................................................................
```

➤ Commandes Git utilisées :

```bash
................................................................................
```

---

## 13. LIVRABLES À FOURNIR EXACTEMENT

Remettez **un seul fichier** nommé :

```
tp_fastapi_crud_NOM_PRENOM.docx  (ou .pdf, .txt ou .py)
```

Le rapport doit contenir :

* Le **code FastAPI** (fichiers clés) **et** les scripts utilitaires.
* Les **commandes Git** associées à chaque étape (branch, checkout, add, commit, merge, branch -d).
* La **résolution du conflit** (avec explication).
* Les **réponses aux questions finales**.
* Le **résultat complet de** :

  ```
  git log --oneline --graph
  ```

Aucun `.zip` ou dossier séparé n’est accepté. Le code et les commandes doivent être intégrés dans ce seul fichier.

---

## Annexe – **Modèles de contenu**

### `requirements.txt` (exemple)

```
fastapi==0.114.0
uvicorn==0.30.6
sqlmodel==0.0.22
Jinja2==3.1.4
python-dotenv==1.0.1
pytest==8.2.0
httpx==0.27.0
```

### `git_commands.txt` – Modèle

```
git branch <nom_branche>
git checkout <nom_branche>
git add <chemin>
git commit -m "<message>"
git checkout main
git merge <nom_branche>
git branch -d <nom_branche>
```

