# **Exercices Tkinter Simples – Validation des Champs (Interface complète)**



## **Environnement virtuel pour tous les exercices (Windows)**

```bash
mkdir projet_tkinter_validations
cd projet_tkinter_validations
python -m venv env
env\Scripts\activate
pip install tk
```

> Cette procédure permet d'isoler le projet et d'éviter les conflits avec d'autres bibliothèques Python installées.

---

### **Exercice 1 – Saisir un nombre entre 0 et 999**

```python
import tkinter as tk

def valider(texte):
    return texte.isdigit() and 0 <= int(texte) <= 999 if texte else True

root = tk.Tk()
root.title("Saisir un nombre entre 0 et 999")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> La fonction `valider()` autorise les nombres uniquement s’ils sont numériques (`isdigit`) et entre 0 et 999.
> La chaîne vide est acceptée temporairement pour permettre la suppression.

---

### **Exercice 2 – Champ pour lettres majuscules uniquement**

```python
import tkinter as tk
import re

def valider(texte):
    return bool(re.fullmatch(r"[A-Z]*", texte))

root = tk.Tk()
root.title("Lettres MAJUSCULES uniquement")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Le motif `[A-Z]*` autorise uniquement les lettres majuscules et permet une saisie progressive.

---

### **Exercice 3 – Nom de fichier se terminant par .py**

```python
import tkinter as tk
import re

def valider(texte):
    return bool(re.fullmatch(r"\w+\.py", texte))

root = tk.Tk()
root.title("Nom de fichier Python (.py)")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Ce motif n’autorise que les noms comme `script.py`, `test1.py`, etc.
> Le `\.` est nécessaire pour désigner un **point réel**, car `.` est un métacaractère.

---

### **Exercice 4 – Âge valide (entre 1 et 120)**

```python
import tkinter as tk

def valider_age(txt):
    if txt == "":
        return True
    return txt.isdigit() and 1 <= int(txt) <= 120

root = tk.Tk()
root.title("Âge entre 1 et 120")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_age), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> La validation est acceptée si l’entrée est vide ou si le nombre est compris entre 1 et 120.
> Cela convient aux formulaires d’inscription par exemple.

---

### **Exercice 5 – Email simple (ex: [nom@site.com](mailto:nom@site.com))**

```python
import tkinter as tk
import re

def valider_email(txt):
    motif = r"[\w\.-]+@[\w-]+\.[a-z]{2,4}"
    return bool(re.fullmatch(motif, txt))

root = tk.Tk()
root.title("Adresse email")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_email), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Ce motif est une version simplifiée qui couvre des cas usuels comme `test@gmail.com` ou `prenom.nom@domaine.org`.

---

### **Exercice 6 – Champ avec seulement des lettres et espaces**

```python
import tkinter as tk
import re

def valider_nom(txt):
    return bool(re.fullmatch(r"[A-Za-z ]*", txt))

root = tk.Tk()
root.title("Nom avec lettres et espaces")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_nom), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Ce champ autorise les prénoms ou noms composés, comme `"Jean Pierre"`, `"Marie"`, etc.

---

### **Exercice 7 – Code postal canadien (format : A1A 1A1)**

```python
import tkinter as tk
import re

def valider_cp(txt):
    return bool(re.fullmatch(r"[A-Za-z]\d[A-Za-z] \d[A-Za-z]\d", txt))

root = tk.Tk()
root.title("Code Postal Canadien")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_cp), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Le code postal canadien suit un format strict : lettre-chiffre-lettre espace chiffre-lettre-chiffre.

---

### **Exercice 8 – Numéro de téléphone (514-123-4567)**

```python
import tkinter as tk
import re

def valider_tel(txt):
    return bool(re.fullmatch(r"514-\d{3}-\d{4}", txt))

root = tk.Tk()
root.title("Numéro téléphone 514-XXX-XXXX")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_tel), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Ce champ impose le format montréalais `514-...`, ce qui est utile dans un formulaire régionalisé.

---

### **Exercice 9 – Champ avec deux majuscules suivies de trois chiffres**

```python
import tkinter as tk
import re

def valider_code(txt):
    return bool(re.fullmatch(r"[A-Z]{2}\d{3}", txt))

root = tk.Tk()
root.title("Code de type AB123")

champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(root.register(valider_code), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Exemple accepté : `AB123`, `XY999`.
> Très utilisé pour les codes d'identifiants ou les plaques.

---

### **Exercice 10 – Mot de passe contenant au moins 1 majuscule et 1 chiffre (≥ 6 caractères)**

```python
import tkinter as tk
import re

def valider_mdp(txt):
    return bool(re.fullmatch(r"(?=.*[A-Z])(?=.*\d).{6,}", txt))

root = tk.Tk()
root.title("Mot de passe sécurisé")

champ = tk.Entry(root,
                 validate="key",
                 show="*",
                 validatecommand=(root.register(valider_mdp), "%P"))
champ.pack(padx=10, pady=10)

root.mainloop()
```

> Ce champ masque les caractères (`show="*"`) et impose un mot de passe **sécurisé**, avec au moins 1 majuscule, 1 chiffre, et 6 caractères minimum.

