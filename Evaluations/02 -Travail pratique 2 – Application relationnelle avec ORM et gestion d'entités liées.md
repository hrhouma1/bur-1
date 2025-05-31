<h1 id="tp2-enonce">Travail pratique 2 – Application relationnelle avec ORM et gestion d'entités liées</h1>

## Contexte pédagogique

Dans le cadre de votre formation en développement logiciel, vous êtes chargé de concevoir une application complète de gestion d’inscriptions académiques pour une institution fictive, **Académie NeoSavoir**.

L’application doit permettre :

* La gestion des **programmes d’études**
* La gestion des **étudiants**
* La relation entre les deux : **chaque étudiant est inscrit à un seul programme**



## Objectif général

Développer une **application graphique relationnelle** avec **PySide6** et **SQLAlchemy**, modulaire et persistante, permettant de manipuler deux entités liées (étudiants et programmes) selon les bonnes pratiques ORM.



## Objectifs spécifiques

1. Utiliser **Qt Designer** pour concevoir les interfaces graphiques
2. Créer des classes ORM relationnelles avec **SQLAlchemy**
3. Implémenter une **relation "plusieurs étudiants → un programme"**
4. Gérer l’ajout, la modification, et la suppression des entités
5. Mettre en place une **suppression en cascade**
6. Relier l'interface graphique aux opérations ORM de façon propre
7. Structurer le projet en modules professionnels (`/ui`, `/logic`, `/models`, etc.)



## Scénario à réaliser

L’application doit permettre à un gestionnaire administratif de :

### 1. Créer et supprimer des **programmes d’études**

* Exemple de programmes : Informatique, Réseaux, IA, Cybersécurité
* Chaque programme a un `nom`, un `code` unique, et une `durée`

### 2. Gérer les **étudiants**

* Chaque étudiant a un `nom`, un `prénom`, un `courriel`
* Il est **inscrit dans un seul programme**
* Lors de l’ajout, l’utilisateur doit sélectionner le programme d’inscription

### 3. Afficher les relations

* Depuis la page “Étudiants”, on peut voir dans quelle filière chaque étudiant est inscrit
* Depuis la page “Programmes”, on peut voir combien d’étudiants sont inscrits dans chaque programme

### 4. Comportement critique attendu :

> **Si un programme est supprimé, tous les étudiants qui y sont inscrits doivent être supprimés automatiquement (suppression en cascade).**



## Contraintes techniques

* Utiliser **SQLAlchemy** comme ORM (pas de `sqlite3` brut)
* Utiliser **PySide6** pour l’interface
* Toutes les relations doivent être déclarées via les **modèles ORM**
* L’ensemble du projet doit être organisé proprement :

  * `/models/` pour les classes ORM
  * `/ui/` pour les `.ui`
  * `/ui_gen/` pour les `.py` générés
  * `/logic/` pour les contrôleurs
  * `/data/` pour l’initialisation et la session SQLAlchemy


## Modèle relationnel attendu

### Table `Programme`

| Champ | Type    | Contraintes        |
| ----- | ------- | ------------------ |
| id    | Integer | Clé primaire, auto |
| nom   | String  | Non nul            |
| code  | String  | Unique, non nul    |
| duree | Integer | En mois            |

### Table `Etudiant`

| Champ         | Type    | Contraintes                |
| ------------- | ------- | -------------------------- |
| id            | Integer | Clé primaire, auto         |
| nom           | String  | Non nul                    |
| prenom        | String  | Non nul                    |
| courriel      | String  | Format courriel            |
| programme\_id | Integer | Foreign Key vers Programme |

**Relation :**
Un `Programme` possède plusieurs `Étudiants`.
Un `Étudiant` appartient à un seul `Programme`.
Suppression d’un programme → tous ses étudiants sont supprimés (cascade).



## Fonctionnalités requises

1. Interface de connexion (même logique que TP1)
2. Interface de tableau de bord (navigation entre pages)
3. Page de gestion des programmes

   * Liste des programmes
   * Ajout d’un programme via formulaire
   * Suppression d’un programme
4. Page de gestion des étudiants

   * Liste des étudiants avec programme d’appartenance
   * Ajout d’un étudiant avec menu déroulant pour choisir le programme
   * Suppression d’un étudiant
5. Message d’alerte clair lors de la suppression d’un programme avec étudiants



## Organisation du projet attendue

```
projet_tp2/
├── models/                # Modèles SQLAlchemy
│   ├── base.py
│   ├── programme.py
│   └── etudiant.py
├── data/                  # Connexion, création base
│   └── database.py
├── logic/                 # Fonctions de gestion
│   └── contrôleurs.py
├── ui/                    # Fichiers .ui
├── ui_gen/                # Fichiers .py générés
├── main.py                # Point d’entrée
└── README.md
```



## Livrables attendus

1. Projet compressé `.zip` avec structure complète
2. README.md documentant l’installation et le lancement
3. Base de données SQLite `db.sqlite` générée
4. Captures d’écran de toutes les interfaces fonctionnelles
5. Un `.exe` généré via `PyInstaller` (bonus)



## Évaluation

| Critère                                                 | Pondération |
| ------------------------------------------------------- | ----------- |
| Fonctionnalités complètes, suppression cascade          | 30 %        |
| Modélisation relationnelle correcte avec ORM            | 20 %        |
| Interfaces fonctionnelles et navigation cohérente       | 20 %        |
| Qualité de la validation et de l’expérience utilisateur | 10 %        |
| Structure modulaire propre                              | 10 %        |
| Bonus : packaging `.exe` avec PyInstaller               | 10 %        |



## Remise

* Archive `.zip` à remettre avant **\[date à définir]**
* Nom de l’archive : `TP2_NOM_PRENOM.zip`
* Le plagiat entraîne l’échec immédiat du TP

