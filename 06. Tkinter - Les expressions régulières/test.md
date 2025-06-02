# Projet 1

## Projet 2

### Projet 3


```html
import re

texte = "Bienvenue à Université123"
motif = r"[A-Za-z]+[0-9]+"
resultat = re.search(motif, texte)

if resultat:
    print("Motif détecté :", resultat.group())
else:
    print("Aucune correspondance.")
```
