<h1 id="tp3-enonce">Travail pratique 3 – Application graphique connectée à une API REST (FastAPI + PySide6)</h1>

# Contexte

Vous travaillez comme développeur dans une entreprise qui conçoit des interfaces clients destinées à consommer des services web internes. Vous êtes chargé de développer **une application graphique PySide6** qui interagit dynamiquement avec une **API REST développée en FastAPI**, représentant un service de gestion d’un **catalogue de cours** et d’**inscription d’étudiants**.

L’application doit pouvoir :

* Afficher les cours disponibles (via l’API)
* Inscrire un étudiant à un cours
* Ajouter un cours dans le système
* Rechercher et supprimer dynamiquement des données


# Objectif général

Développer un système complet **client-serveur** :

* Backend : **FastAPI** exposant une API REST locale
* Frontend : **PySide6** consommant cette API via des requêtes HTTP
* Données : stockées via **SQLAlchemy** et une base SQLite



# Objectifs spécifiques

1. Développer une API REST complète avec **FastAPI**
2. Créer des modèles `Cours` et `Étudiant` avec **SQLAlchemy**
3. Gérer une relation plusieurs étudiants ↔ un cours (many-to-one)
4. Implémenter une logique REST : `GET`, `POST`, `DELETE`, `PUT`
5. Créer une interface graphique qui **consomme dynamiquement l’API**
6. Afficher les retours de l’API dans la GUI (succès, erreur, chargement)
7. Organiser proprement les deux projets (`api/`, `client/`)




# Scénario

L'institution **TechNews** souhaite proposer une application permettant de :

* Visualiser la liste des **cours offerts**
* Ajouter de nouveaux **cours**
* Afficher la liste des **étudiants inscrits** à un cours
* Inscrire un **étudiant** à un cours
* Supprimer un cours (et ses étudiants associés)

L’**API FastAPI** servira de serveur local. L’application **PySide6** consommera ses données via HTTP.


<br/>

# Partie 1 – API REST (FastAPI)

### Ressources à exposer

#### `Cours` (Table)

* `id`: entier, clé primaire
* `code`: chaîne, unique
* `titre`: chaîne
* `nb_heures`: entier

#### `Étudiant` (Table)

* `id`: entier, clé primaire
* `nom`: chaîne
* `courriel`: chaîne
* `cours_id`: Foreign Key → `Cours.id`

### Relations

* Un cours contient plusieurs étudiants
* Un étudiant est inscrit à un seul cours

### Routes FastAPI minimales

* `GET /cours/` → liste des cours
* `POST /cours/` → ajouter un cours
* `DELETE /cours/{id}` → supprimer un cours (avec cascade)
* `GET /etudiants/` → liste complète ou par cours
* `POST /etudiants/` → inscrire un étudiant
* `DELETE /etudiants/{id}` → désinscrire un étudiant

### Exigences backend

* Base SQLite via SQLAlchemy
* Pydantic pour les schémas de validation
* Serveur lancé avec Uvicorn

![tp3](https://github.com/user-attachments/assets/24feba3d-c069-4f2c-99e4-ab38d92c1b74)

<br/>

# Partie 2 – Client PySide6 (interface graphique)

### Pages attendues

#### 1. Page "Cours"

* Affiche la liste des cours depuis l’API (`GET /cours`)
* Permet d’ajouter un cours (`POST /cours`)
* Permet de supprimer un cours (`DELETE /cours/{id}`)
* Bouton pour voir les étudiants inscrits à chaque cours

#### 2. Page "Étudiants"

* Affiche les étudiants (tous ou par cours)
* Permet d’inscrire un étudiant (`POST /etudiants`)
* Permet de supprimer un étudiant (`DELETE /etudiants/{id}`)

### Contraintes de la GUI

* Affichage dynamique via `QTableWidget`
* Requêtes HTTP faites via `requests` ou `httpx`
* Validation et affichage des messages de réponse (erreur ou succès)
* Utilisation de `QDialog` pour saisir les données d’ajout

---

## Organisation du projet attendue

```
projet_tp3/
├── api/                  ← Backend FastAPI
│   ├── main.py
│   ├── models/
│   ├── database.py
│   └── schemas.py
├── client/               ← Frontend PySide6
│   ├── ui/
│   ├── ui_gen/
│   ├── logic/
│   ├── main.py
│   └── config.json
└── README.md
```

---

## Livrables attendus

1. Projet complet organisé (`api/` + `client/`)
2. API fonctionnelle avec FastAPI (`uvicorn api.main:app`)
3. Application graphique PySide6 capable d'interagir avec l’API
4. README avec instructions claires (lancement serveur, client)
5. `.zip` du projet ou dépôt GitHub privé

---

## Évaluation

| Critère                                          | Pondération |
| ------------------------------------------------ | ----------- |
| API REST complète avec FastAPI                   | 30 %        |
| Interface graphique fonctionnelle et interactive | 30 %        |
| Gestion correcte des relations via ORM           | 15 %        |
| Gestion des erreurs et des validations           | 10 %        |
| Structure du projet (modularité, propreté)       | 10 %        |
| Documentation (README clair et fonctionnel)      | 5 %         |

---

## Remise

* Archive `.zip` nommée `TP3_NOM_PRENOM.zip`
* Ou dépôt GitHub nommé `tp3-fastapi-pyside6`
* Remise avant **\[date à définir]**


