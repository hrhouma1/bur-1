# <h1 id="quiz-sqlalchemy-relations">Quiz – Relations et mots-clés SQLAlchemy ORM (20 questions)</h1>



### **Q1.** Quel mot-clé est utilisé pour définir une relation entre deux classes ORM ?

A. `ForeignKey`
B. `relationship`
C. `Column`
D. `bind`

**Réponse :** B
**Explication :** `relationship` crée le lien logique entre deux classes, tandis que `ForeignKey` définit la contrainte en base.



### **Q2.** Quelle option permet de forcer une relation un-à-un (1:1) dans `relationship()` ?

A. `nullable=True`
B. `cascade="delete"`
C. `uselist=False`
D. `unique=True`

**Réponse :** C
**Explication :** `uselist=False` empêche SQLAlchemy de retourner une liste.



### **Q3.** Quel mot-clé définit une contrainte de clé étrangère ?

A. `backref`
B. `relationship`
C. `ForeignKey`
D. `secondary`

**Réponse :** C
**Explication :** `ForeignKey("autretable.id")` crée la contrainte SQL de liaison.


### **Q4.** Quelle option permet de créer une relation bidirectionnelle explicite ?

A. `back_populates`
B. `backref`
C. `primary_key`
D. `nullable`

**Réponse :** A
**Explication :** `back_populates` doit être défini des deux côtés de la relation.



### **Q5.** Dans une relation 1\:N, quel mot-clé est utilisé dans la table "N" pour créer la liaison ?

A. `relationship(...)`
B. `ForeignKey(...)`
C. `backref="..."`
D. `secondary="..."`

**Réponse :** B
**Explication :** Le côté "N" contient une `ForeignKey` vers le côté "1".



### **Q6.** Que fait l'option `unique=True` dans une colonne `ForeignKey()` ?

A. Permet des doublons
B. Garantit que chaque valeur est différente
C. Autorise une relation 1\:N
D. Supprime les valeurs NULL

**Réponse :** B
**Explication :** `unique=True` garantit qu'une valeur ne peut apparaître qu'une seule fois.



### **Q7.** Quelle est la bonne combinaison pour une relation 1:1 ?

A. `ForeignKey` sans `unique`
B. `relationship` avec `uselist=False` et `ForeignKey(unique=True)`
C. `backref="..."` uniquement
D. `relationship` sans options

**Réponse :** B
**Explication :** C’est l’unique combinaison qui impose la contrainte 1:1.



### **Q8.** Quel mot-clé est utilisé pour créer une relation N\:N avec une table d’association ?

A. `cascade`
B. `unique`
C. `secondary`
D. `nullable`

**Réponse :** C
**Explication :** `secondary` pointe vers la table intermédiaire.



### **Q9.** Quelle structure doit être utilisée pour `secondary` ?

A. Une classe
B. Une fonction
C. Un objet `Table(...)`
D. Une chaîne de caractères

**Réponse :** C
**Explication :** `secondary` attend un objet de type `Table`, pas une classe ORM.



### **Q10.** Que se passe-t-il si on oublie `uselist=False` dans une relation 1:1 ?

A. SQLAlchemy renvoie `None`
B. La relation est automatiquement 1\:N
C. La base plante
D. Rien ne change

**Réponse :** B
**Explication :** Par défaut, `relationship` retourne une liste (relation 1\:N).



### **Q11.** Le mot-clé `nullable=False` signifie :

A. La valeur peut être absente
B. La valeur doit toujours être présente
C. La colonne est unique
D. La colonne est une clé primaire

**Réponse :** B
**Explication :** `nullable=False` interdit les valeurs NULL en base.



### **Q12.** À quoi sert `backref="nom"` dans `relationship()` ?

A. À forcer une jointure externe
B. À lier automatiquement le côté inverse de la relation
C. À rendre une clé primaire
D. À désactiver la relation

**Réponse :** B
**Explication :** `backref` crée dynamiquement le lien inverse dans la classe cible.



### **Q13.** Quel mot-clé permet de supprimer automatiquement les enfants liés lors de la suppression du parent ?

A. `cascade="all, delete"`
B. `back_populates`
C. `secondary`
D. `unique=True`

**Réponse :** A
**Explication :** `cascade="all, delete"` propage la suppression du parent aux enfants.



### **Q14.** Dans une relation N\:N, que contiennent les colonnes de la table d’association ?

A. Une chaîne
B. Deux `ForeignKey`
C. Deux `relationship`
D. Un seul `primary_key`

**Réponse :** B
**Explication :** Chaque ligne de la table d’association contient 2 clés étrangères (vers A et B).



### **Q15.** Que retourne une `relationship` sans `uselist=False` ?

A. Un seul objet
B. Une table
C. Une liste d’objets
D. Une chaîne de caractères

**Réponse :** C
**Explication :** C’est le comportement par défaut : liste d’objets liés.



### **Q16.** Quel est le rôle de `Base.metadata.create_all(engine)` ?

A. Ajouter une clé étrangère
B. Définir une contrainte
C. Créer physiquement les tables définies par les modèles
D. Effacer les anciennes données

**Réponse :** C
**Explication :** Cette méthode synchronise les modèles avec la base de données.



### **Q17.** Quel mot-clé est utilisé pour désigner le type SQL dans une colonne ?

A. `relationship`
B. `Column`
C. `Integer`, `String`
D. `ForeignKey`

**Réponse :** C
**Explication :** Ce sont les types SQLAlchemy à utiliser dans `Column(...)`.



### **Q18.** Quel attribut dans une `Column` indique qu’elle est une clé primaire ?

A. `foreign=True`
B. `key=True`
C. `primary=True`
D. `primary_key=True`

**Réponse :** D
**Explication :** `primary_key=True` identifie la colonne comme clé primaire.



### **Q19.** Dans une relation bidirectionnelle explicite, combien de `relationship()` faut-il déclarer ?

A. 0
B. 1
C. 2
D. 3

**Réponse :** C
**Explication :** Un de chaque côté, tous deux reliés par `back_populates`.



### **Q20.** Quelle est la bonne combinaison pour une relation N\:N entre `Etudiant` et `Cours` ?

A. Deux `relationship` sans table intermédiaire
B. Une table `Table()` + deux `relationship()` avec `secondary`
C. Une seule relation déclarée dans `Etudiant`
D. `ForeignKey` dans `Cours` vers `Etudiant`

**Réponse :** B
**Explication :** N\:N nécessite une table d’association et deux relations symétriques avec `secondary`.


