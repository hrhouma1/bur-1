# Partie 1 


https://forms.office.com/r/JgEy4Gf6cQ

<br/>

# Partie 2


- Répondez aux 6 questions ci-dessous. Vous pouvez rédiger vos réponses sans limite de longueur.
Un exemple de réponse vous est fourni à titre de référence.


### Ceci est un exemple (Q0)

**Q0.** Quel type d’objet (classe) est utilisé dans FastAPI pour définir et valider les schémas de données en entrée/sortie ?

- La bonne réponse est : **une classe `BaseModel` de la bibliothèque Pydantic**. Ces classes servent à **valider automatiquement les données reçues ou envoyées** par les endpoints FastAPI.

👉 Exemple minimal :

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    in_stock: bool
```





- Q1. Quelle propriété faut-il activer pour permettre à un widget d’accepter des éléments déposés ?
- Q2. Quelle fonction de SQLAlchemy permet de créer une fabrique de sessions pour interagir avec la base de données ?
- Q3. Quelle est la différence entre les méthodes `@app.get()` et `@app.post()` dans FastAPI ?
- Q4. Dans une architecture trois tiers, quelle couche est responsable de la persistance et de l’accès aux données ?
- Q5. Donne un exemple concret de séparation entre la couche présentation et la couche logique métier.
- Q6. Pourquoi l’architecture trois tiers facilite-t-elle la maintenabilité d’une application ?




# Partie 3 (Choisir un seul exercice parmi les 2 proposés)

### 1) SQLAlchemy ORM — “Mini-Stock Produits”

* **Objectif** : concevoir une petite application de gestion de produits avec base SQLite.
* **Concept clé** : utilisation de `declarative_base`, `Session` et opérations CRUD.
* **Périmètre** :

  * Formulaire avec `QLineEdit` pour le nom et le prix
  * Boutons « Ajouter » et « Supprimer »
  * Tableau `QTableView` pour afficher la liste des produits
* **Étapes** :

  * Définir un modèle `Product(id, name, price)`
  * Créer la base SQLite au lancement
  * Ajouter / supprimer un produit et mettre à jour la vue
* **Acceptation** :

  * Ajouter → le produit s’affiche dans le tableau et persiste en BD
  * Supprimer → enlève l’élément de la BD et de l’interface
  * Relancer l’app → les données déjà enregistrées apparaissent



### 2) FastAPI + PySide6 — “TodoClient”

* **Objectif** : créer une interface PySide6 consommant une API FastAPI locale.
* **Concept clé** : communication entre client lourd et service REST (`GET` / `POST`).
* **Périmètre** :

  * API FastAPI avec routes `/tasks` (GET, POST)
  * Côté PySide6 : champ texte pour saisir une tâche, bouton « Ajouter », liste `QListWidget` des tâches
  * Requêtes envoyées via `httpx` (ou équivalent) en thread non bloquant
* **Étapes** :

  * Bouton « Ajouter » → POST vers l’API
  * Rafraîchir la liste → GET des tâches
  * Afficher le contenu reçu dans la liste Qt
* **Acceptation** :

  * Nouvelle tâche → visible immédiatement après POST
  * Rafraîchir → liste synchronisée avec l’API
  * L’UI reste fluide même si le serveur met du temps à répondre


