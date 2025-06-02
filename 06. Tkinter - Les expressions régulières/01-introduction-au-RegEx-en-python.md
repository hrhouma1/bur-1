# **Introduction aux expressions régulières (RegEx) en Python**



# **1. Définition et objectif**

Une **expression régulière** est une notation symbolique utilisée pour spécifier des motifs (patterns) de texte. Ces motifs servent à rechercher, valider, extraire ou transformer des chaînes de caractères dans des documents, formulaires ou bases de données.

> En d’autres termes, une expression régulière est une manière formelle de poser la question :
> *“Est-ce que cette chaîne de caractères correspond exactement à ce que je cherche ?”*



# **2. Pourquoi utiliser des expressions régulières ?**

Elles permettent d’automatiser des tâches courantes :

* Vérifier qu’une adresse email est bien formée
* Repérer des codes postaux, des numéros ou des mots-clés
* Extraire des données précises dans un texte (ex. : prix, dates)
* Supprimer des caractères indésirables (ex. : ponctuation, symboles)



# **3. Utilisation minimale en Python**

```python
import re

texte = "Bienvenue à Université123"
motif = r"[A-Za-z]+[0-9]+"
resultat = re.search(motif, texte)

if resultat:
    print("Motif détecté :", resultat.group())
else:
    print("Aucune correspondance.")
```

> Ce code vérifie s’il existe un mot composé de lettres suivi de chiffres, comme "Université123".



# **4. Caractères littéraux**

Ce sont les lettres, chiffres ou symboles que l’on cherche tels quels.

```python
re.search("Paris", "Paris est une capitale")  # Match
re.search("Berlin", "Capitale : Berlin")      # Match
re.search("Tokyo", "Ville de Kyoto")          # Aucun match
```



# **5. Symboles spéciaux (métacaractères)**

Les expressions régulières utilisent des symboles pour exprimer des motifs complexes :

| Symbole | Description                           | Exemple                        |
| ------- | ------------------------------------- | ------------------------------ |
| `.`     | Tout caractère sauf saut de ligne     | `a.b` → "acb", "a1b"           |
| `^`     | Début de la chaîne                    | `^Nom` → "Nomade", "Nominal"   |
| `$`     | Fin de la chaîne                      | `fin$` → "La fin"              |
| `*`     | Zéro ou plusieurs                     | `ab*` → "a", "ab", "abb", etc. |
| `+`     | Une ou plusieurs                      | `ab+` → "ab", "abb"            |
| `?`     | Zéro ou un                            | `colou?r` → "color", "colour"  |
| `\d`    | Un chiffre (0-9)                      | `\d+` → "2024"                 |
| `\w`    | Lettre, chiffre ou "\_"               | `\w+` → "Variable\_1"          |
| `\s`    | Espace, tabulation, retour à la ligne | `\s+` → " "                    |



# **6. Quantificateurs – Préciser combien de fois**

| Motif    | Signification        |
| -------- | -------------------- |
| `x{3}`   | exactement trois x   |
| `x{2,5}` | entre deux et cinq x |
| `x{4,}`  | au moins quatre x    |

```python
re.search(r"\d{4}", "Année : 1999")   # Match
re.search(r"A{2,3}", "AA")            # Match
re.search(r"A{2,3}", "AAAA")          # Match partiel "AAA"
```



# **7. Groupes et alternatives**

* **Parenthèses `()`** pour grouper
* **Barre verticale `|`** pour exprimer "ou"

```python
re.search(r"(chien|chat)", "J’ai un chien")   # Match
re.search(r"(a|e|i|o|u)", "xyz")              # Aucun match
```


# **8. Fonctions de base en Python**

| Fonction       | Usage                                   |
| -------------- | --------------------------------------- |
| `re.search()`  | Cherche la **première occurrence**      |
| `re.findall()` | Retourne **toutes les correspondances** |
| `re.sub()`     | **Remplace** un motif par autre chose   |
| `re.split()`   | **Découpe** une chaîne selon un motif   |

### Exemple :

```python
texte = "Nom: Alice, Age: 22; Note: 19.5"
valeurs = re.split(r"[:,;] ?", texte)
print(valeurs)
```

> Résultat : `['Nom', 'Alice', 'Age', '22', 'Note', '19.5']`



# **9. Valider un email**

```python
email = "utilisateur@mail.fr"
motif = r"^[\w\.-]+@[\w-]+\.[a-z]{2,4}$"

if re.match(motif, email):
    print("Email valide.")
else:
    print("Email invalide.")
```

### Explication :

* `^[\w\.-]+` : commence par une série de lettres/chiffres/points
* `@` : obligatoire
* `[\w-]+` : nom de domaine
* `\.[a-z]{2,4}$` : extension `.fr`, `.com`, etc.



# **10. Vérification manuelle d’une adresse email**

Méthode sans expression régulière, utile pour comprendre les règles de base :

```python
def verif_email(email):
    if '@' not in email or '.' not in email.split('@')[-1]:
        return False
    return True
```




# **11. Nettoyage d’un nom d’utilisateur**

### Objectif : supprimer chiffres et caractères spéciaux

```python
nom = "Pauline_98!"
nettoye = re.sub(r"[^A-Za-z]", "", nom)
print(nettoye)  # Pauline
```

> On conserve uniquement les lettres. `[^...]` signifie "tout ce qui est inclut après ^".



# **12. Exercices d’entraînement**

### Exercice 1

Validez si la chaîne "Code-XYZ-2023" contient un mot suivi d’un tiret, puis trois majuscules, un autre tiret et quatre chiffres.

> Motif attendu : `\w+-[A-Z]{3}-\d{4}`
> Test : `"Code-XYZ-2023"` → Match



### Exercice 2

Extraire toutes les dates au format `JJ-MM-AAAA` dans un texte :

```python
texte = "Les examens auront lieu le 15-06-2024 et le 03-07-2025."
dates = re.findall(r"\d{2}-\d{2}-\d{4}", texte)
print(dates)
```

> Résultat : `['15-06-2024', '03-07-2025']`



# Exercice 3

Supprimer tous les caractères non alphabétiques d’un nom complet :

```python
nom = "M@r!e-L0u"
nettoye = re.sub(r"[^a-zA-Z]", "", nom)
print(nettoye)
```

> Résultat attendu : `"MreLou"`





















































#  Exercice 4 — Validation de mot de passe sécurisé

Valider un mot de passe qui respecte **toutes** les règles suivantes :

1. **Au moins 8 caractères**
2. **Au moins une majuscule**
3. **Au moins un chiffre**
4. **Au moins un symbole** parmi `@ # $ % ^ & + =`

> **Motif RegEx utilisé** :
> `^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&+=]).{8,}$`



## Exemple

> `"Professeur@2024"`
> ✔️ Contient une majuscule (`P`), un chiffre (`2024`), un symbole (`@`), et fait plus de 8 caractères → **valide**

1. Longueur ≥ 8 ? ✅ (15 caractères)
2. Contient une majuscule ? ✅ (`P`)
3. Contient un chiffre ? ✅ (`2`, `0`, `2`, `4`)
4. Contient un symbole spécial ? ✅ (`@`)

Donc : **mot de passe valide.**


##  Décomposition du motif

```regex
^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&+=]).{8,}$
```

### 1. `^`

> **Signifie** : début de la chaîne
> Cela garantit que la vérification commence **au tout début du mot de passe**.


> **Début de la chaîne** : garantit qu’on commence à lire dès le premier caractère.

### 2. `(?=.*[A-Z])`

> Vérifie la présence **d’au moins une majuscule**.
>
> * `.` : n’importe quel caractère
> * `*` : zéro ou plusieurs fois
> * `[A-Z]` : une lettre majuscule
> * `(?=...)` : vérifie la présence sans consommer les caractères (**lookahead**)

## Pour résumé 

> **C’est une assertion appelée "lookahead positif"**. Elle signifie :
> → “Il doit y avoir **au moins une lettre majuscule** quelque part dans la chaîne.”

* `.` : n’importe quel caractère (sauf saut de ligne)
* `*` : zéro ou plusieurs fois
* `[A-Z]` : une lettre majuscule
* Donc `.*[A-Z]` : on permet n'importe quoi **jusqu'à ce qu’on trouve** une majuscule.
* Le `(?=...)` ne consomme pas de caractères, il **vérifie seulement leur présence**.





### 3. `(?=.*\d)`

> Condition à vérifier :
> → “Il doit y avoir **au moins un chiffre** quelque part.”

* `\d` : un chiffre (équivalent à `[0-9]`)
* `.*\d` : on autorise tous les caractères jusqu’à ce qu’on trouve **au moins un chiffre**.

> Vérifie la présence **d’au moins un chiffre** (`\d` est équivalent à `[0-9]`)

### 4. `(?=.*[@#$%^&+=])`


> Condition imposée :
> → “Il doit y avoir **au moins un caractère spécial** parmi ceux-ci : `@ # $ % ^ & + =`.”

* On peut modifier cette partie selon les symboles autorisés dans le mot de passe.
* `.*[@#$%^&+=]` : tout caractère jusqu’à ce qu’on **trouve un de ces symboles**.

> Vérifie la présence **d’au moins un caractère spécial** parmi `@#$%^&+=`

### 5. `.{8,}`

> C’est la **dernière contrainte** :
> → “La chaîne doit contenir **au moins 8 caractères** (n’importe lesquels).”

* `.` : tout caractère
* `{8,}` : **au moins 8 fois**

> Vérifie que la chaîne fait **au moins 8 caractères**, n’importe lesquels


### 6. `$`

> **Fin de la chaîne** : rien ne doit venir après.
> Cela signifie qu’on valide le mot de passe **dans sa totalité**, sans qu’il dépasse ou soit tronqué.




##  Code Python pour tester

```python
import re

mot_de_passe = "Professeur@2024"
motif = r"^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&+=]).{8,}$"

if re.match(motif, mot_de_passe):
    print("Mot de passe valide ✅")
else:
    print("Mot de passe invalide ❌")
```



## Résumé des contraintes

| Critère                | Exemple valide |
| ---------------------- | -------------- |
| Longueur ≥ 8           | `"A1@xyzab"`   |
| Contient une majuscule | `"A1@xyzab"`   |
| Contient un chiffre    | `"A1@xyzab"`   |
| Contient un symbole    | `"A1@xyzab"`   |



## ✅ Mots de passe valides

> `"Professeur@2024"`
> `"Securite123$"`
> `"PassWord1@"`
> `"A1#b2C3@"`



## ❌ Mots de passe invalides

> `"motdepasse"`
> ❌ Aucun chiffre, majuscule ou symbole

> `"Motdepasse"`
> ❌ Majuscule présente, mais ni chiffre ni symbole

> `"Motdepasse1"`
> ❌ Majuscule + chiffre, mais pas de symbole

> `"motdepasse@1"`
> ❌ Chiffre + symbole, mais pas de majuscule

> `"M1@"`
> ❌ Trop court (3 caractères seulement)

> `"Mdp@202"`
> ❌ Longueur = 7 → trop court




# Résumé de l'exercice 4

Valider un mot de passe d’au moins 8 caractères, avec une majuscule, un chiffre et un symbole.

> Motif possible : `^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&+=]).{8,}$`
> Test : `"Professeur@2024"` → ✅ Valide
> Décomposé en :
>
> * `^` : début de la chaîne
> * `(?=.*[A-Z])` : contient au moins une lettre majuscule
> * `(?=.*\d)` : contient au moins un chiffre
> * `(?=.*[@#$%^&+=])` : contient au moins un caractère spécial parmi ceux listés
> * `.{8,}` : au moins 8 caractères au total
> * `$` : fin de la chaîne
>   👉 **L’ordre n’est pas important !**


### *Explication détaillée, pas à pas*

```regex
^(?=.*[A-Z])(?=.*\d)(?=.*[@#$%^&+=]).{8,}$
```

Ce motif est utilisé pour valider un mot de passe sécurisé. Il vérifie les conditions suivantes :

> `^`
> Début de la chaîne : on commence l’analyse dès le premier caractère.

> `(?=.*[A-Z])`
> Il faut au moins une lettre majuscule quelque part dans la chaîne.

> `(?=.*\d)`
> Il faut au moins un chiffre dans la chaîne.

> `(?=.*[@#$%^&+=])`
> Il faut au moins un symbole spécial parmi ceux listés : `@ # $ % ^ & + =`.

> `.{8,}`
> La chaîne doit comporter **au minimum 8 caractères**.

> `$`
> Fin de la chaîne : rien ne doit venir après.







| Élément              | Représentation dans le motif | Ce que ça impose                   |
| -------------------- | ---------------------------- | ---------------------------------- |
| Début de chaîne      | `^`                          | Commencer l’analyse dès le début   |
| Au moins 1 majuscule | `(?=.*[A-Z])`                | Doit contenir une majuscule        |
| Au moins 1 chiffre   | `(?=.*\d)`                   | Doit contenir un chiffre           |
| Au moins 1 symbole   | `(?=.*[@#$%^&+=])`           | Doit contenir un caractère spécial |
| Longueur ≥ 8         | `.{8,}`                      | Au moins 8 caractères              |
| Fin de chaîne        | `$`                          | Rien d’autre après                 |




# **13. Résumé final**

1. Une expression régulière définit un **motif textuel** à reconnaître.
2. Python fournit plusieurs fonctions pour **chercher**, **remplacer**, **valider** ou **découper** des chaînes avec ces motifs.
3. Les **métacaractères** permettent de généraliser les recherches (par exemple, tous les chiffres, toutes les majuscules, etc.).
4. Les **quantificateurs** donnent la possibilité de fixer ou d'encadrer le nombre d’occurrences.
5. Il est crucial de **tester les motifs progressivement**, en commençant par des cas simples.




## **14. Exemples**

1. Une expression régulière définit un **motif textuel** à reconnaître.

> Exemple : le motif `r"\d{4}"` correspond à toute séquence de **quatre chiffres** consécutifs, comme dans `"Année : 2025"`.

2. Python fournit plusieurs fonctions pour **chercher**, **remplacer**, **valider** ou **découper** des chaînes avec ces motifs.

> Exemple :
>
> ```python
> re.sub(r"\s+", "-", "Université de Montréal")  
> ```
>
> Résultat : `"Université-de-Montréal"` (remplace les espaces par des tirets).

3. Les **métacaractères** permettent de généraliser les recherches (par exemple, tous les chiffres, toutes les majuscules, etc.).

> Exemple : `\d` détecte un chiffre, `\w` détecte un caractère alphanumérique.
>
> ```python
> re.findall(r"\d", "Code: A123B")  
> ```
>
> Résultat : `['1', '2', '3']`.

4. Les **quantificateurs** donnent la possibilité de fixer ou d'encadrer le nombre d’occurrences.

> Exemple :
>
> ```python
> re.match(r"A{2,4}", "AAA")  
> ```
>
> Résultat : Match car la lettre "A" apparaît trois fois (entre 2 et 4 fois).

5. Il est crucial de **tester les motifs progressivement**, en commençant par des cas simples.

> Exemple : commencez par tester une recherche simple comme :
>
> ```python
> re.search("test", "examen test final")  
> ```
>
> Avant d’essayer un motif plus complexe comme `r"\bTest\d{3}\b"` pour repérer `"Test101"`.







# **15. Questions de compréhension (QCM + Réponses justifiées)**



### **Question 1**

Quel motif permet de détecter **exactement quatre chiffres consécutifs** dans un texte ?

**A.** `\w{4}`
**B.** `\d{4}`
**C.** `.{4}`
**D.** `\s{4}`

> **Réponse correcte : B**
> `\d` signifie "un chiffre" (entre 0 et 9). Le quantificateur `{4}` signifie "exactement quatre fois".
> Ainsi, `\d{4}` détecte des séquences comme `"2023"` ou `"1234"`.



### **Question 2**

Laquelle des fonctions suivantes permet de **remplacer** un motif par une autre chaîne dans un texte ?

**A.** `re.findall()`
**B.** `re.replace()`
**C.** `re.sub()`
**D.** `re.match()`

> **Réponse correcte : C**
> `re.sub()` est utilisée pour substituer un motif par une nouvelle chaîne.
> Exemple : `re.sub(r"chat", "chien", "Le chat dort")` retourne `"Le chien dort"`.



### **Question 3**

Quelle expression régulière permet de valider un mot composé uniquement de **lettres majuscules** ?

**A.** `[A-Z]+`
**B.** `[a-z]+`
**C.** `\d+`
**D.** `\w+`

> **Réponse correcte : A**
> `[A-Z]` cible les lettres majuscules de A à Z. Le `+` indique "au moins une fois".



### **Question 4**

Que signifie le motif `^Bonjour` dans une expression régulière ?

**A.** Le texte doit se terminer par "Bonjour"
**B.** Le texte doit contenir "Bonjour"
**C.** Le texte doit commencer par "Bonjour"
**D.** Le mot "Bonjour" est facultatif

> **Réponse correcte : C**
> Le symbole `^` désigne le début de la chaîne. Donc `^Bonjour` signifie "la chaîne commence par Bonjour".



### **Question 5**

Expliquez pourquoi l'expression suivante **ne détectera pas** la chaîne `"info@site.org"` :

```python
re.match(r"\w+@\w+\.\w{2,3}", "info@site.org")
```

> **Réponse attendue :**
> Le motif est correct, mais la fonction `re.match()` exige que le **motif commence dès le début de la chaîne**. Ici, cela fonctionne car `"info@site.org"` commence bien par un email. Si on avait utilisé une chaîne comme `"email : info@site.org"`, le match aurait échoué.
> Dans ce cas, `re.search()` serait préférable.



### **Question 6**

Qu’est-ce que `[^\d]` signifie en expression régulière ?

**A.** Tous les chiffres
**B.** Tout sauf des chiffres
**C.** Les lettres minuscules
**D.** Les symboles uniquement

> **Réponse correcte : B**
> Le `^` placé à l’intérieur des crochets signifie **négation**. Donc `[^\d]` signifie : "tout caractère **sauf** un chiffre".








# **16. Exercices pratiques de rédaction de motifs**


### **Exercice 1 – Numéro de dossier**

**Consigne :**
Écrivez une expression régulière qui valide un identifiant de dossier respectant le format suivant :
trois lettres majuscules suivies de deux chiffres (exemples valides : `"ABC12"`, `"XYZ99"`).

> **Correction attendue :**
> `^[A-Z]{3}\d{2}$`
> Cette expression signifie :
>
> * `^[A-Z]{3}` → début de chaîne avec trois lettres majuscules
> * `\d{2}` → suivi de deux chiffres
> * `$` → fin de chaîne



### **Exercice 2 – Nom d’utilisateur simplifié**

**Consigne :**
Validez un nom d’utilisateur composé uniquement de lettres (majuscules ou minuscules), sans chiffres ni caractères spéciaux. Minimum : 4 caractères.

> **Correction attendue :**
> `^[A-Za-z]{4,}$`
> Explication :
>
> * `[A-Za-z]` autorise uniquement les lettres
> * `{4,}` exige au moins 4 lettres
> * `^` et `$` pour encadrer la chaîne complète


### **Exercice 3 – Numéro de téléphone local**

**Consigne :**
Validez un numéro de téléphone au format `XXX-XXX-XXXX` où `X` est un chiffre (exemple : `514-123-4567`).

> **Correction attendue :**
> `^\d{3}-\d{3}-\d{4}$`
> Explication :
>
> * `\d{3}` pour les trois premiers blocs
> * `-` pour les séparateurs
> * Encadré par `^` et `$` pour valider la totalité



### **Exercice 4 – Code postal canadien**

**Consigne :**
Créez une expression régulière pour valider un code postal canadien au format `A1A 1A1` (lettre, chiffre, lettre, espace, chiffre, lettre, chiffre).

> **Correction attendue :**
> `^[A-Za-z]\d[A-Za-z] \d[A-Za-z]\d$`
> Chaque composant est précisément défini selon le format canadien.



### **Exercice 5 – Mot de passe modéré**

**Consigne :**
Validez un mot de passe contenant :

* au moins 6 caractères
* au moins une majuscule
* au moins un chiffre

> **Correction attendue :**
> `^(?=.*[A-Z])(?=.*\d).{6,}$`
> Explication :
>
> * `(?=.*[A-Z])` → au moins une lettre majuscule
> * `(?=.*\d)` → au moins un chiffre
> * `.{6,}` → au moins 6 caractères
> * `^` et `$` pour couvrir toute la chaîne




