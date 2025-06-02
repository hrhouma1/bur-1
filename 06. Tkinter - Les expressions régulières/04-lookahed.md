# Exemple : Comprendre `(?=...)` avec `Jack(?=Sprat)`

# 1 -  Motif : 

```
`Jack(?=Sprat)`
```


Ce motif vérifie la présence du mot **`Jack`**, **à condition qu’il soit immédiatement suivi de `Sprat`**, sans pour autant inclure `Sprat` dans le résultat final.

> Autrement dit, on accepte `Jack` **seulement s’il est directement suivi de `Sprat`**, sans aucun caractère entre les deux.





# 2 -  Décryptage du motif `Jack(?=Sprat)`

* `Jack` : c’est ce qu’on cherche à trouver.
* `(?=Sprat)` : signifie **“je vérifie qu’après Jack, il y a Sprat”**
  → **Attention** : cette vérification **n'ajoute pas Sprat** au résultat final.

Autrement dit :

> 🔎 *“Trouve-moi `Jack`, seulement si ce mot est suivi **juste après** par `Sprat`.”*



# 3 - Cas concrets

Testons plusieurs chaînes pour voir si ce motif fonctionne :

| Chaîne testée    | Résultat | Explication pédagogique                                                                                                                                              |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"Jack"`         | ❌        | Il n’y a **rien après `Jack`**, donc le lookahead échoue.                                                                                                            |
| `"Jack123Sprat"` | ❌        | `Sprat` est là, mais **pas juste après** : il y a `123` entre les deux.                                                                                              |
| `"JackSprat"`    | ✅        | `Jack` est **immédiatement suivi** de `Sprat` → motif validé.                                                                                                        |
| `"JackSprat123"` | ✅        | Pareil : on trouve `Jack`, il est suivi de `Sprat` → validé.                                                                                                         |
| `"JackSpratt"`   | ✅        | `Sprat` est au début de `Spratt`, donc la condition est satisfaite (le lookahead ne demande pas que ce soit **exactement** `Sprat`, juste que ça commence comme ça). |



# 4 -  Code Python pour illustrer

```python
import re

motif = r"Jack(?=Sprat)"
phrases = [
    "Jack",
    "Jack123Sprat",
    "JackSprat",
    "JackSprat123",
    "JackSpratt"
]

for phrase in phrases:
    resultat = re.search(motif, phrase)
    print(f"{phrase} → {'✅ trouvé' if resultat else '❌ non trouvé'}")
```

> Ce script montre comment le lookahead vérifie la présence de `Sprat` juste après `Jack`, sans inclure `Sprat` dans le résultat.



# 5 - Conclusion 

Le **lookahead** `(?=...)` est une manière élégante de **poser une condition sur ce qui suit**, **sans l’inclure**.
Dans notre exemple, il sert à restreindre `Jack` aux cas où il est suivi de `Sprat`, ce qui est très utile en **validation conditionnelle**.

