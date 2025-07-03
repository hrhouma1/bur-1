

![image](https://github.com/user-attachments/assets/7b35fdd1-ad05-40a2-ba75-11c0dcd683b1)



*Liste complète des méthodes** définies dans notre classe `MainWindow`*

| N° | Méthode                      | Rôle principal                                                         |
| -- | ---------------------------- | ---------------------------------------------------------------------- |
| 1  | `__init__`                   | Initialisation de la fenêtre principale, de l’UI et de la connexion BD |
| 2  | `setup_ui`                   | Configuration du tableau et des éléments d’interface                   |
| 3  | `connect_signals`            | Connexion des boutons/signaux aux fonctions                            |
| 4  | `load_states`                | Charger les provinces dans le ComboBox                                 |
| 5  | `on_state_changed`           | Charger les villes selon la province choisie                           |
| 6  | `load_students`              | Charger tous les étudiants depuis la base                              |
| 7  | `populate_table`             | Remplir le tableau avec les étudiants                                  |
| 8  | `get_form_data`              | Récupérer les données saisies dans le formulaire                       |
| 9  | `validate_form_data`         | Vérifier que le formulaire est bien rempli et l’email valide           |
| 10 | `add_student`                | Ajouter un nouvel étudiant                                             |
| 11 | `update_student`             | Modifier un étudiant sélectionné                                       |
| 12 | `select_student`             | Charger les données d’un étudiant dans le formulaire                   |
| 13 | `search_students`            | Rechercher des étudiants selon un mot-clé                              |
| 14 | `clear_fields`               | Réinitialiser tous les champs du formulaire                            |
| 15 | `delete_student`             | Supprimer un étudiant sélectionné                                      |
| 16 | `on_table_selection_changed` | Gérer les changements de sélection dans le tableau (actuellement vide) |




# Méthode 1

## Méthode 1 : `__init__(self)`

### Objectif pédagogique

Cette méthode **initialise l’application graphique**. Elle est appelée automatiquement à la création de la fenêtre principale (`MainWindow`). Elle sert à **préparer l’interface utilisateur**, à **établir la connexion avec la base de données**, et à **charger les données**.

---

### Détails du code

```python
def __init__(self):
    super().__init__()
    
    # Création de l'interface utilisateur (chargement depuis Qt Designer)
    self.ui = Ui_Form()
    self.ui.setupUi(self)
    
    # Connexion à la base de données
    self.db_manager = DatabaseManager()
    
    # Configuration initiale de l'interface
    self.setup_ui()
    
    # Connexion des boutons et signaux utilisateur
    self.connect_signals()
    
    # Chargement automatique des étudiants dès le lancement
    self.load_students()
```

---

### Explication 

| Étape                                 | Action                                                                | Pourquoi c’est important                                                        |
| ------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `super().__init__()`                  | Appelle le constructeur de `QMainWindow`                              | Nécessaire pour initialiser correctement la fenêtre Qt                          |
| `self.ui = Ui_Form()`                 | Crée une instance de l’interface graphique (générée avec Qt Designer) | Permet d’utiliser tous les éléments graphiques (champs, boutons, tableau, etc.) |
| `self.ui.setupUi(self)`               | Applique le design sur la fenêtre actuelle                            | Rend l’interface visible et interactive                                         |
| `self.db_manager = DatabaseManager()` | Crée la connexion avec la base de données                             | Permet de faire des requêtes (ajout, recherche, etc.)                           |
| `self.setup_ui()`                     | Configure les menus déroulants, le tableau, etc.                      | Prépare l’interface avant l’interaction de l’utilisateur                        |
| `self.connect_signals()`              | Connecte chaque bouton à une fonction                                 | Permet de réagir quand l’utilisateur clique sur un bouton                       |
| `self.load_students()`                | Remplit le tableau avec les données de la base                        | Affiche les étudiants dès l’ouverture de l’appli                                |

---

## Quiz – Méthode `__init__()`

**Barème** :
Bonne réponse → +1
Mauvaise réponse → -1
Pas de réponse → 0



### 1. Quel est le rôle principal de la méthode `__init__()` dans une application PySide6 ?

- a) Dessiner un graphique
- b) Initialiser la base de données uniquement
- c) Préparer et afficher l’interface graphique + charger la logique
- d) Envoyer un email

**→ Réponse attendue : c**



### 2. Quelle ligne connecte l’application à la base de données MySQL ?

- a) `self.ui.setupUi(self)`
- b) `self.db_manager = DatabaseManager()`
- c) `self.connect_signals()`
- d) `self.load_students()`

**→ Réponse attendue : b**



### 3. Quel est l’effet de `self.load_students()` ?

- a) Enregistre un étudiant
- b) Supprime la table
- c) Remplit le tableau de la GUI avec les étudiants de la base
- d) Vide tous les champs

**→ Réponse attendue : c**



### 4. À quoi sert `self.ui = Ui_Form()` ?

- a) Générer la base de données
- b) Ajouter un nouvel étudiant
- c) Créer une instance de l’interface graphique basée sur Qt Designer
- d) Supprimer tous les widgets

**→ Réponse attendue : c**



### 5. Quelle ligne garantit que la fenêtre peut recevoir et afficher le design Qt ?

- a) `super().__init__()`
- b) `self.db_manager = DatabaseManager()`
- c) `self.ui.setupUi(self)`
- d) `self.connect_signals()`

**→ Réponse attendue : c**


<br/>
<br/>

# Plus de questions pour la Méthode 1

## Quiz – Méthode `__init__` dans `MainWindow`

> **Objectif :** Comprendre comment la méthode `__init__` initialise l'application, établit les dépendances, prépare les composants de l’interface, et met en place la logique de départ.

---

### 1. Quand la méthode `__init__` est-elle appelée dans la classe `MainWindow` ?

* a) Lors du clic sur le bouton “Ajouter”
* b) Lors de l’instanciation de `MainWindow`
* c) Lors de l’exécution de `load_students()`
* d) Lors de l’importation de `DatabaseManager`

**Bonne réponse :** *b) Lors de l’instanciation de `MainWindow`*

---

### 2. Que fait exactement la ligne suivante dans `__init__` ?

```python
self.ui = Ui_Form()  
```

* a) Elle connecte la base de données à l’interface
* b) Elle crée une instance de la classe d’interface générée avec Qt Designer
* c) Elle configure le tableau des étudiants
* d) Elle applique les styles CSS à l'application

**Bonne réponse :** *b) Elle crée une instance de la classe d’interface générée avec Qt Designer*

---

### 3. Quel est le rôle de l’appel suivant ?

```python
self.ui.setupUi(self)  
```

* a) Charger dynamiquement les signaux Qt
* b) Connecter les widgets à la base de données
* c) Positionner les éléments graphiques dans la fenêtre
* d) Ouvrir le formulaire d’ajout d’étudiant

**Bonne réponse :** *c) Positionner les éléments graphiques dans la fenêtre*

---

### 4. Quelle est la fonction de cette ligne ?

```python
self.db_manager = DatabaseManager()  
```

* a) Elle ouvre une connexion directe à MySQL via PyQt
* b) Elle définit la classe responsable des opérations CRUD sur la base de données
* c) Elle charge tous les étudiants dans l’interface
* d) Elle crée un fichier local temporaire pour les sauvegardes

**Bonne réponse :** *b) Elle définit la classe responsable des opérations CRUD sur la base de données*

---

### 5. Est-ce que `DatabaseManager()` établit une connexion réelle à la base dans `__init__` ?

* a) Oui, il utilise `connect()` une seule fois
* b) Non, il définit les paramètres mais ne connecte pas
* c) Oui, et la connexion reste ouverte
* d) Non, la connexion est toujours ouverte ailleurs

**Bonne réponse :** *c) Oui, et la connexion reste ouverte*

---

### 6. Pourquoi appelle-t-on `self.setup_ui()` juste après avoir créé l’objet `db_manager` ?

* a) Pour appliquer un thème sombre à l’interface
* b) Pour configurer les ComboBox et le tableau
* c) Pour créer dynamiquement des widgets
* d) Pour activer la logique de validation

**Bonne réponse :** *b) Pour configurer les ComboBox et le tableau*

---

### 7. Que fait la méthode `connect_signals()` appelée ensuite ?

* a) Elle initialise la structure de la base
* b) Elle relie les actions de l’interface (clics) aux méthodes Python
* c) Elle applique une animation d’entrée à la fenêtre
* d) Elle réinitialise les champs du formulaire

**Bonne réponse :** *b) Elle relie les actions de l’interface (clics) aux méthodes Python*

---

### 8. Pourquoi `self.load_students()` est-elle appelée en dernier dans `__init__` ?

* a) Pour afficher les messages d’erreur
* b) Pour charger les données une fois l’interface prête
* c) Pour tester la connexion réseau
* d) Pour réinitialiser les signaux Qt

**Bonne réponse :** *b) Pour charger les données une fois l’interface prête*





<br/>
<br/>

# Méthode 2


## Quiz pédagogique – Méthode `setup_ui()` dans `MainWindow`

> **Objectif :** Comprendre le rôle de `setup_ui()`, la configuration initiale de l’interface utilisateur, la gestion des ComboBox, et la mise en place du tableau.

---

### 1. Quel est le but principal de la méthode `setup_ui()` ?

* a) Déclencher les requêtes SQL nécessaires
* b) Configurer les widgets graphiques dès le lancement
* c) Lancer les opérations de validation
* d) Ajouter des étudiants par défaut

**Bonne réponse :** *b) Configurer les widgets graphiques dès le lancement*

---

### 2. Que fait cette ligne dans `setup_ui()` ?

```python
self.ui.tableWidget.setSelectionBehavior(self.ui.tableWidget.SelectionBehavior.SelectRows)  
```

* a) Elle permet la sélection d’une cellule unique
* b) Elle empêche la sélection multiple
* c) Elle autorise la sélection de lignes entières
* d) Elle vide les lignes existantes du tableau

**Bonne réponse :** *c) Elle autorise la sélection de lignes entières*

---

### 3. À quoi sert `QHeaderView.ResizeMode.Stretch` dans la configuration du tableau ?

* a) À trier les colonnes automatiquement
* b) À ajuster dynamiquement la hauteur des lignes
* c) À forcer les colonnes à remplir tout l’espace horizontal
* d) À ajouter des boutons dans l'en-tête

**Bonne réponse :** *c) À forcer les colonnes à remplir tout l’espace horizontal*

---

### 4. Quelle configuration est appliquée au `comboBox_2` (ville) dans cette méthode ?

* a) Il devient invisible par défaut
* b) Il affiche uniquement les villes de l’Ontario
* c) Il est rendu éditable pour permettre la saisie manuelle
* d) Il est lié à une base externe PostgreSQL

**Bonne réponse :** *c) Il est rendu éditable pour permettre la saisie manuelle*

---

### 5. Que signifie le paramètre `NoInsert` dans :

```python
self.ui.comboBox_2.setInsertPolicy(self.ui.comboBox_2.InsertPolicy.NoInsert)  
```

* a) L'utilisateur peut modifier la ville, mais elle ne sera pas ajoutée à la liste
* b) La liste des villes est automatiquement triée
* c) Les villes ne peuvent pas être supprimées
* d) Les villes sont ajoutées deux fois pour compatibilité

**Bonne réponse :** *a) L'utilisateur peut modifier la ville, mais elle ne sera pas ajoutée à la liste*

---

### 6. Pourquoi la méthode `load_states()` est-elle appelée à la fin de `setup_ui()` ?

* a) Pour appliquer des couleurs aux provinces
* b) Pour afficher un message de bienvenue
* c) Pour charger les provinces dans le ComboBox principal
* d) Pour vider la base de données avant chargement

**Bonne réponse :** *c) Pour charger les provinces dans le ComboBox principal*

---

### 7. Que se passe-t-il si la base de données ne contient aucune province ?

* a) Le champ est désactivé
* b) Un message d’erreur s’affiche
* c) Des provinces canadiennes par défaut sont ajoutées
* d) Un signal sonore est déclenché

**Bonne réponse :** *c) Des provinces canadiennes par défaut sont ajoutées*

---

### 8. À quel moment `setup_ui()` est-elle appelée ?

* a) Lors du clic sur le bouton “Ajouter”
* b) Juste après l’instanciation de `DatabaseManager()`
* c) Avant la méthode `__init__`
* d) Lors de l’appel de `load_students()`

**Bonne réponse :** *b) Juste après l’instanciation de `DatabaseManager()`*




<br/>
<br/>

# Méthode 3


## Quiz pédagogique – Méthode `connect_signals()` dans `MainWindow`

> **Objectif :** Comprendre la logique de connexion des signaux aux slots (fonctions), le lien entre l’interface et les méthodes Python, et les effets déclenchés par les interactions de l’utilisateur.

---

### 1. À quoi sert la méthode `connect_signals()` ?

* a) À vérifier les erreurs dans les formulaires
* b) À connecter les éléments de l'interface aux fonctions associées
* c) À synchroniser la base de données avec l’interface
* d) À désactiver les boutons inutilisés

**Bonne réponse :** *b) À connecter les éléments de l'interface aux fonctions associées*

---

### 2. Quelle est la conséquence de la ligne suivante ?

```python
self.ui.add_btn.clicked.connect(self.add_student)  
```

* a) Le bouton est caché
* b) Le bouton exécute `add_student` quand on clique dessus
* c) Le bouton se désactive après le clic
* d) Le bouton ferme la fenêtre

**Bonne réponse :** *b) Le bouton exécute `add_student` quand on clique dessus*

---

### 3. Quel élément déclenche la méthode `on_table_selection_changed()` ?

* a) Un double-clic sur le tableau
* b) Un changement de ligne sélectionnée dans le tableau
* c) Le lancement de l’application
* d) Une modification dans la base de données

**Bonne réponse :** *b) Un changement de ligne sélectionnée dans le tableau*

---

### 4. Pourquoi utiliser `.connect()` dans PySide6 ?

* a) Pour activer les champs de texte
* b) Pour relier un signal (ex: clic) à une action (slot)
* c) Pour modifier le style graphique
* d) Pour créer une nouvelle fenêtre

**Bonne réponse :** *b) Pour relier un signal (ex: clic) à une action (slot)*

---

### 5. Quelle méthode est exécutée quand la province est changée dans `comboBox` ?

* a) `validate_form_data()`
* b) `load_states()`
* c) `on_state_changed()`
* d) `load_students()`

**Bonne réponse :** *c) `on_state_changed()`*

---

### 6. Que relie exactement cette ligne ?

```python
self.ui.comboBox.currentTextChanged.connect(self.on_state_changed)  
```

* a) Le texte d’un champ email à la recherche automatique
* b) Le changement de ville au champ email
* c) Le changement de province à la mise à jour des villes
* d) Le changement de nom à la base de données

**Bonne réponse :** *c) Le changement de province à la mise à jour des villes*

---

### 7. À quel moment `connect_signals()` est-elle appelée ?

* a) Dès qu’on clique sur un bouton
* b) Lors du démarrage, dans `__init__()`
* c) Après avoir vidé les champs
* d) Juste avant de fermer l’application

**Bonne réponse :** *b) Lors du démarrage, dans `__init__()`*

---

### 8. Pourquoi est-il essentiel de centraliser ces connexions dans une méthode dédiée ?

* a) Pour pouvoir les désactiver d’un seul coup
* b) Pour éviter de les répéter dans plusieurs fonctions
* c) Pour rendre le code plus clair et maintenable
* d) Pour intercepter tous les signaux sans effort

**Bonne réponse :** *c) Pour rendre le code plus clair et maintenable*



<br/>
<br/>

# Méthode 4





## Méthode 4 - `load_states()` dans `MainWindow`

> **Objectif :** Comprendre le fonctionnement de la méthode `load_states()`, son lien avec la base de données, la logique de secours en cas de liste vide, et l’impact sur l’interface.

---

### 1. Quel est l’objectif principal de `load_states()` ?

* a) Ajouter des villes par défaut dans la base
* b) Afficher les étudiants dans le tableau
* c) Remplir le menu déroulant `comboBox` avec la liste des provinces
* d) Effacer les anciens étudiants

**Bonne réponse :** *c) Remplir le menu déroulant `comboBox` avec la liste des provinces*

---

### 2. Quelle méthode de la base de données est appelée dans `load_states()` ?

* a) `get_all_students()`
* b) `search_students()`
* c) `get_states()`
* d) `get_cities_by_state()`

**Bonne réponse :** *c) `get_states()`*

---

### 3. Que se passe-t-il si `get_states()` retourne une liste vide ?

* a) Un message d’erreur est affiché
* b) Le tableau est désactivé
* c) Une liste de provinces canadiennes est ajoutée manuellement
* d) L’application se ferme

**Bonne réponse :** *c) Une liste de provinces canadiennes est ajoutée manuellement*

---

### 4. Quel est le but de cette ligne dans `load_states()` ?

```python
self.ui.comboBox.clear()
```

* a) Elle vide le tableau d’étudiants
* b) Elle réinitialise le champ de recherche
* c) Elle efface les éléments précédents du `comboBox` pour le recharger proprement
* d) Elle désactive le bouton Ajouter

**Bonne réponse :** *c) Elle efface les éléments précédents du `comboBox` pour le recharger proprement*

---

### 5. Quand `load_states()` est-elle appelée dans le cycle de vie de l’application ?

* a) Lorsqu’on clique sur “Ajouter”
* b) À la fin de l’application
* c) Au démarrage via `setup_ui()`
* d) Lors d’un changement de ville

**Bonne réponse :** *c) Au démarrage via `setup_ui()`*

---

### 6. Pourquoi est-il important de proposer une liste par défaut si la base est vide ?

* a) Pour simuler une erreur et afficher une alerte
* b) Pour que l’utilisateur puisse tout de même saisir des données
* c) Pour éviter d’utiliser Qt Designer
* d) Pour effacer les doublons

**Bonne réponse :** *b) Pour que l’utilisateur puisse tout de même saisir des données*

---

### 7. Où sont stockées les provinces par défaut si la base est vide ?

* a) Dans un fichier JSON externe
* b) Dans une variable nommée `default_provinces`
* c) Dans le champ `lineEdit_2`
* d) Dans le fichier `main_ui.py`

**Bonne réponse :** *b) Dans une variable nommée `default_provinces`*

---

### 8. Cette méthode modifie-t-elle directement la base de données ?

* a) Oui, elle insère les provinces manquantes
* b) Non, elle se contente d’afficher les données dans le `comboBox`
* c) Oui, elle vide et recharge la table `states`
* d) Non, mais elle envoie un email si la liste est vide

**Bonne réponse :** *b) Non, elle se contente d’afficher les données dans le `comboBox`*


<br/>
<br/>

# Méthode 5



## Quiz – Méthode `on_state_changed(state)` dans `MainWindow`

> **Objectif :** Comprendre comment la sélection d’une province dans l’interface déclenche la mise à jour de la liste des villes, et comment le système gère les absences de données en base.

---

### 1. À quel moment cette méthode est-elle appelée automatiquement ?

* a) Lorsque l’utilisateur clique sur le bouton « Ajouter »
* b) Lorsqu’on clique sur une ligne du tableau
* c) Lorsque l’utilisateur sélectionne une province dans le `comboBox`
* d) Lors de l’initialisation de la base de données

**Bonne réponse :** *c) Lorsque l’utilisateur sélectionne une province dans le `comboBox`*

---

### 2. Que fait la méthode `get_cities_by_state(state)` dans cette méthode ?

* a) Elle insère de nouvelles villes dans la base
* b) Elle supprime les villes existantes du tableau
* c) Elle retourne une liste de villes correspondant à la province sélectionnée
* d) Elle affiche une boîte de dialogue

**Bonne réponse :** *c) Elle retourne une liste de villes correspondant à la province sélectionnée*

---

### 3. Pourquoi utilise-t-on `self.ui.comboBox_2.clear()` avant d’ajouter de nouvelles villes ?

* a) Pour supprimer la province précédente
* b) Pour réinitialiser les sélections de l’utilisateur
* c) Pour éviter d’empiler les anciennes villes à chaque changement de province
* d) Pour améliorer les performances de la base

**Bonne réponse :** *c) Pour éviter d’empiler les anciennes villes à chaque changement de province*

---

### 4. Que se passe-t-il si la méthode `get_cities_by_state(state)` retourne une liste vide ?

* a) L’application affiche une erreur
* b) La ville précédente est conservée
* c) Une liste de villes par défaut est ajoutée selon la province
* d) Rien ne se passe, le champ reste vide

**Bonne réponse :** *c) Une liste de villes par défaut est ajoutée selon la province*

---

### 5. Où se trouve la liste de secours des villes par province dans le code ?

* a) Dans une variable `default_cities`
* b) Dans un dictionnaire nommé `province_cities`
* c) Dans un fichier CSV externe
* d) Dans une base secondaire `fallback_db`

**Bonne réponse :** *b) Dans un dictionnaire nommé `province_cities`*

---

### 6. Quelle est la logique utilisée pour ajouter les villes par défaut si aucune n’est trouvée ?

* a) On ajoute toujours les villes de Toronto et Vancouver
* b) On utilise une liste générale `["Calgary", "Toronto", "Vancouver"]` sans vérifier la province
* c) On vérifie si la province est présente dans `province_cities` et on ajoute ses villes associées
* d) On insère des villes dans la base avant de les afficher

**Bonne réponse :** *c) On vérifie si la province est présente dans `province_cities` et on ajoute ses villes associées*

---

### 7. Quelle est la fonction de cette ligne à la fin du bloc conditionnel ?

```python
self.ui.comboBox_2.addItems(default_cities)
```

* a) Elle insère les villes dans la base de données
* b) Elle remplit le `comboBox_2` avec la liste par défaut
* c) Elle affiche une alerte si aucune ville n’est trouvée
* d) Elle ferme le formulaire

**Bonne réponse :** *b) Elle remplit le `comboBox_2` avec la liste par défaut*


<br/>
<br/>

# Méthode 6




## Quiz – Méthode `load_students()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre le lien entre la base de données et l’affichage des données dans le tableau de l’interface utilisateur. Identifier les fonctions appelées en cascade et anticiper les erreurs possibles.

---

### 1. Quelle est la fonction principale de `load_students()` ?

* a) Supprimer tous les étudiants
* b) Afficher dans le tableau tous les étudiants récupérés depuis la base
* c) Ajouter un étudiant manuellement
* d) Vider tous les champs du formulaire

**Bonne réponse :** *b) Afficher dans le tableau tous les étudiants récupérés depuis la base*

---

### 2. Quelle méthode est appelée à l’intérieur de `load_students()` pour afficher les données dans l’interface ?

* a) `setup_ui()`
* b) `add_student()`
* c) `populate_table(students)`
* d) `validate_form_data()`

**Bonne réponse :** *c) `populate_table(students)`*

---

### 3. Que fait exactement la ligne suivante ?

```python
students = self.db_manager.get_all_students()
```

* a) Elle modifie les informations d’un étudiant
* b) Elle vide la base de données
* c) Elle récupère tous les enregistrements d’étudiants depuis la base
* d) Elle ajoute un étudiant fictif dans le tableau

**Bonne réponse :** *c) Elle récupère tous les enregistrements d’étudiants depuis la base*

---

### 4. Quelles sont les conséquences si la base de données est vide ?

* a) Le tableau est rempli avec des données fictives
* b) Un message d’erreur est affiché
* c) Le tableau reste vide (aucune ligne affichée)
* d) L’application se ferme automatiquement

**Bonne réponse :** *c) Le tableau reste vide (aucune ligne affichée)*

---

### 5. Pourquoi `load_students()` est-elle appelée à la fin de la méthode `__init__()` ?

* a) Pour ajouter un étudiant test
* b) Pour remplir immédiatement le tableau avec les données de la base au lancement
* c) Pour éviter une erreur de connexion
* d) Pour vider les champs

**Bonne réponse :** *b) Pour remplir immédiatement le tableau avec les données de la base au lancement*

---

### 6. Quand cette méthode est-elle appelée en dehors de `__init__()` ?

* a) Lorsqu’on clique sur « Ajouter », « Modifier » ou « Supprimer »
* b) Lorsqu’on tape du texte dans un champ
* c) Lorsqu’on change de province
* d) Lorsqu’on ferme la fenêtre

**Bonne réponse :** *a) Lorsqu’on clique sur « Ajouter », « Modifier » ou « Supprimer »*

---

### 7. Quelle serait une amélioration possible à apporter à `load_students()` ?

* a) Ajouter un tri automatique des noms
* b) Ajouter un message d’erreur s’il y a trop d’étudiants
* c) Supprimer les boutons pendant le chargement
* d) Appeler `validate_form_data()` en boucle

**Bonne réponse suggérée :** *a) Ajouter un tri automatique des noms*



<br/>
<br/>

# Méthode 7


## Quiz – Méthode 7 `populate_table()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre le rôle de `populate_table()` dans l'affichage dynamique des étudiants, la logique de remplissage ligne/colonne, et la gestion des objets `QTableWidgetItem`.

---

### 1. Que reçoit la méthode `populate_table()` comme argument ?

* a) Un dictionnaire contenant les champs du formulaire
* b) Une liste de lignes (étudiants) issues de la base de données
* c) Un signal Qt pour activer la table
* d) Un index de sélection d’étudiant

**Bonne réponse :** *b) Une liste de lignes (étudiants) issues de la base de données*

---

### 2. Que fait la ligne suivante ?

```python
self.ui.tableWidget.setRowCount(len(students))
```

* a) Elle initialise les colonnes du tableau
* b) Elle supprime toutes les lignes du tableau
* c) Elle définit le nombre de lignes à afficher en fonction du nombre d'étudiants
* d) Elle vide les cellules du tableau

**Bonne réponse :** *c) Elle définit le nombre de lignes à afficher en fonction du nombre d'étudiants*

---

### 3. À quoi sert cette ligne ?

```python
QTableWidgetItem(str(student[0]))
```

* a) Elle modifie une cellule vide en une ComboBox
* b) Elle transforme un nom en majuscules
* c) Elle crée un widget de type case à cocher
* d) Elle crée un objet affichable dans une cellule à partir d’un entier (studentId)

**Bonne réponse :** *d) Elle crée un objet affichable dans une cellule à partir d’un entier (studentId)*

---

### 4. Pourquoi utilise-t-on une boucle `for` dans cette méthode ?

* a) Pour supprimer tous les étudiants
* b) Pour traiter chaque étudiant un par un et insérer ses données dans une ligne
* c) Pour répéter une animation
* d) Pour créer des boutons dynamiques

**Bonne réponse :** *b) Pour traiter chaque étudiant un par un et insérer ses données dans une ligne*

---

### 5. Quel est l'ordre exact des colonnes utilisées dans le tableau ?

* a) ID, Prénom, Ville, Province, Email
* b) ID, Prénom, Nom, Ville, Province, Email
* c) ID, Nom, Prénom, Province, Email
* d) Prénom, Nom, Ville, Province

**Bonne réponse :** *b) ID, Prénom, Nom, Ville, Province, Email*

---

### 6. Quelle conséquence aurait l’absence d’appel à `populate_table()` après `add_student()` ou `update_student()` ?

* a) Les champs du formulaire ne fonctionneraient plus
* b) L’utilisateur ne pourrait plus supprimer
* c) Le tableau ne serait pas mis à jour visuellement
* d) Le programme générerait une erreur système

**Bonne réponse :** *c) Le tableau ne serait pas mis à jour visuellement*

---

### 7. Si `student` est une liste comme `[12, "Marie", "Dubois", "Québec", "Montréal", "marie@example.com"]`, quelle ligne du tableau affichera `"Montréal"` ?

* a) Colonne 2
* b) Colonne 3
* c) Colonne 4
* d) Colonne 1

**Bonne réponse :** *c) Colonne 3*


<br/>
<br/>

# Méthode 8



## Quiz – Méthode 8 `get_form_data()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment les champs de l’interface sont lus, organisés et utilisés pour interagir avec le backend.

---

### 1. Quelle est la structure retournée par `get_form_data()` ?

* a) Une chaîne de caractères
* b) Une liste de champs de saisie
* c) Un dictionnaire contenant les informations saisies par l’utilisateur
* d) Un tableau HTML

**Bonne réponse :** *c) Un dictionnaire contenant les informations saisies par l’utilisateur*

---

### 2. À quoi sert `.strip()` dans ce contexte ?

* a) À crypter les données
* b) À convertir les champs en majuscules
* c) À supprimer les espaces inutiles avant/après les valeurs saisies
* d) À colorer les champs en rouge

**Bonne réponse :** *c) À supprimer les espaces inutiles avant/après les valeurs saisies*

---

### 3. Quel champ du formulaire est associé à la clé `'email'` dans le dictionnaire retourné ?

* a) `lineEdit_3`
* b) `comboBox_2`
* c) `lineEdit_6`
* d) `lineEdit_7`

**Bonne réponse :** *c) `lineEdit_6`*

---

### 4. La clé `'state'` du dictionnaire correspond à :

* a) Un champ de saisie libre
* b) Un champ de recherche
* c) Une sélection dans le `comboBox` des provinces
* d) Une cellule du tableau

**Bonne réponse :** *c) Une sélection dans le `comboBox` des provinces*

---

### 5. Pourquoi `get_form_data()` est-elle indispensable avant d’appeler `add_student()` ou `update_student()` ?

* a) Pour tester la connexion Internet
* b) Pour afficher les erreurs système
* c) Pour récupérer toutes les données du formulaire sous une forme uniforme
* d) Pour effacer les données de la table

**Bonne réponse :** *c) Pour récupérer toutes les données du formulaire sous une forme uniforme*

---

### 6. Que retourne la méthode si tous les champs du formulaire sont vides ?

* a) Un dictionnaire avec des valeurs `None`
* b) Un dictionnaire avec des chaînes vides `""` pour chaque champ
* c) Une exception `KeyError`
* d) Un tableau rempli automatiquement

**Bonne réponse :** *b) Un dictionnaire avec des chaînes vides `""` pour chaque champ*

---

### 7. À quel moment cette méthode est-elle généralement appelée dans le cycle de l’application ?

* a) Lorsqu’un utilisateur clique sur un bouton d’action (Ajouter, Modifier)
* b) À chaque démarrage de la fenêtre
* c) Quand un étudiant est supprimé
* d) Quand l’utilisateur ferme l’application

**Bonne réponse :** *a) Lorsqu’un utilisateur clique sur un bouton d’action (Ajouter, Modifier)*




<br/>
<br/>

# Méthode 9


## Quiz – Méthode 9 `validate_form_data(data)` dans `MainWindow`

> **Objectif pédagogique :** Comprendre le rôle central de la validation côté interface avant d’interagir avec la base de données. Identifier les conditions nécessaires pour qu’une opération d’écriture soit permise.

---

### 1. Quel est le rôle principal de la méthode `validate_form_data()` ?

* a) Vérifier les connexions à la base de données
* b) Valider que les champs du formulaire sont correctement remplis avant toute opération
* c) Réinitialiser tous les champs de l’interface
* d) Vérifier que l’étudiant existe déjà

**Bonne réponse :** *b) Valider que les champs du formulaire sont correctement remplis avant toute opération*

---

### 2. Que se passe-t-il si l’un des champs du dictionnaire `data` est vide ?

* a) La méthode retourne `None`
* b) L'application plante avec une erreur
* c) Une boîte d’avertissement s’affiche et la méthode retourne `False`
* d) L’étudiant est quand même ajouté

**Bonne réponse :** *c) Une boîte d’avertissement s’affiche et la méthode retourne `False`*

---

### 3. Quel est l’effet pratique du `return False` dans cette méthode ?

* a) Il empêche l’exécution de la méthode `add_student()` ou `update_student()`
* b) Il vide le tableau
* c) Il affiche tous les étudiants
* d) Il redémarre la base

**Bonne réponse :** *a) Il empêche l’exécution de la méthode `add_student()` ou `update_student()`*

---

### 4. Pourquoi la validation de l’adresse email est-elle détaillée et divisée en plusieurs vérifications ?

* a) Pour respecter les normes de sécurité Web
* b) Pour forcer l’utilisation d’emails professionnels
* c) Pour éviter les erreurs fréquentes de saisie avant d’enregistrer en base
* d) Pour contourner les limitations du widget QLineEdit

**Bonne réponse :** *c) Pour éviter les erreurs fréquentes de saisie avant d’enregistrer en base*

---

### 5. La méthode `validate_form_data()` effectue-t-elle une **vérification directe dans la base de données** (comme `SELECT`) ?

* a) Oui, elle utilise `db_manager.get_student()`
* b) Non, elle ne fait que vérifier les valeurs en local dans le formulaire
* c) Oui, via `search_students()`
* d) Oui, mais uniquement si l’utilisateur est connecté

**Bonne réponse :** *b) Non, elle ne fait que vérifier les valeurs en local dans le formulaire*

---

### 6. Est-ce que la validation vérifie si l’email est déjà présent dans la base de données (doublon) ?

* a) Oui, avec `db_manager.find_by_email()`
* b) Non, ce contrôle n’est pas inclus dans cette méthode
* c) Oui, implicitement via `load_students()`
* d) Oui, mais c’est déclenché par un autre signal

**Bonne réponse :** *b) Non, ce contrôle n’est pas inclus dans cette méthode*

---

### 7. Quelle condition invalide **immédiatement** une adresse email ?

```python
if '@' not in email:
    QMessageBox.warning(self, "Erreur", "L'email doit contenir '@'")
    return False
```

* a) Si l’email est vide
* b) S’il contient un espace
* c) S’il ne contient pas le caractère `@`
* d) S’il commence par un chiffre

**Bonne réponse :** *c) S’il ne contient pas le caractère `@`*

---

### 8. Pourquoi l’application effectue-t-elle autant de vérifications avant d’appeler une requête SQL `INSERT` ou `UPDATE` ?

* a) Pour éviter les plantages MySQL dus à des données malformées
* b) Pour ralentir l’interface volontairement
* c) Pour contourner les erreurs du système d’exploitation
* d) Pour se passer complètement de validation côté base

**Bonne réponse :** *a) Pour éviter les plantages MySQL dus à des données malformées*

---

### 9. Quel risque est **évité** grâce à la vérification suivante ?

```python
if '..' in email:
    QMessageBox.warning(self, "Erreur", "L'email ne peut pas contenir de double point")
    return False
```

* a) Risque d'injection SQL
* b) Risque d’email syntaxiquement invalide
* c) Risque de duplication de compte
* d) Risque de boucle infinie

**Bonne réponse :** *b) Risque d’email syntaxiquement invalide*

---

### 10. Quand la connexion à la base de données est-elle utilisée dans ce processus ?

* a) Dès que la méthode `validate_form_data()` est appelée
* b) Seulement **après** que `validate_form_data()` retourne `True`, via `add_student()` ou `update_student()`
* c) Immédiatement après la saisie dans le champ email
* d) À chaque fois qu’un champ change

**Bonne réponse :** *b) Seulement **après** que `validate_form_data()` retourne `True`, via `add_student()` ou `update_student()`*




<br/>
<br/>

# Méthode 10




## Quiz – Méthode 10 `add_student()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment les données du formulaire sont traitées, validées, puis insérées dans la base. Identifier les étapes critiques avant et après la requête SQL.

---

### 1. Quelle est la toute première étape de la méthode `add_student()` ?

* a) Connexion manuelle à la base de données
* b) Appel à la méthode `get_form_data()` pour lire les champs du formulaire
* c) Affichage du tableau
* d) Validation directe de l’adresse email

**Bonne réponse :**
*b) Appel à la méthode `get_form_data()` pour lire les champs du formulaire*

---

### 2. Que se passe-t-il si la validation des données échoue (`validate_form_data(data)` retourne `False`) ?

* a) L'étudiant est quand même inséré dans la base
* b) Une boîte de dialogue demande confirmation
* c) La méthode `add_student()` s’arrête immédiatement
* d) La base est vidée pour éviter l’erreur

**Bonne réponse :**
*c) La méthode `add_student()` s’arrête immédiatement*

---

### 3. Quelle méthode de la classe `DatabaseManager` est appelée pour l’insertion ?

* a) `insert_student()`
* b) `execute_insert()`
* c) `db_manager.add_student(...)`
* d) `connect_and_insert()`

**Bonne réponse :**
*c) `db_manager.add_student(...)`*

---

### 4. Que contient l’objet `data` passé à la base de données ?

* a) Un dictionnaire avec des valeurs JSON
* b) Des éléments bruts de l’interface Qt
* c) Les données propres, validées, extraites du formulaire
* d) Un fichier CSV temporaire

**Bonne réponse :**
*c) Les données propres, validées, extraites du formulaire*

---

### 5. Que se passe-t-il si l’insertion échoue (retour `success == False`) ?

* a) L'application ferme la base et redémarre
* b) Rien, aucune action visible
* c) Une boîte d’alerte signale une erreur
* d) Le champ email est automatiquement vidé

**Bonne réponse :**
*c) Une boîte d’alerte signale une erreur*

---

### 6. Si l’insertion réussit, quelles actions sont immédiatement déclenchées ?

*(plusieurs réponses possibles)*

* a) Un message "Étudiant ajouté avec succès" est affiché
* b) Le formulaire est vidé avec `clear_fields()`
* c) Tous les étudiants sont rechargés avec `load_students()`
* d) Une requête `DELETE` est lancée automatiquement

**Bonnes réponses :**
*a), b), c)*

---

### 7. Pourquoi `load_states()` est appelée même après un ajout réussi ?

* a) Pour détecter les doublons
* b) Pour recharger les provinces si une nouvelle a été ajoutée
* c) Pour lancer une requête `UPDATE`
* d) Pour réinitialiser les connexions à la base

**Bonne réponse :**
*b) Pour recharger les provinces si une nouvelle a été ajoutée*

---

### 8. Est-ce que la méthode `add_student()` ouvre une **nouvelle connexion SQL manuellement** à chaque appel ?

* a) Oui, via un `mysql.connect()` à chaque fois
* b) Non, elle utilise l’instance unique `db_manager` déjà connectée
* c) Oui, mais uniquement si `host != localhost`
* d) Non, elle envoie un signal Qt vers un backend

**Bonne réponse :**
*b) Non, elle utilise l’instance unique `db_manager` déjà connectée*

---

### 9. Où se situe le vrai **appel SQL INSERT** dans toute cette logique ?

* a) Dans la méthode `add_student()` de `MainWindow`
* b) Dans le fichier `main_ui.py`
* c) Dans la méthode `add_student()` de `DatabaseManager`
* d) Dans le fichier `.ui` généré par Qt Designer

**Bonne réponse :**
*c) Dans la méthode `add_student()` de `DatabaseManager`*

---

### 10. Quelle conséquence cela aurait-il si `validate_form_data()` était supprimée ?

* a) L’ajout ne serait plus possible
* b) Des données erronées pourraient être insérées dans la base
* c) L’application planterait immédiatement
* d) Le formulaire serait bloqué à chaque clic

**Bonne réponse :**
*b) Des données erronées pourraient être insérées dans la base*



<br/>
<br/>

# Méthode 11


## Quiz – Méthode 11 `update_student()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment un enregistrement existant est modifié dans la base de données à partir des données du formulaire, tout en assurant la validation et la sécurité logique.

---

### 1. Quelle est la première vérification effectuée dans la méthode `update_student()` ?

* a) Vérifier si la base de données est connectée
* b) Vérifier si un étudiant est sélectionné dans le tableau
* c) Vérifier si tous les champs sont vides
* d) Vérifier si l'email est en double

**Bonne réponse :**
*b) Vérifier si un étudiant est sélectionné dans le tableau*

---

### 2. Comment récupère-t-on l’identifiant (`student_id`) de l’étudiant à modifier ?

* a) En lisant la valeur du champ Email
* b) En exécutant une requête SQL `SELECT id FROM ...`
* c) En extrayant la valeur de la première cellule (`colonne 0`) de la ligne sélectionnée
* d) Il est généré aléatoirement dans `get_form_data()`

**Bonne réponse :**
*c) En extrayant la valeur de la première cellule (`colonne 0`) de la ligne sélectionnée*

---

### 3. Que se passe-t-il si aucun étudiant n’est sélectionné dans le tableau ?

* a) L’étudiant est mis à jour avec ID = 0
* b) Une boîte d’alerte demande d’en sélectionner un
* c) Une exception est levée
* d) Tous les étudiants sont mis à jour

**Bonne réponse :**
*b) Une boîte d’alerte demande d’en sélectionner un*

---

### 4. Quelles méthodes sont appelées avant de modifier la base de données ?

*(plusieurs réponses possibles)*

* a) `get_form_data()`
* b) `validate_form_data(data)`
* c) `clear_fields()`
* d) `load_students()`

**Bonnes réponses :**
*a), b)*

---

### 5. Pourquoi est-il important de revérifier les données via `validate_form_data(data)` **même pour une mise à jour** ?

* a) Car les anciens champs peuvent avoir été effacés accidentellement
* b) Car certaines modifications peuvent introduire des erreurs de saisie
* c) Car cela empêche de modifier un étudiant avec un email vide ou invalide
* d) Toutes les réponses ci-dessus

**Bonne réponse :**
*d) Toutes les réponses ci-dessus*

---

### 6. Quelle méthode de la classe `DatabaseManager` est utilisée ici pour écrire dans la base ?

* a) `replace_student()`
* b) `db_manager.update_student(...)`
* c) `db.commit()`
* d) `update_table(...)`

**Bonne réponse :**
*b) `db_manager.update_student(...)`*

---

### 7. Si la mise à jour réussit, que se passe-t-il ensuite ?

*(plusieurs réponses possibles)*

* a) Un message de succès s’affiche
* b) Le formulaire est vidé
* c) Tous les étudiants sont rechargés dans le tableau
* d) La fenêtre se ferme automatiquement

**Bonnes réponses :**
*a), b), c)*

---

### 8. Pourquoi est-il essentiel que `student_id` soit correctement récupéré **avant l'appel SQL** ?

* a) Pour pouvoir supprimer l’étudiant s’il y a une erreur
* b) Car sans ID, la base de données ne saurait pas **quel enregistrement** modifier
* c) Pour éviter un doublon dans la liste
* d) Pour forcer une reconnexion

**Bonne réponse :**
*b) Car sans ID, la base de données ne saurait pas **quel enregistrement** modifier*

---

### 9. Est-ce que `update_student()` utilise une nouvelle connexion à la base à chaque appel ?

* a) Oui, à chaque appel `update_student()` crée une nouvelle connexion
* b) Non, elle utilise la connexion centralisée dans `self.db_manager`
* c) Oui, mais uniquement pour les étudiants canadiens
* d) Non, car elle n’accède jamais à la base

**Bonne réponse :**
*b) Non, elle utilise la connexion centralisée dans `self.db_manager`*

---

### 10. Si l’utilisateur modifie la ville ou la province sans valider le formulaire, que se passe-t-il lors de la mise à jour ?

* a) La modification est annulée automatiquement
* b) Les nouvelles valeurs sont récupérées via `get_form_data()` et utilisées dans la requête SQL
* c) Une alerte s'affiche
* d) Rien, les anciennes valeurs sont conservées

**Bonne réponse :**
*b) Les nouvelles valeurs sont récupérées via `get_form_data()` et utilisées dans la requête SQL*




<br/>
<br/>

# Méthode 12



## Quiz – Méthode 12 `select_student()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment une sélection dans le tableau est utilisée pour remplir les champs du formulaire, comment les champs sont liés aux colonnes, et pourquoi cette méthode est utile avant une modification ou une suppression.

---

### 1. À quel moment `select_student()` est-elle généralement appelée ?

* a) Lors du démarrage de l’application
* b) Lorsqu’on clique sur le bouton "Rechercher"
* c) Lorsqu’un utilisateur sélectionne une ligne dans le tableau puis clique sur "Sélectionner"
* d) Lorsque le formulaire est vide

**Bonne réponse :**
*c) Lorsqu’un utilisateur sélectionne une ligne dans le tableau puis clique sur "Sélectionner"*

---

### 2. Que fait la méthode si **aucune ligne n’est sélectionnée** dans le tableau ?

* a) Elle déclenche une recherche automatique
* b) Elle affiche une boîte d’alerte demandant de sélectionner un étudiant
* c) Elle insère un étudiant vide
* d) Elle supprime l’étudiant par défaut

**Bonne réponse :**
*b) Elle affiche une boîte d’alerte demandant de sélectionner un étudiant*

---

### 3. À quoi sert la ligne suivante dans `select_student()` ?

```python
self.ui.lineEdit_2.setText(self.ui.tableWidget.item(selected_row, 1).text())
```

* a) Elle enregistre le prénom dans la base
* b) Elle lit le prénom et l'affiche dans le champ `lineEdit_2`
* c) Elle nettoie le champ de saisie du prénom
* d) Elle initialise une nouvelle ligne du tableau

**Bonne réponse :**
*b) Elle lit le prénom et l'affiche dans le champ `lineEdit_2`*

---

### 4. Quel est le rôle exact du bloc suivant dans la méthode ?

```python
state_index = self.ui.comboBox.findText(state)
if state_index != -1:
    self.ui.comboBox.setCurrentIndex(state_index)
```

* a) Ajouter une nouvelle province si elle est absente
* b) Supprimer la province
* c) Sélectionner dans la liste déroulante la province correspondant à l’étudiant sélectionné
* d) Réinitialiser toutes les provinces

**Bonne réponse :**
*c) Sélectionner dans la liste déroulante la province correspondant à l’étudiant sélectionné*

---

### 5. Pourquoi la ville (`city`) est parfois saisie via `setEditText()` au lieu de `setCurrentIndex()` ?

* a) Parce que `comboBox_2` n’autorise pas les doublons
* b) Parce que la ville n’est pas toujours présente dans la liste des villes chargées
* c) Parce que `setCurrentIndex()` est obsolète
* d) Parce qu’il faut forcer une requête SQL

**Bonne réponse :**
*b) Parce que la ville n’est pas toujours présente dans la liste des villes chargées*

---

### 6. La méthode `select_student()` modifie-t-elle des données dans la base ?

* a) Oui, elle met automatiquement à jour l'étudiant
* b) Non, elle ne fait qu'afficher des données dans le formulaire
* c) Oui, si la ville est différente
* d) Oui, mais seulement si `update_btn` est cliqué

**Bonne réponse :**
*b) Non, elle ne fait qu'afficher des données dans le formulaire*

---

### 7. Que doit faire l'utilisateur **après avoir utilisé `select_student()`** s’il veut modifier l’étudiant ?

* a) Cliquer sur "Supprimer"
* b) Cliquer sur "Ajouter"
* c) Modifier les champs, puis cliquer sur "Mettre à jour"
* d) Relancer l’application

**Bonne réponse :**
*c) Modifier les champs, puis cliquer sur "Mettre à jour"*

---

### 8. Pourquoi cette méthode est-elle importante **dans une interface CRUD** (Create, Read, Update, Delete) ?

* a) Elle permet de générer des logs utilisateur
* b) Elle prépare le formulaire pour une mise à jour sans saisir les données manuellement
* c) Elle réinitialise la base de données
* d) Elle encode les champs pour la transmission réseau

**Bonne réponse :**
*b) Elle prépare le formulaire pour une mise à jour sans saisir les données manuellement*


<br/>
<br/>

# Méthode 13



## Quiz – Méthode 13 `search_students()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment les utilisateurs peuvent effectuer une recherche dynamique sur les étudiants, comment les résultats sont chargés dans le tableau, et quelle est la logique de filtrage appliquée.

---

### 1. Quelle est la première opération effectuée dans `search_students()` ?

* a) Vider le tableau
* b) Valider tous les champs du formulaire
* c) Récupérer et nettoyer le texte du champ de recherche (`lineEdit_7`)
* d) Appeler directement la base de données

**Bonne réponse :**
*c) Récupérer et nettoyer le texte du champ de recherche (`lineEdit_7`)*

---

### 2. Que se passe-t-il si l’utilisateur **ne saisit rien** dans le champ de recherche ?

* a) Une erreur est affichée
* b) Le formulaire est vidé
* c) Tous les étudiants sont affichés à nouveau
* d) L’application se ferme

**Bonne réponse :**
*c) Tous les étudiants sont affichés à nouveau*

---

### 3. Quelle méthode du gestionnaire de base (`db_manager`) est utilisée ici ?

* a) `get_all_students()`
* b) `find_students_by_name()`
* c) `search_students(...)`
* d) `query_database(...)`

**Bonne réponse :**
*c) `search_students(...)`*

---

### 4. Comment les résultats sont-ils affichés après la recherche ?

* a) Via une boucle `print()` dans la console
* b) En appelant `self.populate_table(students)`
* c) En insérant manuellement chaque champ dans les LineEdit
* d) En affichant un graphique

**Bonne réponse :**
*b) En appelant `self.populate_table(students)`*

---

### 5. Que se passe-t-il si aucun étudiant ne correspond au terme de recherche ?

* a) Le tableau reste vide et une boîte d'information s’affiche
* b) Une exception est levée
* c) Le champ de recherche est réinitialisé
* d) L’utilisateur est redirigé vers la base

**Bonne réponse :**
*a) Le tableau reste vide et une boîte d'information s’affiche*

---

### 6. Quel est le comportement utilisateur attendu après une recherche réussie ?

* a) Modifier les résultats et cliquer sur "Ajouter"
* b) Cliquer sur "Sélectionner", puis "Mettre à jour" si nécessaire
* c) Fermer l’application
* d) Effacer la base pour relancer

**Bonne réponse :**
*b) Cliquer sur "Sélectionner", puis "Mettre à jour" si nécessaire*

---

### 7. Est-ce que la méthode `search_students()` utilise une connexion directe à la base de données MySQL ?

* a) Non, elle lit depuis un fichier CSV
* b) Oui, mais via la méthode encapsulée `db_manager.search_students()`
* c) Oui, directement avec `mysql.connector.connect(...)`
* d) Non, les données sont générées aléatoirement

**Bonne réponse :**
*b) Oui, mais via la méthode encapsulée `db_manager.search_students()`*

---

### 8. Pourquoi `search_students()` **ne modifie jamais** les données de la base ?

* a) Parce qu’elle travaille avec une copie locale
* b) Parce qu’elle n’utilise que des requêtes de lecture (SELECT)
* c) Parce que l’utilisateur n’a pas encore cliqué sur "Mettre à jour"
* d) Parce qu’elle réinitialise la table

**Bonne réponse :**
*b) Parce qu’elle n’utilise que des requêtes de lecture (SELECT)*



<br/>
<br/>

# Méthode 14



## Quiz – Méthode 14 `clear_fields()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre comment l’interface est réinitialisée entre deux opérations, pourquoi cette méthode est cruciale dans une interface graphique, et quels champs elle affecte précisément.

---

### 1. À quoi sert la méthode `clear_fields()` dans le contexte général de l’application ?

* a) À supprimer un étudiant de la base
* b) À effacer tous les messages d’erreur
* c) À réinitialiser tous les champs du formulaire à vide
* d) À arrêter la connexion avec la base de données

**Bonne réponse :**
*c) À réinitialiser tous les champs du formulaire à vide*

---

### 2. Quels champs sont effacés directement dans cette méthode ?

* a) Uniquement le champ de recherche
* b) Les champs de prénom, nom, email, ville, province et recherche
* c) La base de données entière
* d) Le tableau des étudiants

**Bonne réponse :**
*b) Les champs de prénom, nom, email, ville, province et recherche*

---

### 3. Pourquoi appelle-t-on `clearEditText()` sur le `comboBox_2` ?

* a) Pour supprimer l’historique des recherches
* b) Parce que `comboBox_2` est éditable et peut contenir une ville saisie manuellement
* c) Pour forcer le rechargement des villes
* d) Pour effacer toutes les options du tableau

**Bonne réponse :**
*b) Parce que `comboBox_2` est éditable et peut contenir une ville saisie manuellement*

---

### 4. Quelle est la différence entre `.clear()` et `.clearEditText()` ?

* a) Il n’y en a pas
* b) `.clear()` efface toutes les options, `.clearEditText()` efface uniquement la saisie actuelle d’un `QComboBox` éditable
* c) `.clearEditText()` supprime le champ complet
* d) `.clear()` n’est pas une méthode valide

**Bonne réponse :**
*b) `.clear()` efface toutes les options, `.clearEditText()` efface uniquement la saisie actuelle d’un `QComboBox` éditable*

---

### 5. Pourquoi la méthode teste-t-elle si `self.ui.comboBox.count() > 0` avant de faire `setCurrentIndex(0)` ?

* a) Pour éviter une erreur si le ComboBox est vide
* b) Pour recharger les données depuis la base
* c) Pour créer une nouvelle province
* d) Pour bloquer la saisie utilisateur

**Bonne réponse :**
*a) Pour éviter une erreur si le ComboBox est vide*

---

### 6. Est-ce que `clear_fields()` modifie la base de données ?

* a) Oui, elle supprime toutes les entrées
* b) Non, elle agit uniquement sur l'interface utilisateur
* c) Oui, elle réinitialise les IDs
* d) Oui, mais temporairement

**Bonne réponse :**
*b) Non, elle agit uniquement sur l'interface utilisateur*

---

### 7. Quel bouton est typiquement relié à cette méthode ?

* a) `add_btn`
* b) `clear_btn`
* c) `delete_btn`
* d) `update_btn`

**Bonne réponse :**
*b) `clear_btn`*

---

### 8. Pourquoi cette méthode est-elle importante **après l’ajout ou la modification d’un étudiant** ?

* a) Pour supprimer les doublons
* b) Pour éviter d’ajouter deux fois les mêmes données
* c) Pour libérer de la mémoire
* d) Pour nettoyer l’interface avant une nouvelle saisie

**Bonne réponse :**
*d) Pour nettoyer l’interface avant une nouvelle saisie*




<br/>
<br/>

# Méthode 15





## Quiz – Méthode 15 `delete_student()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre les étapes d’une suppression sécurisée d’un enregistrement, comment l’utilisateur est impliqué dans la confirmation, et comment l’interface et la base restent synchronisées.

---

### 1. Quel est le tout premier test effectué par la méthode `delete_student()` ?

* a) Vérifier si la base de données est ouverte
* b) Vérifier si un étudiant est sélectionné dans le tableau
* c) Afficher une boîte de confirmation
* d) Réinitialiser le formulaire

**Bonne réponse :**

* b) Vérifier si un étudiant est sélectionné dans le tableau \*

---

### 2. Comment l’application identifie-t-elle l’étudiant à supprimer ?

* a) En relisant le fichier CSV
* b) En lisant la ligne sélectionnée dans `tableWidget` (colonne 0 = `student_id`)
* c) En demandant manuellement à l’utilisateur
* d) En filtrant les champs de saisie

**Bonne réponse :**

* b) En lisant la ligne sélectionnée dans `tableWidget` (colonne 0 = `student_id`) \*

---

### 3. Pourquoi utilise-t-on `QMessageBox.question()` avant la suppression ?

* a) Pour permettre à l’utilisateur de confirmer son intention
* b) Pour afficher une publicité
* c) Pour relancer l’application
* d) Pour enregistrer une copie de sauvegarde

**Bonne réponse :**

* a) Pour permettre à l’utilisateur de confirmer son intention \*

---

### 4. Quelles sont les deux options proposées dans la boîte de confirmation ?

* a) OK / Fermer
* b) Oui / Non
* c) Confirmer / Annuler
* d) Continuer / Revenir

**Bonne réponse :**

* b) Oui / Non \*

---

### 5. Si l’utilisateur clique sur “Non”, que fait l’application ?

* a) Elle affiche une erreur
* b) Elle ferme l’application
* c) Elle ne fait rien
* d) Elle supprime tout de même l’étudiant

**Bonne réponse :**

* c) Elle ne fait rien \*

---

### 6. Quelle méthode de `db_manager` est utilisée pour la suppression réelle ?

* a) `clear_all()`
* b) `delete_student(student_id)`
* c) `drop_student()`
* d) `remove_entry()`

**Bonne réponse :**

* b) `delete_student(student_id)` \*

---

### 7. Que se passe-t-il si la suppression en base échoue (`success == False`) ?

* a) L'application est redémarrée
* b) L’utilisateur voit un message d’erreur via `QMessageBox.warning(...)`
* c) Le formulaire est automatiquement réinitialisé
* d) Rien n’est affiché

**Bonne réponse :**

* b) L’utilisateur voit un message d’erreur via `QMessageBox.warning(...)` \*

---

### 8. Pourquoi `clear_fields()` et `load_students()` sont-ils rappelés après la suppression réussie ?

* a) Pour effacer les logs
* b) Pour recharger les données à jour et vider le formulaire
* c) Pour relancer la connexion
* d) Pour améliorer la performance

**Bonne réponse :**

* b) Pour recharger les données à jour et vider le formulaire \*

---

### 9. Peut-on exécuter cette méthode sans sélection dans le tableau ?

* a) Oui, l’étudiant est choisi au hasard
* b) Oui, mais une erreur sera affichée
* c) Non, un test empêche l’exécution sans sélection
* d) Non, mais cela ferme l'application

**Bonne réponse :**

* c) Non, un test empêche l’exécution sans sélection \*

---

### 10. Est-ce que la connexion à la base de données est répétée ici ?

* a) Non, elle est gérée par `db_manager`, déjà instancié dans `__init__`
* b) Oui, on réouvre la connexion à chaque suppression
* c) Oui, via `mysql.connect()` direct
* d) Non, car aucune base n’est utilisée ici

**Bonne réponse :**

* a) Non, elle est gérée par `db_manager`, déjà instancié dans `__init__` \*









<br/>
<br/>

# Méthode 16


## Quiz – Méthode 16 `on_table_selection_changed()` dans `MainWindow`

> **Objectif pédagogique :** Comprendre la raison d’être de cette méthode, comment elle peut être utilisée pour améliorer l’expérience utilisateur, et pourquoi elle est définie même si vide.

---

### 1. Quand cette méthode est-elle automatiquement appelée par l’application ?

* a) Lors du clic sur un bouton
* b) Lorsqu’un étudiant est supprimé
* c) Quand l’utilisateur sélectionne une ligne du tableau (`tableWidget`)
* d) Lors de l’ouverture de la fenêtre

**Bonne réponse :**

* c) Quand l’utilisateur sélectionne une ligne du tableau (`tableWidget`) \*

---

### 2. Quel signal est connecté à cette méthode dans `connect_signals()` ?

* a) `clicked.connect(...)`
* b) `itemSelectionChanged.connect(...)`
* c) `rowChanged.connect(...)`
* d) `tableRefreshed.connect(...)`

**Bonne réponse :**

* b) `itemSelectionChanged.connect(...)` \*

---

### 3. Actuellement, que fait cette méthode dans ton code ?

* a) Elle affiche une boîte de dialogue
* b) Elle sélectionne automatiquement un étudiant
* c) Rien – elle est vide mais prête à être utilisée
* d) Elle ajoute un étudiant

**Bonne réponse :**

* c) Rien – elle est vide mais prête à être utilisée \*

---

### 4. Pourquoi garder une méthode vide dans une classe professionnelle ?

* a) Pour améliorer la vitesse de compilation
* b) Pour éviter une erreur Python
* c) Pour préparer des extensions fonctionnelles (comme pré-remplir un formulaire ou désactiver un bouton)
* d) Pour diminuer la charge mémoire

**Bonne réponse :**

* c) Pour préparer des extensions fonctionnelles (comme pré-remplir un formulaire ou désactiver un bouton) \*

---

### 5. Quelle action **utile** pourrait être ajoutée dans `on_table_selection_changed()` ?

* a) Appeler automatiquement `select_student()`
* b) Supprimer la ligne sélectionnée
* c) Lancer un son de notification
* d) Éteindre l'application

**Bonne réponse :**

* a) Appeler automatiquement `select_student()` \*

---

### 6. Est-ce que cette méthode interagit directement avec la base de données ?

* a) Oui, elle modifie le contenu
* b) Oui, elle interroge les villes
* c) Non, elle agit uniquement sur l’interface (sélection utilisateur)
* d) Non, mais elle supprime un étudiant

**Bonne réponse :**

* c) Non, elle agit uniquement sur l’interface (sélection utilisateur) \*

---

### 7. Quel avantage pédagogique y a-t-il à inclure cette méthode dans un projet de formation ?

* a) Montrer que chaque action utilisateur peut déclencher du code
* b) Accélérer l’application
* c) Simuler une base de données
* d) Cacher du code critique

**Bonne réponse :**

* a) Montrer que chaque action utilisateur peut déclencher du code \*


