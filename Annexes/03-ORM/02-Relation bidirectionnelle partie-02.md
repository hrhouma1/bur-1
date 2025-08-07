# Rappel: 

- Une **relation bidirectionnelle** en SQLAlchemy signifie que **les deux objets liés peuvent accéder l’un à l’autre directement**.

##  Définition simple

> Si A est lié à B, et que B est aussi lié à A, alors la relation est **bidirectionnelle**.



##  Exemple concret : Étudiant ↔ Carte Étudiant (relation 1:1)

### Objectif :

* Depuis un **étudiant**, je peux accéder à **sa carte**.
* Depuis une **carte**, je peux accéder à **son étudiant**.


##  Implémentation SQLAlchemy

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

### Explication

* `carte` est l’attribut de `Etudiant` pointant vers `CarteEtudiant`
* `etudiant` est l’attribut de `CarteEtudiant` pointant vers `Etudiant`
* Les deux sont liés par `back_populates="..."`



##  Illustration (relation bidirectionnelle)

```
etudiant.carte      --> donne la carte liée
carte.etudiant      --> donne l'étudiant lié
```

C’est ça une **relation bidirectionnelle** : les deux objets peuvent **naviguer l’un vers l’autre**.



##  À l’inverse : relation unidirectionnelle

Si tu n’écris que :

```python
class Etudiant(Base):
    ...
    carte = relationship("CarteEtudiant")
```

Alors **seul l’étudiant peut accéder à la carte**, mais **la carte ne peut pas accéder à l’étudiant**. C’est **unidirectionnel**.



##  Résumé

| Type de relation      | Comportement                                           |
| --------------------- | ------------------------------------------------------ |
| **Bidirectionnelle**  | A → B et B → A via `relationship(..., back_populates)` |
| **Unidirectionnelle** | A → B uniquement, pas de retour B → A                  |


