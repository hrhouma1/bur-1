# Définition – Relation bidirectionnelle

Une **relation bidirectionnelle** signifie que **les deux objets liés** peuvent **s’accéder mutuellement** dans le code.

Autrement dit :

* Si un objet A est lié à un objet B,
* Et que B est aussi lié à A,
* Alors la relation est **bidirectionnelle**.



# Exemple concret – Étudiant ↔ CarteÉtudiant (relation 1:1)

### Objectif :

* Depuis un **étudiant**, on veut accéder à **sa carte**.
* Depuis une **carte**, on veut accéder à **l'étudiant**.



## Implémentation SQLAlchemy correcte (relation bidirectionnelle)

```python
class Etudiant(Base):
    __tablename__ = "etudiants"
    id = Column(Integer, primary_key=True)
    nom = Column(String)
    prenom = Column(String)

    carte = relationship("CarteEtudiant", back_populates="etudiant", uselist=False)

class CarteEtudiant(Base):
    __tablename__ = "cartes"
    id = Column(Integer, primary_key=True)
    numero = Column(String)
    etudiant_id = Column(Integer, ForeignKey("etudiants.id"), unique=True)

    etudiant = relationship("Etudiant", back_populates="carte")
```



## Explication des mots-clés

* `carte = relationship(...)` dans la classe `Etudiant` permet d'accéder à l'objet `CarteEtudiant`.
* `etudiant = relationship(...)` dans la classe `CarteEtudiant` permet d'accéder à l'objet `Etudiant`.
* `back_populates="..."` relie explicitement les deux relations entre elles.
* `uselist=False` indique que la relation est un-à-un (pas une liste).



## Illustration dans le code

```python
e = session.query(Etudiant).first()
print(e.carte.numero)  # Depuis l'étudiant, j'accède à sa carte

c = session.query(CarteEtudiant).first()
print(c.etudiant.nom)  # Depuis la carte, j'accède à l'étudiant
```



# Différence avec une relation unidirectionnelle

### Si on écrit uniquement :

```python
class Etudiant(Base):
    ...
    carte = relationship("CarteEtudiant")
```

Et qu'on **n'ajoute rien dans `CarteEtudiant`**, alors :

* L'étudiant peut accéder à la carte (`e.carte`)
* Mais la carte **ne peut pas accéder à l'étudiant** (`c.etudiant` n’existe pas)

Cela s'appelle une **relation unidirectionnelle**.



# Résumé

| Type de relation  | Description                                                |
| ----------------- | ---------------------------------------------------------- |
| Bidirectionnelle  | Les deux objets peuvent s'accéder l’un à l’autre (`A ↔ B`) |
| Unidirectionnelle | Un seul objet connaît l’autre (`A → B`, mais pas `B → A`)  |
| Mot-clé utilisé   | `back_populates` dans les deux classes (ou `backref`)      |

