

## 1. `validate` et `validatecommand`

### Objectif :

`validate` permet de **contrôler directement ce qui est autorisé dans un champ `Entry`**, **avant** que la saisie ne soit appliquée.

### Fonctionnement :

* On utilise `validate="key"` pour valider **à chaque frappe**.
* La fonction référencée dans `validatecommand` doit **retourner `True` ou `False`**.

  * `True` → la saisie est autorisée
  * `False` → la saisie est bloquée
* Les arguments comme `%P`, `%S`, `%d` sont **substitués par Tkinter** (valeurs temporaires, pas des arguments Python classiques).

### Exemples d’usage :

* Empêcher d’écrire plus de 10 caractères.
* N’autoriser que des chiffres ou des lettres.
* Imposer un format précis (par exemple : code postal, email, etc.).

### Limites :

* `validatecommand` est parfois capricieux, surtout si mal configuré.
* Il ne déclenche pas toujours `invalidcommand` si l’état reste invalide.
* La fonction ne reçoit pas d'objet `event` comme avec `bind`.

---

## 2. `bind`

### Objectif :

`bind` permet de **réagir à des événements** de l’utilisateur (frappe clavier, clic souris, etc.), **sans bloquer la saisie**.

### Fonctionnement :

* On utilise `widget.bind("<KeyRelease>", fonction)` pour exécuter une action après chaque touche relâchée.
* La fonction reçoit un argument `event`, qui donne accès à :

  * La touche pressée
  * Le widget concerné
  * Les coordonnées du clic, etc.

### Exemples d’usage :

* Afficher un message après chaque frappe.
* Mettre le champ en rouge si la saisie est invalide.
* Enregistrer automatiquement un champ à chaque modification.
* Réagir à `Enter`, `Ctrl+C`, `Ctrl+V`, etc.

### Limites :

* `bind` n’empêche **jamais** la saisie : il agit **après** l’entrée de l’utilisateur.
* Il faut manuellement gérer les effets visuels ou les validations.

---

## 3. Comparaison approfondie

| Aspect                                 | `validate`                      | `bind`                                                 |
| -------------------------------------- | ------------------------------- | ------------------------------------------------------ |
| S'exécute avant/après la frappe        | Avant                           | Après                                                  |
| Peut empêcher la saisie                | Oui (en retournant `False`)     | Non (la saisie est déjà effectuée)                     |
| Retour de la fonction                  | Doit être `True` ou `False`     | Pas de contrainte, on peut faire ce qu'on veut         |
| Accès au texte du champ                | Via `%P`, `%S`, etc.            | Via `event.widget.get()`                               |
| Contrôle fin de saisie (`Enter`, etc.) | Non                             | Oui (ex: `<Return>`)                                   |
| Prise en charge des raccourcis clavier | Non                             | Oui                                                    |
| Support natif Tkinter (ancienne API)   | Oui                             | Oui, mais plus moderne et plus souple                  |
| Utilisation recommandée                | Pour bloquer la saisie invalide | Pour afficher un message, mettre à jour un champ, etc. |

---

## Conclusion

* **Non, ce n’est pas la même chose.**
* `validate` est un **mécanisme de contrôle** strict intégré à Tkinter.
* `bind` est un **mécanisme de réponse aux événements**, plus souple mais non bloquant.
* Dans les applications réelles, on utilise souvent **les deux ensemble** : `validate` pour restreindre, `bind` pour informer ou améliorer l'expérience utilisateur.

<br/>
<br/>

# Exercice 1 :

- Comparez ces deux codes :








## Code 1

```python
import tkinter as tk
import re

# Fonction de validation (empêche les frappes invalides)
def valider_texte(texte):
    # Autoriser uniquement lettres (A-Za-z), entre 0 et 10 caractères
    return bool(re.fullmatch(r"[A-Za-z]{0,10}", texte))

# Fonction de feedback dynamique (affichée après la frappe)
def verifier_apres_frappe(event):
    texte = champ.get()
    if re.fullmatch(r"[A-Za-z]{3,10}", texte):
        info.set("OK")
        champ.config(bg="white")
    else:
        info.set("Saisir 3 à 10 lettres uniquement.")
        champ.config(bg="misty rose")

# Fenêtre principale
root = tk.Tk()
root.title("Validation mixte : validate + bind")

# Variable pour afficher le message
info = tk.StringVar()

# Enregistrement de la fonction validate
cmd_valider = root.register(valider_texte)

# Champ de saisie avec validate
champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(cmd_valider, "%P"))
champ.pack(padx=10, pady=5)

# Message affiché après la frappe
label = tk.Label(root, textvariable=info, fg="blue")
label.pack()

# Liaison avec bind (feedback visuel)
champ.bind("<KeyRelease>", verifier_apres_frappe)

# Boucle Tkinter
root.mainloop()
```

---

## Explication

| Élément                | Rôle                                                                  |
| ---------------------- | --------------------------------------------------------------------- |
| `validate="key"`       | Active la validation **avant** l’insertion dans le champ              |
| `validatecommand`      | Fonction qui retourne `True` si la frappe est correcte, sinon `False` |
| `bind("<KeyRelease>")` | Exécute une fonction après chaque frappe (même si rien n’a changé)    |
| `StringVar()`          | Permet d’afficher dynamiquement des messages                          |
| `champ.config(bg=...)` | Change la couleur du champ en fonction de l’état (facultatif)         |



##  Objectif du code 1  avec `bind`

* L’utilisateur doit entrer **entre 3 et 10 lettres uniquement**.
* Si l'utilisateur tape autre chose, la frappe est **bloquée** (via `validate`).
* Un message d'information apparaît **en direct** (via `bind`).
* Le champ change de couleur si l’entrée devient invalide.



##  Objectif du code 2  avec `invalidcommand`


- Je vous présente **le même exemple**, mais cette fois **sans utiliser `bind`**.
- On remplace `bind(...)` par `invalidcommand`, qui est déclenché **automatiquement lorsque `validatecommand` retourne False**.

L'objectif est d':
* Empêcher la frappe si le texte contient autre chose que des lettres (A–Z, a–z) ou dépasse 10 caractères.
* Afficher un message d’erreur et changer la couleur du champ si la frappe est invalide.
* Ne pas utiliser `bind`, uniquement `validate` + `invalidcommand`.



## Code  avec `invalidcommand`

```python
import tkinter as tk
import re

# Fonction de validation stricte : bloque la frappe invalide
def valider_texte(texte):
    if re.fullmatch(r"[A-Za-z]{0,10}", texte):
        info.set("")  # Efface l’erreur si valide
        champ.config(bg="white")
        return True
    else:
        return False  # Ne pas autoriser la saisie invalide

# Fonction appelée quand la saisie est rejetée (invalidcommand)
def saisie_invalide():
    info.set("Erreur : lettres uniquement (max 10)")
    champ.config(bg="misty rose")

# Fenêtre principale
root = tk.Tk()
root.title("Validation stricte avec invalidcommand")

# Variable pour message d'erreur
info = tk.StringVar()

# Enregistrement des fonctions
valide = root.register(valider_texte)
invalide = root.register(saisie_invalide)

# Champ avec validation
champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(valide, "%P"),
                 invalidcommand=invalide)
champ.pack(padx=10, pady=5)

# Message visuel
label = tk.Label(root, textvariable=info, fg="blue")
label.pack()

# Lancer l’interface
root.mainloop()
```

---

## Différences clés avec `bind`

| Point de comparaison          | `bind("<KeyRelease>")` | `invalidcommand`                                |
| ----------------------------- | ---------------------- | ----------------------------------------------- |
| Déclenchement                 | Après chaque frappe    | Seulement quand la saisie est bloquée (`False`) |
| Peut réagir en continu ?      | Oui                    | Non (pas si le texte reste invalide)            |
| Besoin de vérifier soi-même ? | Oui (manuellement)     | Non, déclenché automatiquement si invalid       |

---

## Limite connue de `invalidcommand`

`invalidcommand` **ne se relance pas** tant que le champ reste invalide.
Exemple : si on tape `1`, puis `2`, puis `3`, l’erreur n’est **pas réaffichée** à chaque frappe.



