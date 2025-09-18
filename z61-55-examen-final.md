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

