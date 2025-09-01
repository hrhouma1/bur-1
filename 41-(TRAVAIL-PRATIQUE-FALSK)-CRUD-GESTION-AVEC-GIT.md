# TRAVAIL PRATIQUE – APPLICATION FLASK (CRUD) & GESTION AVEC GIT

- **Nom de l'étudiant** : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ 
- Groupe : \_\_\_\_\_\_\_\_\_\_\_

<br/>

### Consignes générales importantes :

* **Chaque modification doit être suivie exactement des commandes Git suivantes dans l'ordre :**

  1. `git branch nom_branche`
  2. `git checkout nom_branche`
  3. `git add chemin/fichier_modifié`
  4. `git commit -m "Message clair expliquant la modification"`
  5. Retour sur `main` : `git checkout main`
  6. Fusion de branche : `git merge nom_branche`
  7. Suppression branche fusionnée : `git branch -d nom_branche`

* Chaque commande Git utilisée doit être **clairement inscrite** dans `git_commands.txt`.

* Le code Flask ira dans le dossier `app/`.

* Les scripts utilitaires (ex. seed, export) seront dans `scripts/`.

* La base SQLite et autres données seront dans `data/`.

* Utilisez un **environnement virtuel** et un fichier `requirements.txt`.



## 1. INITIALISATION DU PROJET

**Emplacement :** à la racine de votre espace de travail.

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

➤ Exemple 1 :

```bash
Commande 1 : ___________________________________________   # Créer un dossier nommé projet_flask_crud (sans espaces)
Commande 2 : ___________________________________________   # Se déplacer dans le dossier
Commande 3 : ___________________________________________   # Créer et activer un venv (Windows / macOS-Linux, au choix)
Commande 4 : ___________________________________________   # Créer requirements.txt (flask, flask_sqlalchemy, python-dotenv, pytest)
Commande 5 : ___________________________________________   # Initialiser un dépôt Git
Commande 6 : ___________________________________________   # Créer README.md (description courte du projet)
Commande 7 : ___________________________________________   # Premier commit: "Initialisation du projet Flask CRUD"
```

➤ Exemple 2 :

* Allez voir `exemple.md` dans le même dossier ici.

---

## 2. SQUELETTE FLASK MINIMAL

**Branche Git à créer :** `flask-init`

**Arborescence à créer :**

```
app/
  __init__.py
  routes.py
run.py
```

**Objectif :**

* Créez une application Flask minimale qui affiche `Hello, Flask!` à la route `/`.
* Lancement avec `python run.py`.

➤ Fichiers à compléter :

`app/__init__.py`

```python
# Créez et exposez une fonction create_app() retournant l'app Flask.
# Enregistrer les blueprints/Routes ici si nécessaire.
................................................................................
```

`app/routes.py`

```python
# Déclarer un Blueprint 'main' et définir la route '/' renvoyant "Hello, Flask!"
................................................................................
```

`run.py`

```python
# Importer create_app et lancer l'app en debug (port 5000)
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 3. CONFIGURATION & BASE DE DONNÉES (SQLite + SQLAlchemy)

**Branche Git à créer :** `db-setup`

**Fichiers :**

```
config.py
app/models.py
app/__init__.py  # à modifier pour initialiser SQLAlchemy
data/             # dossier pour la base SQLite (ex: data/app.db)
```

**Objectif :**

* Configurer `SQLALCHEMY_DATABASE_URI` vers `sqlite:///data/app.db`.
* Déclarer un modèle `Item` (id, name, price, quantity, created\_at).
* Créer la base (create\_all au démarrage si absente).

`config.py`

```python
# Définir la classe Config avec SQLALCHEMY_DATABASE_URI, SQLALCHEMY_TRACK_MODIFICATIONS=False
................................................................................
```

`app/models.py`

```python
# Déclarer db = SQLAlchemy() (ou importé de __init__), puis le modèle Item
# Champs : id (int, pk), name (str, unique?), price (float), quantity (int), created_at (datetime)
................................................................................
```

`app/__init__.py` (mise à jour)

```python
# Initialiser db, charger Config, créer les tables si besoin
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 4. VUES HTML – CRUD COMPLET SUR ITEM

**Branche Git à créer :** `crud-html`

**Arborescence :**

```
app/
  routes.py      # à compléter
templates/
  base.html
  items_list.html
  item_create.html
  item_detail.html
  item_edit.html
static/
  style.css
```

**Objectif :**

* Implémenter **5 routes HTML** :

  * `GET /items` : liste paginée simple
  * `GET /items/new` & `POST /items` : création
  * `GET /items/<int:item_id>` : détail
  * `GET /items/<int:item_id>/edit` & `POST /items/<int:item_id>/edit` : mise à jour
  * `POST /items/<int:item_id>/delete` : suppression
* Templates sobres (héritage de `base.html`).

`app/routes.py` (compléter les vues et la logique formulaire `request.form`)

```python
# Ajouter les routes HTML pour lister, créer, lire, mettre à jour, supprimer Item
................................................................................
```

`templates/base.html`

```html
<!-- Layout minimal avec un bloc de contenu -->
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

## 5. API JSON – CRUD (Endpoints REST)

**Branche Git à créer :** `crud-api`

**Objectif :**

* Créer un Blueprint `api` avec les endpoints suivants (réponses **JSON**):

  * `GET /api/items` → liste
  * `POST /api/items` → création (JSON body)
  * `GET /api/items/<id>` → détail
  * `PUT /api/items/<id>` → mise à jour
  * `DELETE /api/items/<id>` → suppression
* Gestion d’erreurs propre (404, 400) et validation minimale.

`app/api.py`

```python
# Définir Blueprint('api', __name__, url_prefix="/api")
# Implémenter les 5 endpoints CRUD en JSON
................................................................................
```

`app/__init__.py` (enregistrer le blueprint API)

```python
# Enregistrer le blueprint api
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 6. SCRIPT DE SEED & EXPORT (UTILITAIRES)

**Branche Git à créer :** `scripts-utils`

**Fichiers :**

```
scripts/seed.py        # insère quelques items de démonstration
scripts/export_json.py # exporte tous les items en JSON -> data/items_export.json
```

`scripts/seed.py`

```python
# Insérer N items (name, price, quantity) en base via SQLAlchemy
................................................................................
```

`scripts/export_json.py`

```python
# Exporter toutes les lignes Item au format JSON vers data/items_export.json
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 7. TESTS AUTOMATISÉS (pytest)

**Branche Git à créer :** `tests-api`

**Arborescence :**

```
tests/
  test_api.py
```

**Objectif :**

* Tester au minimum : création (POST), lecture (GET), mise à jour (PUT), suppression (DELETE) d’un item via l’API.

`tests/test_api.py`

```python
# Créer un client de test Flask, puis tester la séquence CRUD JSON
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

* Utiliser `python-dotenv` pour charger `.env` (ex: `FLASK_ENV=development`, `SECRET_KEY=...`).
* Ne pas committer le `.env` (ajouter au `.gitignore`).
* Lire `SECRET_KEY` dans `config.py`.

`.env` (non commité)

```
FLASK_ENV=development
SECRET_KEY=................................
```

`.gitignore`

```
# Ignorer venv, .env, data/app.db-journal, __pycache__, etc.
................................................................................
```

`config.py` (MAJ pour lire environ)

```python
# Charger environ et définir SECRET_KEY
................................................................................
```

➤ Vos commandes Git:

```python
# Écrire ici vos commandes Git :
................................................................................
```

---

## 10. QUESTIONS FINALES (`analyse.txt` à compléter précisément)

1. Pourquoi séparons-nous HTML (templates) et API (JSON) dans une app Flask ?
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

2. Donnez un exemple concret de **validation côté serveur** lors d’un POST (champ manquant / invalide).
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

3. Donnez la commande Git pour **restaurer** un fichier de template supprimé accidentellement depuis l’historique :
   Réponse : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

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

Un commit a **supprimé par erreur** la route `PUT /api/items/<id>`. Les tests `tests/test_api.py` échouent.

### 12.2. Étapes

1. À partir de `main`, créez la branche `hotfix-api`.
2. Lancez les tests (`pytest`) → échec attendu.
3. **Identifiez le commit** où la route PUT fonctionnait.
4. **Restaurez** uniquement la portion manquante depuis l’historique (sans recréer à la main si possible).
5. Commitez le correctif avec un message clair.

➤ Script / extrait corrigé dans `app/api.py` :

```python
# Coller ici uniquement la portion restaurée (PUT /api/items/<id>) :
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
tp_flask_crud_NOM_PRENOM.docx  (ou .pdf, .txt ou .py)
```

Le rapport doit contenir :

* Le **code Flask** (fichiers clés) **et** les scripts utilitaires.
* Les **commandes Git** associées à chaque étape (branch, checkout, add, commit, merge, branch -d).
* La **résolution du conflit** (avec explication).
* Les **réponses aux questions finales**.
* Le **résultat complet de** :

  ```
  git log --oneline --graph
  ```

Aucun `.zip` ou dossier séparé n’est accepté. Le code et les commandes doivent être intégrés dans ce seul fichier.

---

## Annexe – Modèles de contenu

### `requirements.txt` (exemple)

```
Flask==3.0.0
Flask-SQLAlchemy==3.1.1
python-dotenv==1.0.1
pytest==8.2.0
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

