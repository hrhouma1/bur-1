
![connexion](https://github.com/user-attachments/assets/24981c6b-c5fd-45de-9c96-24019ae48deb)


![image](https://github.com/user-attachments/assets/36d46121-9b0d-4827-a220-11ff7ec18a9b)



## Liste numérotée des méthodes de `DatabaseManager`

| Numéro | Nom de la méthode               | Description courte                                                  |
| ------ | ------------------------------- | ------------------------------------------------------------------- |
| 1      | `__init__()`                    | Initialise les paramètres de connexion et appelle `create_tables()` |
| 2      | `get_connection()`              | Crée et retourne une connexion MySQL                                |
| 3      | `create_tables()`               | Crée ou recrée la table `students_info`                             |
| 4      | `add_student(...)`              | Insère un nouvel étudiant dans la base                              |
| 5      | `get_all_students()`            | Récupère tous les étudiants                                         |
| 6      | `get_student_by_id(student_id)` | Récupère un étudiant par son identifiant                            |
| 7      | `update_student(...)`           | Met à jour les informations d’un étudiant                           |
| 8      | `delete_student(student_id)`    | Supprime un étudiant selon son identifiant                          |
| 9      | `search_students(...)`          | Recherche des étudiants selon un terme et un champ                  |
| 10     | `get_states()`                  | Récupère toutes les provinces (états) uniques                       |
| 11     | `get_cities_by_state(state)`    | Récupère les villes associées à une province donnée                 |
| 12     | `clear_all_students()`          | Supprime tous les étudiants de la base                              |
| 13     | `get_student_count()`           | Retourne le nombre total d’étudiants dans la base                   |




# Méthode 1 : `__init__`

### Signature :

```python
def __init__(self, host: str = "localhost", user: str = "root", password: str = "root", database: str = "db_students"):
```

### Rôle :

Cette méthode initialise une nouvelle instance de `DatabaseManager`. Elle ne se connecte pas directement à la base de données mais prépare tous les paramètres de connexion (hôte, utilisateur, mot de passe, nom de la base).

Ensuite, elle appelle immédiatement :

```python
self.create_tables()
```

Ce qui permet de s’assurer que la table `students_info` est bien créée dans la base, ou recréée si elle existe déjà.

---

### Ce que fait concrètement `__init__` :

1. Stocke les paramètres de connexion dans l’objet.
2. Appelle la méthode `create_tables()` pour garantir la structure de la table.
3. Permet l’utilisation ultérieure de `get_connection()` pour toute opération SQL.



## Quiz – Méthode `__init__` de la classe `DatabaseManager`

**Barème :**
Bonne réponse → **+1 point**
Mauvaise réponse → **-1 point**
Pas de réponse → **0 point**



### 1. Quel est le rôle de la méthode `__init__` dans `DatabaseManager` ?

* a) Elle exécute toutes les requêtes SQL
* b) Elle initialise les paramètres de connexion et appelle `create_tables()`
* c) Elle compte les étudiants dans la base
* d) Elle se connecte directement à la base et envoie une requête SELECT

**Bonne réponse : b**



### 2. Quel est le nom par défaut de la base de données utilisé dans `__init__` ?

* a) students
* b) info\_students
* c) db\_students
* d) database\_info

**Bonne réponse : c**



### 3. Pourquoi appelle-t-on `self.create_tables()` dans le constructeur ?

* a) Pour vider les tables de la base
* b) Pour initialiser les champs de formulaire
* c) Pour créer la table `students_info` si elle n’existe pas
* d) Pour insérer un étudiant test

**Bonne réponse : c**


### 4. Quels paramètres sont acceptés par la méthode `__init__` ?

* a) id, name, email
* b) host, user, password, database
* c) port, ssl, charset
* d) city, state, count

**Bonne réponse : b**



### 5. Le constructeur ouvre-t-il une connexion permanente à la base ?

* a) Oui, il utilise `connect()` une seule fois
* b) Non, il définit les paramètres mais ne connecte pas
* c) Oui, et la connexion reste ouverte
* d) Non, la connexion est toujours ouverte ailleurs

**Bonne réponse : b**


<br/>
<br/>

# Méthode 2

## Explication – Méthode `get_connection()`

### Signature :

```python
def get_connection(self):
```

### Rôle :

Cette méthode tente de créer une connexion MySQL en utilisant les attributs définis dans `__init__` : `host`, `user`, `password`, `database`.

### Fonctionnement :

1. Elle utilise `mysql.connector.connect(...)` pour se connecter à la base.
2. Si la connexion réussit, elle **retourne l’objet connexion**.
3. Si une erreur survient, elle affiche un message et retourne `None`.

### Code clé :

```python
try:
    conn = mysql.connector.connect(...)
    return conn
except Error as e:
    print(f"Erreur de connexion à la base de données MySQL: {e}")
    return None
```

---

### Où est-elle appelée ?

**Presque toutes les autres méthodes** de `DatabaseManager` l'appellent, comme :

* `create_tables()`
* `add_student(...)`
* `get_all_students()`
* `update_student(...)`
* `delete_student(...)`
* `search_students(...)`
* `get_states()`
* `get_cities_by_state(...)`
* `clear_all_students()`
* `get_student_count()`

Chaque fois, elle est utilisée ainsi :

```python
conn = self.get_connection()
if conn:
    ...
```

Cela permet :

* D’avoir une **connexion courte durée** (ouverte/fermée pour chaque requête).
* D’assurer une **gestion d’erreur locale** (pas de crash global).





## Quiz – Méthode `get_connection()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. À quoi sert la méthode `get_connection()` ?

* a) Elle envoie des requêtes SQL à la base
* b) Elle retourne une connexion MySQL configurée
* c) Elle ferme la base de données
* d) Elle initialise la table `students_info`

**Bonne réponse : b**

---

### 2. Que retourne `get_connection()` si la connexion échoue ?

* a) Une exception est levée
* b) Une chaîne de caractères
* c) Rien du tout
* d) La valeur `None`

**Bonne réponse : d**

---

### 3. Quel module Python est utilisé pour créer la connexion dans `get_connection()` ?

* a) sqlite3
* b) mysql.connector
* c) sqlalchemy
* d) pymysql

**Bonne réponse : b**

---

### 4. Dans quelles méthodes `get_connection()` est-elle utilisée ?

* a) Uniquement dans `__init__()`
* b) Seulement dans `add_student()`
* c) Dans toutes les méthodes qui accèdent à la base
* d) Elle n’est jamais utilisée

**Bonne réponse : c**

---

### 5. Pourquoi referme-t-on la connexion dans un bloc `finally` après chaque appel ?

* a) Pour éviter une exception
* b) Pour économiser la mémoire de Python
* c) Pour garantir que la connexion est bien fermée, même en cas d'erreur
* d) Pour vérifier si l'utilisateur est encore connecté

**Bonne réponse : c**





<br/>
<br/>

# Méthode 3



## Méthode 3 : `create_tables()`

### Signature :

```python
def create_tables(self):
```

### Rôle :

Cette méthode garantit que la table `students_info` existe dans la base de données avec le **schéma exact** attendu.

Elle est **appelée automatiquement dans le constructeur `__init__()`**, ce qui permet de préparer l’environnement dès le lancement de l’application.

---

### Fonctionnement détaillé :

1. Elle appelle `get_connection()` pour obtenir une connexion valide.
2. Si la connexion est active :

   * Elle **supprime la table `students_info`** si elle existe déjà (`DROP TABLE IF EXISTS`)
   * Elle **recrée** la table avec le schéma SQL complet (`CREATE TABLE ...`)
3. Elle utilise `conn.commit()` pour valider les changements.
4. Elle gère les erreurs SQL avec `try/except` et ferme proprement la connexion avec `finally`.

---

### Pourquoi un `DROP TABLE` ?

Parce que cette méthode suppose qu’on souhaite toujours repartir avec une table **propre et vide** à chaque redémarrage. C’est **acceptable en phase de développement**, mais risqué en production.

---

### Structure SQL de la table créée :

* `studentId` (clé primaire, auto-incrémentée)
* `firstName` (obligatoire)
* `lastName`, `state`, `city`, `emailAddress` (optionnels sauf email utilisé plus tard pour vérification d’unicité dans l’application)

---

## Quiz – Méthode `create_tables()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle principal de `create_tables()` ?

* a) Compter les lignes de la table
* b) Nettoyer la base de données
* c) Créer (ou recréer) la table `students_info` dans MySQL
* d) Afficher les colonnes existantes

**Bonne réponse : c**

---

### 2. Pourquoi la méthode utilise-t-elle la commande `DROP TABLE IF EXISTS` ?

* a) Pour libérer de l’espace disque
* b) Pour afficher les erreurs
* c) Pour éviter une erreur si la table existe déjà
* d) Pour bloquer l’accès à la table

**Bonne réponse : c**

---

### 3. Quelle instruction SQL est utilisée pour définir la structure de la table ?

* a) INSERT INTO ...
* b) SELECT \* FROM ...
* c) CREATE TABLE ...
* d) ALTER TABLE ...

**Bonne réponse : c**

---

### 4. Où cette méthode est-elle appelée automatiquement ?

* a) Dans `add_student()`
* b) Dans `get_connection()`
* c) Dans le constructeur `__init__()`
* d) Elle doit être appelée manuellement

**Bonne réponse : c**

---

### 5. Quelle est la clé primaire de la table `students_info` ?

* a) emailAddress
* b) studentId
* c) fullName
* d) Aucun champ n’est défini comme clé

**Bonne réponse : b**















<br/>
<br/>

# Méthode 4





## Méthode 4 : `add_student(...)`

### Signature :

```python
def add_student(self, first_name: str, last_name: str, city: str, state: str, email: str) -> bool:
```

### Rôle :

Insère un **nouvel étudiant** dans la table `students_info` de la base de données MySQL.
Elle est appelée quand l’utilisateur clique sur **"Add"** dans l’interface graphique.

---

### Fonctionnement détaillé :

1. Elle appelle `get_connection()` pour obtenir une connexion active.
2. Elle prépare une requête SQL `INSERT INTO students_info (...) VALUES (...)`.
3. Elle utilise les paramètres reçus pour insérer proprement les données.
4. Si l’insertion réussit :

   * Elle valide avec `conn.commit()`
   * Affiche un message de confirmation dans la console
   * Retourne `True`
5. Si une erreur survient (doublon, problème SQL, etc.) :

   * Elle affiche un message d'erreur
   * Retourne `False`
6. La connexion est toujours **fermée proprement** dans le bloc `finally`.

---

### Exemple de requête exécutée :

```sql
INSERT INTO students_info (firstName, lastName, city, state, emailAddress)
VALUES ('Alice', 'Lemoine', 'Montréal', 'Québec', 'alice@example.com');
```

---

### Validation des doublons :

Bien que la méthode elle-même **ne vérifie pas explicitement les doublons**, elle repose sur une **contrainte d’unicité** ou sur la gestion des erreurs MySQL pour empêcher les conflits (notamment sur l'email).

---

## Quiz – Méthode `add_student(...)`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le but principal de la méthode `add_student()` ?

* a) Supprimer un étudiant
* b) Ajouter un étudiant dans la base de données
* c) Compter les lignes de la table
* d) Vider la base de données

**Bonne réponse : b**

---

### 2. Quelle instruction SQL est utilisée dans cette méthode ?

* a) DELETE
* b) SELECT
* c) UPDATE
* d) INSERT INTO

**Bonne réponse : d**

---

### 3. Que retourne la méthode si l’insertion est un succès ?

* a) True
* b) False
* c) Le nombre d'étudiants
* d) None

**Bonne réponse : a**

---

### 4. Quand la connexion à la base est-elle fermée ?

* a) Jamais
* b) Seulement en cas de succès
* c) Toujours, dans un bloc `finally`
* d) À la prochaine requête

**Bonne réponse : c**

---

### 5. Que se passe-t-il si l’email est déjà présent dans la base ?

* a) L’étudiant est remplacé
* b) L’opération est annulée et False est retourné
* c) L’étudiant est ajouté quand même
* d) La méthode interroge l'utilisateur

**Bonne réponse : b**












<br/>
<br/>

# Méthode 5






## Méthode 5 : `get_all_students()`

### Objectif pédagogique :

Faire comprendre **pourquoi** cette méthode est indispensable dans toute application CRUD, **comment** elle fonctionne, et **comment elle s’intègre** dans l’interface utilisateur.

---

### 1. Pourquoi a-t-on besoin de `get_all_students()` ?

Imagine une application de gestion d’étudiants. Lorsqu'on ouvre l’application, **l’utilisateur doit voir tous les étudiants enregistrés** dans la base. Il ne s’agit pas d’afficher un seul étudiant, ni de faire une recherche filtrée, mais bien de **charger l’ensemble des enregistrements**.

Cette méthode est donc **essentielle pour initialiser l’affichage** dans l’interface (dans le `tableWidget`) et pour **recharger les données après chaque ajout, modification ou suppression**.

---

### 2. Code de la méthode :

```python
def get_all_students(self) -> List[Tuple]:
    """
    Retrieve all students from the database
    Returns:
        List of tuples containing student data
    """
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("SELECT * FROM students_info ORDER BY studentId")
            students = cursor.fetchall()
            return students
        except Error as e:
            print(f"Erreur lors de la récupération des étudiants: {e}")
            return []
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return []
```

---

### 3. Analyse ligne par ligne :

| Ligne de code                                                      | Explication pédagogique                               |
| ------------------------------------------------------------------ | ----------------------------------------------------- |
| `conn = self.get_connection()`                                     | Ouvre une connexion vers MySQL                        |
| `cursor = conn.cursor()`                                           | Crée un curseur pour envoyer une requête              |
| `cursor.execute("SELECT * FROM students_info ORDER BY studentId")` | Récupère tous les étudiants, triés par leur ID        |
| `students = cursor.fetchall()`                                     | Charge tous les résultats dans une liste de tuples    |
| `return students`                                                  | Retourne les données à la partie appelante            |
| `except` et `finally`                                              | Gèrent les erreurs et ferment proprement la connexion |

---

### 4. Que retourne la méthode ?

Une **liste de tuples** comme :

```python
[
  (1, 'Alice', 'Dupont', 'Québec', 'Montréal', 'alice@example.com'),
  (2, 'Jean', 'Durand', 'Ontario', 'Toronto', 'jean@example.ca'),
  ...
]
```

Chaque tuple représente un étudiant avec tous ses champs.

---

## Quiz – Méthode `get_all_students()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Pourquoi la méthode `get_all_students()` est-elle indispensable dans l’application ?

* a) Elle efface les étudiants
* b) Elle permet d’insérer un étudiant
* c) Elle affiche tous les étudiants dans le tableau à l’ouverture ou après modification
* d) Elle vérifie les adresses email

**Bonne réponse : c**

---

### 2. Quel type de données cette méthode retourne-t-elle ?

* a) Une seule chaîne de caractères
* b) Une liste de dictionnaires
* c) Une liste de tuples représentant chaque étudiant
* d) Un booléen

**Bonne réponse : c**

---

### 3. Que fait la commande SQL utilisée dans cette méthode ?

* a) Elle modifie les étudiants
* b) Elle supprime des lignes
* c) Elle sélectionne toutes les lignes de la table `students_info`
* d) Elle crée une nouvelle table

**Bonne réponse : c**

---

### 4. Pourquoi utilise-t-on `ORDER BY studentId` dans la requête ?

* a) Pour trier les colonnes par nom
* b) Pour trier les résultats du plus récent au plus ancien
* c) Pour afficher les étudiants dans l’ordre de leur ID (croissant)
* d) Ce n’est pas nécessaire, juste décoratif

**Bonne réponse : c**

---

### 5. Que retourne la méthode en cas d’erreur SQL ?

* a) None
* b) Une chaîne d’erreur
* c) Une liste vide
* d) Un booléen

**Bonne réponse : c**












<br/>
<br/>

# Méthode 6






## Méthode 6 : `get_student_by_id(student_id: int)`

### Objectif pédagogique :

Faire comprendre **comment filtrer une seule ligne spécifique** dans une base de données relationnelle (MySQL) à partir d’un identifiant unique (clé primaire).

---

### 1. Quel est le besoin métier ?

Dans une application de gestion d’étudiants, il faut parfois :

* Afficher **les détails d’un étudiant sélectionné**
* Pré-remplir un formulaire pour modification
* Vérifier l’existence d’un étudiant

La méthode `get_student_by_id()` répond à ce besoin :

> **"Donne-moi l’étudiant dont l’ID est exactement celui-ci."**

---

### 2. Code de la méthode

```python
def get_student_by_id(self, student_id: int) -> Optional[Tuple]:
    """
    Retrieve a specific student by ID
    Args:
        student_id: The student's ID
    Returns:
        Tuple containing student data or None if not found
    """
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("SELECT * FROM students_info WHERE studentId = %s", (student_id,))
            student = cursor.fetchone()
            return student
        except Error as e:
            print(f"Erreur lors de la récupération de l'étudiant: {e}")
            return None
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return None
```

---

### 3. Analyse pédagogique

| Élément                                            | Explication                                               |
| -------------------------------------------------- | --------------------------------------------------------- |
| `student_id`                                       | Identifiant numérique unique passé en argument            |
| `SELECT * FROM students_info WHERE studentId = %s` | Requête paramétrée pour éviter les injections SQL         |
| `cursor.fetchone()`                                | Récupère **une seule ligne** (le premier résultat trouvé) |
| `return student` ou `None`                         | Retourne un `tuple` ou `None` si aucun résultat           |
| Connexion fermée dans `finally`                    | Bonne pratique, toujours respecter la fermeture propre    |

---

### 4. Exemple de retour :

```python
(3, "Lina", "Mercier", "Québec", "Gatineau", "lina@example.com")
```

---

## Quiz – Méthode `get_student_by_id()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle de la méthode `get_student_by_id()` ?

* a) Supprimer un étudiant
* b) Insérer un nouvel étudiant
* c) Récupérer les informations d’un seul étudiant par son ID
* d) Afficher tous les étudiants

**Bonne réponse : c**

---

### 2. Quel mot-clé SQL est utilisé dans cette méthode ?

* a) INSERT
* b) SELECT
* c) DELETE
* d) UPDATE

**Bonne réponse : b**

---

### 3. Que retourne la méthode si aucun étudiant n’est trouvé avec l’ID fourni ?

* a) Une chaîne vide
* b) Une liste vide
* c) False
* d) None

**Bonne réponse : d**

---

### 4. Quelle fonction est utilisée pour obtenir une seule ligne ?

* a) `fetchall()`
* b) `fetchone()`
* c) `commit()`
* d) `execute()`

**Bonne réponse : b**

---

### 5. Pourquoi utilise-t-on `(student_id,)` au lieu de `(student_id)` dans l’exécution de la requête ?

* a) Pour forcer Python à convertir en float
* b) Pour envoyer une liste
* c) Pour former un tuple d’un seul élément (syntaxe correcte)
* d) Ce n’est pas nécessaire

**Bonne réponse : c**






<br/>
<br/>

# Méthode 7







## Méthode 7 : `update_student(...)`

### Signature :

```python
def update_student(self, student_id: int, first_name: str, last_name: str, city: str, state: str, email: str) -> bool:
```

---

### 1. Pourquoi a-t-on besoin de cette méthode ?

Lorsque l’utilisateur modifie les données d’un étudiant (changement d’adresse, correction d’un prénom, mise à jour d’email, etc.), il faut que ces modifications soient **enregistrées en base de données**.

Cette méthode est utilisée après :

* Une **sélection** d’un étudiant dans le tableau
* Une **modification** des champs dans le formulaire
* Un **clic** sur le bouton "Update"

---

### 2. Fonctionnement de la méthode

1. Ouvre une connexion avec `get_connection()`
2. Prépare une requête SQL `UPDATE ... WHERE studentId = %s`
3. Exécute cette requête avec les nouvelles valeurs
4. Vérifie si une ligne a été modifiée (`cursor.rowcount > 0`)
5. Retourne `True` en cas de succès, `False` sinon
6. Ferme toujours la connexion dans `finally`

---

### 3. Code de la méthode

```python
def update_student(self, student_id: int, first_name: str, last_name: str, 
                   city: str, state: str, email: str) -> bool:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("""
                UPDATE students_info 
                SET firstName = %s, lastName = %s, city = %s, state = %s, emailAddress = %s
                WHERE studentId = %s
            """, (first_name, last_name, city, state, email, student_id))
            
            if cursor.rowcount > 0:
                conn.commit()
                print(f"Étudiant ID {student_id} mis à jour avec succès.")
                return True
            else:
                print(f"Aucun étudiant trouvé avec l'ID {student_id}.")
                return False
        except mysql.connector.IntegrityError:
            print(f"Erreur: L'email {email} existe déjà dans la base de données.")
            return False
        except Error as e:
            print(f"Erreur lors de la mise à jour de l'étudiant: {e}")
            return False
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return False
```

---

### 4. Comportements clés à retenir

* Ne modifie que l’étudiant correspondant à l’ID donné.
* Ne fait rien si aucun étudiant ne correspond.
* Protège contre les emails en double (cas d'erreur SQL).
* Confirme visuellement le succès dans la console.

---

## Quiz – Méthode `update_student(...)`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle de la méthode `update_student()` ?

* a) Ajouter un étudiant
* b) Récupérer tous les étudiants
* c) Modifier les informations d’un étudiant existant
* d) Supprimer un étudiant

**Bonne réponse : c**

---

### 2. Que fait la clause `WHERE studentId = %s` dans cette requête ?

* a) Elle sélectionne tous les étudiants
* b) Elle cible uniquement l’étudiant à mettre à jour
* c) Elle trie les résultats par ID
* d) Elle empêche les doublons

**Bonne réponse : b**

---

### 3. Que signifie `cursor.rowcount > 0` ?

* a) Que plusieurs étudiants ont été ajoutés
* b) Qu’au moins une ligne a été modifiée dans la base
* c) Que toutes les lignes ont été supprimées
* d) Que la requête a échoué

**Bonne réponse : b**

---

### 4. Pourquoi utilise-t-on un bloc `try / except / finally` ?

* a) Pour écrire du code plus rapide
* b) Pour tester si la base est vide
* c) Pour gérer les erreurs SQL et fermer proprement la connexion
* d) Pour éviter l'utilisation de commit()

**Bonne réponse : c**

---

### 5. Que retourne la méthode si aucun étudiant avec cet ID n’est trouvé ?

* a) True
* b) False
* c) None
* d) Une chaîne vide

**Bonne réponse : b**




<br/>
<br/>

# Méthode 8






## Méthode 8 : `delete_student(student_id: int)`

### Signature :

```python
def delete_student(self, student_id: int) -> bool:
```

---

### 1. Pourquoi a-t-on besoin de cette méthode ?

Dans une application de gestion d’étudiants, l’utilisateur doit pouvoir **supprimer un étudiant** de la base s’il n’est plus actif, ou en cas d’erreur d’enregistrement.

La méthode `delete_student()` permet d’exécuter cette action **de façon ciblée et sécurisée**, en supprimant **uniquement l’étudiant dont l’ID est précisé**.

---

### 2. Fonctionnement pédagogique de la méthode

1. Appelle `get_connection()` pour ouvrir une connexion à MySQL
2. Exécute une requête SQL :

   ```sql
   DELETE FROM students_info WHERE studentId = %s
   ```
3. Vérifie si une ligne a réellement été supprimée (`cursor.rowcount > 0`)
4. Retourne `True` si la suppression a eu lieu, `False` sinon
5. Affiche un message de confirmation ou d’erreur dans la console
6. Ferme proprement la connexion dans `finally`

---

### 3. Code complet

```python
def delete_student(self, student_id: int) -> bool:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("DELETE FROM students_info WHERE studentId = %s", (student_id,))
            
            if cursor.rowcount > 0:
                conn.commit()
                print(f"Étudiant ID {student_id} supprimé avec succès.")
                return True
            else:
                print(f"Aucun étudiant trouvé avec l'ID {student_id}.")
                return False
        except Error as e:
            print(f"Erreur lors de la suppression de l'étudiant: {e}")
            return False
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return False
```

---

### 4. Bonnes pratiques observées

* Utilise `cursor.rowcount` pour vérifier le résultat réel.
* Ne plante pas en cas d’erreur SQL.
* Ferme toujours la connexion même en cas d’erreur.
* Requêtes paramétrées (`%s`) pour **éviter les injections SQL**.

---

## Quiz – Méthode `delete_student()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle de la méthode `delete_student()` ?

* a) Supprimer tous les étudiants
* b) Supprimer un étudiant à partir de son ID
* c) Archiver un étudiant dans un fichier CSV
* d) Réinitialiser la base de données

**Bonne réponse : b**

---

### 2. Que se passe-t-il si l’ID fourni ne correspond à aucun étudiant ?

* a) Une exception est levée
* b) L’étudiant est inséré quand même
* c) Rien n’est supprimé et la méthode retourne `False`
* d) Tous les étudiants sont supprimés

**Bonne réponse : c**

---

### 3. Pourquoi vérifie-t-on `cursor.rowcount > 0` ?

* a) Pour vérifier si au moins une ligne a été modifiée
* b) Pour savoir combien d'étudiants sont connectés
* c) Pour trier les étudiants
* d) Pour afficher la date de suppression

**Bonne réponse : a**

---

### 4. Quelle est l'instruction SQL utilisée par cette méthode ?

* a) `SELECT`
* b) `DROP`
* c) `DELETE`
* d) `REMOVE`

**Bonne réponse : c**

---

### 5. Quelle est la meilleure pratique observée dans la gestion des erreurs ?

* a) On ignore les erreurs pour ne pas interrompre le programme
* b) On interrompt toujours le programme en cas d’erreur
* c) On utilise `try/except/finally` pour capturer les erreurs et fermer la connexion proprement
* d) On recommence la suppression jusqu'à réussite

**Bonne réponse : c**







<br/>
<br/>

# Méthode 9








## Méthode 9 : `search_students(search_term: str, search_field: str = "all")`

### Signature :

```python
def search_students(self, search_term: str, search_field: str = "all") -> List[Tuple]:
```

---

### 1. Pourquoi cette méthode est-elle essentielle ?

Dans une application de gestion, l’utilisateur doit pouvoir **rechercher un étudiant sans parcourir toute la liste**.
Cette méthode permet d’exécuter une **recherche souple** :

* soit dans **tous les champs** (`firstName`, `lastName`, `city`, `state`, `emailAddress`)
* soit dans **un champ spécifique**, selon `search_field`

Elle est utilisée après que l’utilisateur a **tapé un mot-clé** dans un champ de recherche, puis cliqué sur un bouton "Search".

---

### 2. Fonctionnement de la méthode

1. Appelle `get_connection()`
2. Si `search_field == "all"` :

   * Exécute une requête SQL avec plusieurs `OR`
3. Sinon, exécute une requête ciblée :

   ```sql
   SELECT * FROM students_info WHERE firstName LIKE %s ORDER BY studentId
   ```
4. Le `search_term` est transformé en :

   ```python
   "%valeur%"  →  LIKE %valeur%
   ```
5. Utilise `fetchall()` pour obtenir tous les résultats
6. Gère les erreurs SQL et retourne une liste vide si besoin
7. Ferme proprement la connexion

---

### 3. Code complet

```python
def search_students(self, search_term: str, search_field: str = "all") -> List[Tuple]:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            search_term = f"%{search_term}%"
            
            if search_field == "all":
                cursor.execute("""
                    SELECT * FROM students_info 
                    WHERE firstName LIKE %s OR lastName LIKE %s OR 
                          city LIKE %s OR state LIKE %s OR emailAddress LIKE %s
                    ORDER BY studentId
                """, (search_term, search_term, search_term, search_term, search_term))
            elif search_field in ["firstName", "lastName", "city", "state", "emailAddress"]:
                query = f"SELECT * FROM students_info WHERE {search_field} LIKE %s ORDER BY studentId"
                cursor.execute(query, (search_term,))
            else:
                return []
            
            students = cursor.fetchall()
            return students
        except Error as e:
            print(f"Erreur lors de la recherche: {e}")
            return []
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return []
```

---

### 4. Ce qu’il faut comprendre

* `LIKE` est utilisé pour des **recherches partielles** (pas besoin de correspondance exacte).
* La recherche peut être **multi-champs** ou **spécifique**.
* On renvoie **une liste de tuples** contenant les étudiants trouvés.
* En cas d’erreur, la méthode est **robuste** (elle retourne une liste vide).

---

## Quiz – Méthode `search_students(...)`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle de la méthode `search_students()` ?

* a) Afficher tous les étudiants
* b) Rechercher un ou plusieurs étudiants selon un mot-clé
* c) Créer une nouvelle base de données
* d) Supprimer tous les étudiants

**Bonne réponse : b**

---

### 2. Que signifie le mot-clé SQL `LIKE` dans cette méthode ?

* a) C’est une commande pour ajouter une ligne
* b) C’est une commande pour exécuter plusieurs requêtes
* c) C’est un opérateur de comparaison partielle (recherche floue)
* d) C’est un filtre exact (équivalent à =)

**Bonne réponse : c**

---

### 3. Que se passe-t-il si l’on utilise `search_field="all"` ?

* a) Rien n’est filtré
* b) Tous les champs (nom, ville, etc.) sont recherchés
* c) Seul le champ `firstName` est utilisé
* d) Les doublons sont ignorés

**Bonne réponse : b**

---

### 4. Quel type de résultat retourne cette méthode ?

* a) Une chaîne de caractères
* b) Un entier
* c) Une liste de tuples d’étudiants
* d) Un dictionnaire

**Bonne réponse : c**

---

### 5. Quel est le format de la variable `search_term` utilisée dans la requête SQL ?

* a) `"valeur exacte"`
* b) `"*motclé*"`
* c) `"%motclé%"`
* d) `"motclé#"`

**Bonne réponse : c**






<br/>
<br/>

# Méthode 10





## Méthode 10 : `get_states()`

### Signature :

```python
def get_states(self) -> List[str]:
```

---

### 1. Quel est le besoin métier ?

L’interface de l’application propose un **menu déroulant (comboBox)** qui liste toutes les **provinces (states)** présentes dans la base.
Mais il ne faut pas afficher **plusieurs fois la même province** si elle est utilisée par plusieurs étudiants.

Exemple :

```
Québec
Ontario
Alberta
```

…pas :

```
Québec
Québec
Québec
Ontario
```

C’est exactement le rôle de `get_states()` :

> **Renvoyer la liste unique (distincte) de toutes les provinces enregistrées.**

---

### 2. Fonctionnement de la méthode

1. Appelle `get_connection()`
2. Exécute une requête SQL :

   ```sql
   SELECT DISTINCT state FROM students_info WHERE state IS NOT NULL ORDER BY state
   ```
3. Utilise `fetchall()` pour récupérer toutes les lignes uniques
4. Construit une **liste de chaînes** à partir des résultats
5. Retourne cette liste
6. Gère les erreurs SQL
7. Ferme la connexion proprement

---

### 3. Code complet

```python
def get_states(self) -> List[str]:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("SELECT DISTINCT state FROM students_info WHERE state IS NOT NULL ORDER BY state")
            states = [row[0] for row in cursor.fetchall()]
            return states
        except Error as e:
            print(f"Erreur lors de la récupération des états: {e}")
            return []
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return []
```

---

### 4. Ce qu’il faut comprendre

* **`DISTINCT`** : garantit que chaque province apparaît **une seule fois**.
* On ajoute `WHERE state IS NOT NULL` pour **ignorer les champs vides**.
* Le résultat est transformé en **liste de chaînes** prêtes à être utilisées dans un `comboBox`.

---

## Quiz – Méthode `get_states()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle principal de la méthode `get_states()` ?

* a) Afficher tous les étudiants
* b) Récupérer la liste des provinces uniques dans la base
* c) Compter le nombre de villes
* d) Supprimer tous les étudiants d'une province

**Bonne réponse : b**

---

### 2. Que signifie l'instruction SQL `SELECT DISTINCT state ...` ?

* a) Elle sélectionne tous les états triés par ID
* b) Elle supprime les doublons dans les résultats
* c) Elle convertit les états en majuscules
* d) Elle sélectionne uniquement la première ligne

**Bonne réponse : b**

---

### 3. Pourquoi filtre-t-on avec `WHERE state IS NOT NULL` ?

* a) Pour ignorer les doublons
* b) Pour exclure les lignes vides ou nulles
* c) Pour optimiser les performances
* d) Pour convertir les chaînes en int

**Bonne réponse : b**

---

### 4. Quel type est retourné par la méthode `get_states()` ?

* a) Liste de tuples
* b) Liste de dictionnaires
* c) Liste de chaînes de caractères
* d) Une seule chaîne concaténée

**Bonne réponse : c**

---

### 5. À quoi sert cette méthode dans l’interface graphique ?

* a) À remplir le tableau d'étudiants
* b) À initialiser la comboBox des provinces
* c) À afficher les logs SQL
* d) À trier les villes par code postal

**Bonne réponse : b**




<br/>
<br/>

# Méthode 11




## Méthode 11 : `get_cities_by_state(state: str)`

### Signature :

```python
def get_cities_by_state(self, state: str) -> List[str]:
```

---

### 1. Pourquoi cette méthode est-elle utile ?

Dans l’interface, lorsqu’un utilisateur **sélectionne une province (state)** dans le menu déroulant, on veut lui proposer une liste des **villes (cities)** correspondant **uniquement à cette province**.

Par exemple, si la province sélectionnée est `Québec`, on veut afficher :

```
Montréal
Québec
Laval
Gatineau
Longueuil
Sherbrooke
```

Et non toutes les villes du Canada.

---

### 2. Fonctionnement de la méthode

1. Appelle `get_connection()`
2. Exécute une requête SQL paramétrée avec `state` :

   ```sql
   SELECT DISTINCT city 
   FROM students_info 
   WHERE state = %s AND city IS NOT NULL 
   ORDER BY city
   ```
3. Utilise `fetchall()` pour obtenir les résultats
4. Transforme les lignes en **liste de chaînes**
5. Gère les erreurs SQL
6. Ferme proprement la connexion

---

### 3. Code complet

```python
def get_cities_by_state(self, state: str) -> List[str]:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("SELECT DISTINCT city FROM students_info WHERE state = %s AND city IS NOT NULL ORDER BY city", (state,))
            cities = [row[0] for row in cursor.fetchall()]
            return cities
        except Error as e:
            print(f"Erreur lors de la récupération des villes: {e}")
            return []
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return []
```

---

### 4. Ce qu’il faut comprendre

* **`%s`** est une variable SQL sécurisée, utilisée pour éviter les injections.
* **`DISTINCT`** garantit qu’une ville n’est pas répétée.
* La méthode retourne **seulement les villes liées à une province donnée**.
* Elle est souvent déclenchée automatiquement par un signal Qt (comme `.currentTextChanged` dans l’interface).

---

## Quiz – Méthode `get_cities_by_state(state)`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Que fait la méthode `get_cities_by_state(state)` ?

* a) Elle renvoie toutes les villes de la base
* b) Elle supprime les villes de la province
* c) Elle renvoie la liste des villes associées à une province précise
* d) Elle affiche un message de bienvenue

**Bonne réponse : c**

---

### 2. Pourquoi utilise-t-on `WHERE state = %s` dans la requête ?

* a) Pour exclure les villes de cette province
* b) Pour filtrer uniquement les villes de la province sélectionnée
* c) Pour trier les états
* d) Pour mettre à jour les noms de villes

**Bonne réponse : b**

---

### 3. Que retourne cette méthode en cas d’erreur SQL ?

* a) None
* b) 0
* c) Une liste vide
* d) Une chaîne de type erreur

**Bonne réponse : c**

---

### 4. Pourquoi `city IS NOT NULL` est utilisé dans la requête ?

* a) Pour supprimer les villes nulles
* b) Pour exclure les champs vides de la liste retournée
* c) Pour compter les villes
* d) Pour afficher uniquement les capitales

**Bonne réponse : b**

---

### 5. Quel type de données est retourné par cette méthode ?

* a) Liste de tuples
* b) Liste de dictionnaires
* c) Liste de chaînes (`List[str]`)
* d) Dictionnaire clé-valeur

**Bonne réponse : c**






<br/>
<br/>

# Méthode 12




## Méthode 12 : `clear_all_students()`

### Signature :

```python
def clear_all_students(self) -> bool:
```

---

### 1. Pourquoi cette méthode est-elle utile ?

Cette méthode sert à **effacer tous les enregistrements** de la table `students_info`. Elle est très utile pour :

* **Réinitialiser** la base entre deux tests
* **Vider les données** en cas de mauvaise importation
* **Nettoyer** la base avant un déploiement
* Fournir un bouton “Réinitialiser” dans une interface admin

---

### 2. Fonctionnement

1. Ouvre une connexion via `get_connection()`
2. Lance la commande SQL :

   ```sql
   DELETE FROM students_info
   ```
3. Applique `conn.commit()` pour valider la suppression
4. Affiche combien de lignes ont été supprimées
5. Retourne `True` si l’opération a bien eu lieu, `False` sinon
6. Ferme toujours la connexion proprement

---

### 3. Code complet

```python
def clear_all_students(self) -> bool:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("DELETE FROM students_info")
            conn.commit()
            print(f"{cursor.rowcount} étudiants supprimés.")
            return True
        except Error as e:
            print(f"Erreur lors de la suppression des étudiants: {e}")
            return False
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return False
```

---

### 4. Détails importants

* Il ne faut **pas confondre** avec `DROP TABLE`, qui supprime la table elle-même.
* `DELETE FROM students_info` garde la **structure intacte**, seul le **contenu** est supprimé.
* `cursor.rowcount` permet de savoir combien de lignes ont été réellement affectées.

---

## Quiz – Méthode `clear_all_students()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est l'effet de la commande `DELETE FROM students_info` ?

* a) Elle supprime uniquement l’étudiant avec l’ID 1
* b) Elle vide totalement la table sans supprimer sa structure
* c) Elle supprime la table entière
* d) Elle renomme la table

**Bonne réponse : b**

---

### 2. Quelle méthode est utilisée pour valider définitivement la suppression dans la base ?

* a) `rollback()`
* b) `save()`
* c) `commit()`
* d) `close()`

**Bonne réponse : c**

---

### 3. Que retourne cette méthode si une erreur survient pendant l'exécution ?

* a) None
* b) False
* c) Une chaîne contenant l’erreur
* d) Zéro

**Bonne réponse : b**

---

### 4. Que permet de mesurer `cursor.rowcount` dans ce contexte ?

* a) Le nombre de tables supprimées
* b) Le nombre de colonnes modifiées
* c) Le nombre d’étudiants supprimés
* d) Le nombre de connexions ouvertes

**Bonne réponse : c**

---

### 5. Cette méthode est particulièrement adaptée pour :

* a) Ajouter un nouvel étudiant
* b) Vérifier les doublons
* c) Réinitialiser complètement la table avant un test
* d) Modifier une seule ligne

**Bonne réponse : c**



<br/>
<br/>

# Méthode 13



## Méthode 13 : `get_student_count()`

### Signature :

```python
def get_student_count(self) -> int:
```

---

### 1. Pourquoi cette méthode est-elle utile ?

Cette méthode retourne le **nombre total d'étudiants présents dans la base**, ce qui permet :

* D'afficher un **compteur en temps réel** dans l’interface utilisateur
* De faire des **statistiques simples** sans avoir à parcourir toute la table
* De **vérifier** si la base est vide ou non avant d'effectuer des actions (ex : exportation, affichage...)

---

### 2. Fonctionnement

1. Se connecte à la base via `get_connection()`
2. Exécute une requête SQL :

   ```sql
   SELECT COUNT(*) FROM students_info
   ```
3. Récupère la valeur retournée avec `cursor.fetchone()[0]`
4. Retourne cette valeur (entier)

---

### 3. Code complet

```python
def get_student_count(self) -> int:
    conn = self.get_connection()
    if conn:
        try:
            cursor = conn.cursor()
            cursor.execute("SELECT COUNT(*) FROM students_info")
            count = cursor.fetchone()[0]
            return count
        except Error as e:
            print(f"Erreur lors du comptage des étudiants: {e}")
            return 0
        finally:
            if conn.is_connected():
                cursor.close()
                conn.close()
    return 0
```

---

### 4. Points pédagogiques importants

* `SELECT COUNT(*)` retourne **une seule valeur** : le nombre total de lignes.
* `fetchone()` récupère **le premier (et unique) résultat**.
* La méthode est **très rapide** même si la base contient des milliers de lignes.
* Elle retourne toujours un **entier (int)**.

---

## Quiz – Méthode `get_student_count()`

**Barème :**
Bonne réponse → +1 point
Mauvaise réponse → -1 point
Pas de réponse → 0 point

---

### 1. Quel est le rôle principal de cette méthode ?

* a) Supprimer un étudiant
* b) Récupérer la liste complète des étudiants
* c) Compter le nombre d’étudiants présents dans la base
* d) Réinitialiser la table

**Bonne réponse : c**

---

### 2. Quelle commande SQL est utilisée pour effectuer le comptage ?

* a) `SELECT * FROM students_info`
* b) `DELETE FROM students_info`
* c) `SELECT COUNT(*) FROM students_info`
* d) `SHOW TABLES`

**Bonne réponse : c**

---

### 3. Que contient `cursor.fetchone()[0]` après exécution ?

* a) Une liste de lignes
* b) Le nom de la colonne
* c) Un booléen
* d) Le nombre total de lignes dans la table

**Bonne réponse : d**

---

### 4. Quelle est la valeur retournée en cas d’erreur ?

* a) None
* b) -1
* c) 0
* d) False

**Bonne réponse : c**

---

### 5. Cette méthode est typiquement utilisée pour :

* a) Remplir le tableau de la GUI
* b) Connaître la taille actuelle de la base d’étudiants
* c) Valider un formulaire
* d) Tester une requête INSERT

**Bonne réponse : b**



