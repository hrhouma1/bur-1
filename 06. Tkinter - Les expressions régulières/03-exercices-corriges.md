# **Exercices – Applications directes**

### **Consigne :** 

- Pour chaque énoncé, écrivez l'expression régulière (RegEx) la plus appropriée.
- N’utilisez pas de fonctions Python pour le moment : concentrez-vous uniquement sur l'écriture du motif.



### **Exercice 1**

Trouver un mot de **quatre lettres exactes** (lettres minuscules uniquement).

### **Exercice 2**

Valider un mot qui commence par une **majuscule**, suivi de lettres minuscules (ex. : "Alice").

### **Exercice 3**

Détecter un **nombre à trois chiffres** (ex. : "123", "456").

### **Exercice 4**

Trouver une séquence de **deux mots séparés par un espace** (ex. : "Bonjour Monde").

### **Exercice 5**

Valider un nombre entier **positif ou négatif** (ex. : "-42", "17").

### **Exercice 6**

Repérer une **adresse IP** au format `xxx.xxx.xxx.xxx` où chaque `x` est un ou plusieurs chiffres.

### **Exercice 7**

Détecter toutes les **lettres majuscules** dans une chaîne.

### **Exercice 8**

Valider une **date** au format `JJ/MM/AAAA`.

### **Exercice 9**

Trouver un mot contenant **exactement deux chiffres à la fin** (ex. : "Ref45", "Item09").

### **Exercice 10**

Supprimer tous les **caractères non alphabétiques** d’un mot.



## **Corrections**

> **Exercice 1 :**
> `^[a-z]{4}$`
> Ce motif indique : début (`^`), 4 lettres minuscules, fin (`$`).

> **Exercice 2 :**
> `^[A-Z][a-z]+$`
> Une majuscule suivie d’au moins une minuscule.

> **Exercice 3 :**
> `^\d{3}$`
> Trois chiffres, encadrés par le début et la fin de chaîne.

> **Exercice 4 :**
> `^\w+\s\w+$`
> Un mot (`\w+`), un espace (`\s`), un autre mot (`\w+`).

> **Exercice 5 :**
> `^-?\d+$`
> Le tiret `-` est optionnel (`?`), suivi d’un ou plusieurs chiffres.

> **Exercice 6 :**
> `^\d{1,3}(\.\d{1,3}){3}$`
> Une suite de 4 blocs numériques séparés par des points.

> **Exercice 7 :**
> `[A-Z]`
> Ce motif détecte chaque lettre majuscule dans une chaîne.

> **Exercice 8 :**
> `^\d{2}/\d{2}/\d{4}$`
> Deux chiffres, une barre, deux chiffres, une barre, quatre chiffres.

> **Exercice 9 :**
> `^\w+\d{2}$`
> Mot alphanumérique se terminant par deux chiffres exacts.

> **Exercice 10 :**
> `[^A-Za-z]`
> Ce motif détecte tout caractère **non alphabétique**. Utilisé avec `re.sub()`, il permet de supprimer ces caractères.

