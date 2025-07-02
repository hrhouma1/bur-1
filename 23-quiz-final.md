## Quiz – Fichier `connect_database.py` et classe `DatabaseManager`

> **Objectif pédagogique :** Aider l’étudiant à bien comprendre :
>
> * Comment la **connexion à la base MySQL** est centralisée,
> * Comment les **méthodes utilisent cette connexion**,
> * Et pourquoi cette organisation est importante pour la **maintenabilité du code**.


# 1. Quelle est la responsabilité principale de la classe `DatabaseManager` ?

* a) Gérer les fichiers de l’interface Qt
* b) Représenter les étudiants dans l’interface graphique
* c) Gérer la connexion et les requêtes vers la base de données
* d) Organiser les signaux Qt

**Bonne réponse :**

* c) Gérer la connexion et les requêtes vers la base de données \*



# 2. Où et quand la connexion à la base de données est-elle initialisée ?

```python
def __init__(self, host="localhost", user="root", password="root", database="db_students"):
    self.conn = mysql.connector.connect(...)
```

* a) Lors de l’appel à `connect()` dans chaque méthode
* b) Lors de l’appel à `app.exec()`
* c) Une seule fois, à l’instanciation de `DatabaseManager` dans `MainWindow.__init__()`
* d) Au moment de lancer le script `main.py`

**Bonne réponse :**

* c) Une seule fois, à l’instanciation de `DatabaseManager` dans `MainWindow.__init__()` \*



# 3. Est-ce que chaque méthode (`get_all_students`, `add_student`, etc.) ouvre une nouvelle connexion ?

* a) Oui, à chaque requête SQL une nouvelle connexion est créée
* b) Non, elles réutilisent l’objet `self.conn` instancié dans `__init__`
* c) Cela dépend du système d’exploitation
* d) Non, car la base est stockée en mémoire

**Bonne réponse :**

* b) Non, elles réutilisent l’objet `self.conn` instancié dans `__init__` \*



# 4. Quel est le rôle typique de `cursor = self.conn.cursor()` dans chaque méthode ?

* a) Créer une nouvelle base
* b) Lire et écrire dans un fichier CSV
* c) Préparer et exécuter une requête SQL
* d) Lancer une nouvelle fenêtre Qt

**Bonne réponse :**

* c) Préparer et exécuter une requête SQL \*



# 5. Pourquoi appelle-t-on `self.conn.commit()` après un `INSERT`, `UPDATE` ou `DELETE` ?

* a) Pour augmenter la vitesse
* b) Pour synchroniser avec l’interface Qt
* c) Pour s’assurer que la modification est bien enregistrée dans la base
* d) Pour quitter le programme

**Bonne réponse :**

* c) Pour s’assurer que la modification est bien enregistrée dans la base \*



# 6. Que risque-t-on si on oublie `commit()` après une insertion ?

* a) L’application plante
* b) Rien de grave, c’est automatique
* c) Les données ne seront pas vraiment enregistrées
* d) Le curseur SQL est réinitialisé

**Bonne réponse :**

* c) Les données ne seront pas vraiment enregistrées \*



# 7. Pourquoi centraliser toutes les requêtes SQL dans une seule classe comme `DatabaseManager` ?

* a) Pour tout mettre dans un seul fichier
* b) Pour isoler la logique d’accès aux données et favoriser la maintenance
* c) Pour éviter d’écrire du SQL dans Qt Designer
* d) Pour ne pas utiliser PyMySQL

**Bonne réponse :**

* b) Pour isoler la logique d’accès aux données et favoriser la maintenance \*



# 8. À quel moment la connexion à la base est-elle refermée dans ce projet ?

* a) Lorsqu’on ferme l’application
* b) Automatiquement par Python (garbage collector)
* c) Jamais explicitement dans le code visible
* d) À la fin de chaque requête

**Bonne réponse :**

* c) Jamais explicitement dans le code visible \*

> *(Remarque : Il serait bon de prévoir une méthode `close()` dans `DatabaseManager` pour gérer ça correctement.)*



# 9. Quel avantage apporte cette structure orientée objet à l’échelle d’un projet plus grand ?

* a) Elle permet de réutiliser le même objet pour tous les appels à la base
* b) Elle évite les doublons de code SQL
* c) Elle permet de tester plus facilement les méthodes
* d) Toutes les réponses ci-dessus

**Bonne réponse :**

* d) Toutes les réponses ci-dessus \*



# 10. Cette architecture respecte-t-elle le principe de séparation des responsabilités ?

* a) Non, le SQL est mélangé à l’interface graphique
* b) Oui, l’accès aux données est séparé de l’interface Qt
* c) Non, car tout est dans un seul fichier
* d) Cela dépend du nombre d’étudiants

**Bonne réponse :**

* b) Oui, l’accès aux données est séparé de l’interface Qt \*


