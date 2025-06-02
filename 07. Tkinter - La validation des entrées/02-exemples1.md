## **Exercices simples avec corrections**

---

### **Exercice 1**

Créer un champ Tkinter qui n’autorise que des chiffres entre 0 et 999.

> **Correction attendue :**
>
> ```python
> def valider(texte):
>     return texte.isdigit() and 0 <= int(texte) <= 999 if texte else True
> ```
>
> On autorise les chiffres uniquement (`isdigit`) et on convertit en entier pour vérifier la plage. La chaîne vide est autorisée pour permettre d’effacer.

---

### **Exercice 2**

Créer un champ qui n’accepte que des lettres majuscules.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"[A-Z]*", texte))
> ```
>
> Le motif `[A-Z]*` autorise une chaîne de **zéro ou plusieurs lettres majuscules**.

---

### **Exercice 3**

Créer un champ qui n’accepte qu’un mot de passe de 6 caractères minimum avec au moins une lettre majuscule.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"(?=.*[A-Z]).{6,}", texte))
> ```
>
> `(?=.*[A-Z])` est une **assertion positive** : elle garantit qu’au moins une lettre majuscule est présente.

---

### **Exercice 4**

Créer un champ qui valide les noms de fichiers se terminant par `.py`.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"\w+\.py", texte))
> ```
>
> `\w+` correspond à un mot, suivi d’un point (`\.`), puis de `py`.

---

### **Exercice 5**

Créer un champ qui accepte une date au format `JJ/MM/AAAA`.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"\d{2}/\d{2}/\d{4}", texte))
> ```
>
> `\d{2}` pour jour et mois, `\d{4}` pour l’année. Les `/` sont des séparateurs fixes.

---

### **Exercice 6**

Créer un champ qui accepte un identifiant à 8 chiffres exactement.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"\d{8}", texte))
> ```
>
> Ce motif exige **exactement 8 chiffres consécutifs**.

---

### **Exercice 7**

Créer un champ qui accepte uniquement un prénom qui commence par une majuscule et contient seulement des lettres.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"[A-Z][a-z]+", texte))
> ```
>
> Ce motif impose : une majuscule suivie d’au moins une lettre minuscule.

---

### **Exercice 8**

Créer un champ qui accepte uniquement des nombres **positifs ou négatifs** (ex. : `12`, `-5`).

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"-?\d+", texte))
> ```
>
> Le `-?` signifie que le signe `-` est optionnel. Ensuite, `\d+` valide une ou plusieurs chiffres.

---

### **Exercice 9**

Créer un champ qui valide une chaîne contenant uniquement des **lettres et des espaces**.

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"[A-Za-z ]*", texte))
> ```
>
> Ce motif autorise les lettres majuscules, minuscules et les espaces. Il accepte aussi la chaîne vide.

---

### **Exercice 10**

Créer un champ qui valide une adresse email basique (ex. : `nom@exemple.com`).

> **Correction attendue :**
>
> ```python
> import re
> def valider(texte):
>     return bool(re.fullmatch(r"[\w\.-]+@[\w-]+\.[a-z]{2,4}", texte))
> ```
>
> Ce motif autorise une forme simple d’email : partie avant l’arobase, domaine, extension (ex. : `.com`, `.org`, etc.).

