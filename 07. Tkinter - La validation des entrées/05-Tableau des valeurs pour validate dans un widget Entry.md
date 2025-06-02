# 1 - Tableau des valeurs pour validate dans un widget Entry


Dans Tkinter, quand tu écris :

```python
champ = tk.Entry(root, validate="key", validatecommand=...)
```

le paramètre `validate="key"` peut être remplacé par **plusieurs autres valeurs prédéfinies** qui déterminent **à quel moment la validation est déclenchée**.

Voici le tableau **complet** des valeurs possibles pour `validate=` :


##  Tableau des valeurs pour `validate` dans un widget `Entry`

| **Valeur de `validate`**  | **Moment de déclenchement**                                                       |
| ------------------------- | --------------------------------------------------------------------------------- |
| `"focus"`                 | Quand le champ **reçoit ou perd le focus** (FocusIn ou FocusOut)                  |
| `"focusin"`               | Quand le champ **reçoit** le focus (équivalent à `<FocusIn>`)                     |
| `"focusout"`              | Quand le champ **perd** le focus (équivalent à `<FocusOut>`)                      |
| `"key"`                   | À **chaque frappe de touche** (avant l’ajout du caractère dans le champ)          |
| `"all"`                   | Sur **tous les événements ci-dessus** (focus + touche)                            |
| `"none"` (ou valeur vide) | Aucune validation automatique (c’est la valeur par défaut si tu ne précises rien) |



##  Exemple 1

```python
champ = tk.Entry(root,
                 validate="focusout",  # validation quand on quitte le champ
                 validatecommand=(root.register(ma_fonction), "%P"))
```



## Remarques

* `"key"` est le plus courant pour bloquer certaines frappes en direct.
* `"focusout"` est utile pour valider une saisie **quand l'utilisateur sort du champ** (ex : champ email ou mot de passe).
* `"all"` permet de tout capturer mais peut être **plus difficile à contrôler** car il est appelé très souvent.

# 2 -  Exemple qui illustre la différence entre `"key"` et `"focusout"` 



## Objectif de l'exemple

* Montrer comment `validate="key"` bloque les frappes invalides immédiatement.
* Montrer comment `validate="focusout"` déclenche la validation uniquement lorsqu'on sort du champ (permet de taper librement, mais vérifie à la fin).
* Afficher un message d’erreur pour chaque cas.

---

## Code complet

```python
import tkinter as tk
import re

def valider_key(texte):
    if re.fullmatch(r"[A-Za-z]{0,10}", texte):
        message_key.set("")
        return True
    return False

def erreur_key():
    message_key.set("Seulement des lettres (max 10)")

def valider_focusout(texte):
    if re.fullmatch(r"[0-9]{4}", texte):
        message_focusout.set("Code valide")
        champ_focusout.config(bg="white")
        return True
    else:
        message_focusout.set("Doit contenir exactement 4 chiffres")
        champ_focusout.config(bg="misty rose")
        return False

# Fenêtre principale
root = tk.Tk()
root.title("Comparaison validate='key' vs validate='focusout'")

# Messages pour chaque champ
message_key = tk.StringVar()
message_focusout = tk.StringVar()

# Champ avec validate="key" (bloque la saisie dès qu’elle est invalide)
champ_key = tk.Entry(root,
                     validate="key",
                     validatecommand=(root.register(valider_key), "%P"),
                     invalidcommand=root.register(erreur_key))
champ_key.pack(padx=10, pady=5)

label_key = tk.Label(root, textvariable=message_key, fg="blue")
label_key.pack()

# Champ avec validate="focusout" (valide uniquement en quittant le champ)
champ_focusout = tk.Entry(root,
                          validate="focusout",
                          validatecommand=(root.register(valider_focusout), "%P"))
champ_focusout.pack(padx=10, pady=5)

label_focusout = tk.Label(root, textvariable=message_focusout, fg="blue")
label_focusout.pack()

# Référence globale au champ focusout pour coloration
champ_focusout = champ_focusout

# Lancer l’interface
root.mainloop()
```



## Résultat attendu

* Le premier champ (`validate="key"`) bloque directement toute frappe invalide (lettres uniquement, max 10).
* Le second champ (`validate="focusout"`) permet de tout taper, mais la validation a lieu uniquement quand on clique ailleurs ou appuie sur `Tab`. Le fond devient rouge si la validation échoue.




#  Exemple 2




## Objectif 

* Montrer comment chaque mode (`key`, `focusout`, `all`) déclenche la validation à des moments différents.
* Afficher un message d’état distinct pour chaque champ.
* Permettre une comparaison concrète et visible du comportement de chacun.

---

## Code complet

```python
import tkinter as tk
import re

# Champ 1 — validate="key" : validation à chaque frappe
def valider_key(texte):
    if re.fullmatch(r"[a-zA-Z]{0,10}", texte):
        msg_key.set("")
        return True
    return False

def erreur_key():
    msg_key.set("Lettres uniquement, max 10 caractères")

# Champ 2 — validate="focusout" : validation à la sortie du champ
def valider_focusout(texte):
    if re.fullmatch(r"\d{4}", texte):
        champ_focusout.config(bg="white")
        msg_focusout.set("Code PIN valide")
        return True
    champ_focusout.config(bg="misty rose")
    msg_focusout.set("Doit être un code à 4 chiffres")
    return False

# Champ 3 — validate="all" : validation à chaque frappe, focusin, focusout
def valider_all(texte):
    if texte.lower() == "admin":
        champ_all.config(bg="white")
        msg_all.set("Nom accepté")
        return True
    champ_all.config(bg="misty rose")
    msg_all.set("Saisir exactement : admin")
    return False

# Création de la fenêtre
root = tk.Tk()
root.title("Comparaison validate = key, focusout, all")

# Messages pour chaque champ
msg_key = tk.StringVar()
msg_focusout = tk.StringVar()
msg_all = tk.StringVar()

# validate = "key"
champ_key = tk.Entry(root,
                     validate="key",
                     validatecommand=(root.register(valider_key), "%P"),
                     invalidcommand=root.register(erreur_key))
champ_key.pack(padx=10, pady=5)
tk.Label(root, textvariable=msg_key, fg="blue").pack()

# validate = "focusout"
champ_focusout = tk.Entry(root,
                          validate="focusout",
                          validatecommand=(root.register(valider_focusout), "%P"))
champ_focusout.pack(padx=10, pady=5)
tk.Label(root, textvariable=msg_focusout, fg="blue").pack()

# validate = "all"
champ_all = tk.Entry(root,
                     validate="all",
                     validatecommand=(root.register(valider_all), "%P"))
champ_all.pack(padx=10, pady=5)
tk.Label(root, textvariable=msg_all, fg="blue").pack()

# Références nécessaires pour modifier les couleurs
champ_focusout = champ_focusout
champ_all = champ_all

# Lancer l'application
root.mainloop()
```

---

## Comportement de chaque champ

| Champ     | `validate=`  | Déclenche la validation...                                      |
| --------- | ------------ | --------------------------------------------------------------- |
| 1er champ | `"key"`      | À chaque frappe                                                 |
| 2e champ  | `"focusout"` | Quand l'utilisateur quitte le champ (clic ailleurs, `Tab`)      |
| 3e champ  | `"all"`      | Lors de l'entrée dans le champ, à chaque frappe, et à la sortie |



