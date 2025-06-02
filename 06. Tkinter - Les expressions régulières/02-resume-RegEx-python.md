# Résumé - Les expressions régulières (RegEx) en Python

## **Objectif**

 Chaque concept est expliqué, illustré, puis mis en pratique dans des scénarios concrets.



## **1. Qu’est-ce qu’une expression régulière ?**

Une **expression régulière** est une chaîne de caractères représentant un **motif** que l’on souhaite détecter, valider ou remplacer dans un texte.

### Exemple

```python
import re

texte = "Hello World!"
motif = r"Hello.*World!"
x = re.search(motif, texte)

if x is not None:
    print("Motif trouvé :", x.group())
else:
    print("Aucun match.")
```



## **2. Exemple d’expression régulière simple**

Vérifions si une phrase correspond exactement à :

> "Le chat est très mignon."

```python
re.search(r"^Le (chat|chien) est (très )?\w+\.$", "Le chat est très mignon.")
```

Cette expression :

* commence par "Le",
* accepte "chat" ou "chien",
* accepte un "très" optionnel,
* accepte un mot (ex : mignon),
* se termine par un point.



## **3. Caractères littéraux**

### Exemples concrets

```python
re.search("hello", "hello world")  # Match
re.search("dog", "the dog sleeps") # Match
re.search("a", "alphabet")         # Match
```

Ces recherches utilisent uniquement des lettres exactes, sans métacaractères.



## **4. Métacaractères et symboles spéciaux**

| Symbole | Signification                              |            |
| ------- | ------------------------------------------ | ---------- |
| `.`     | Un caractère quelconque sauf saut de ligne |            |
| `^`     | Début de ligne                             |            |
| `$`     | Fin de ligne                               |            |
| `*`     | 0 ou plusieurs                             |            |
| `+`     | 1 ou plusieurs                             |            |
| `?`     | 0 ou 1                                     |            |
| `\d`    | Chiffre                                    |            |
| `\w`    | Lettre ou chiffre                          |            |
| `\s`    | Espace                                     |            |
| \`      | \`                                         | OU logique |
| `()`    | Groupe                                     |            |

**Exemple mixte :**

```python
re.search(r"^(\d+|hello)$", "1234")   # Match
re.search(r"^(\d+|hello)$", "hello")  # Match
```



## **5. Quantificateurs : nombre d’occurrences**

```python
re.search(r"a{3}", "aaabc")     # Match "aaa"
re.search(r"a{1,3}", "aab")     # Match "aa"
re.search(r"Rege{10}x", "Regeeeeeeeeeex")  # Match si 10 e
```



## **6. Modificateurs (Flags)**

Utilisés pour modifier le comportement global :

* `(?i)` : insensible à la casse
* `(?m)` : mode multiligne
* `(?s)` : inclut les sauts de ligne

```python
re.search(r"(?i)bonjour monde", "BONJOUR MONDE")  # Match
```


## **7. Rechercher avec Python – Cas pratiques**

### Exemple 1 : un seul match

```python
txt = "Bonjour Dupont5118 et Salem2250"
m = re.search(r"[A-Za-z]+\d{1,3}", txt)

if m:
    print("Texte trouvé :", m.group())
    print("Position :", m.span())
```

### Exemple 2 : tous les résultats

```python
matches = re.findall(r"[A-Za-z]+\d{1,3}", txt)

for i, m in enumerate(matches):
    print(f"Match #{i+1} :", m)
```



## **8. Nettoyer une chaîne avec `re.sub`**

### Exemple : retirer tous les caractères non alphabétiques

```python
nom_utilisateur = "Josephine@90"
nettoyé = re.sub(r"\W|\d", "", nom_utilisateur)
print("Résultat :", nettoyé)  # Affiche : Josephine
```

---

## **9. Découper une chaîne (`re.split`)**

```python
txt = "120,10 3,43;2:10 100"
data = re.split(r",|;|:| ", txt)
print("Données séparées :", data)
```


## **10. Valider une adresse email (version simple)**

```python
email = "test@gmail.com"
if re.match(r"^\w+@\w+\.\w{2,3}$", email):
    print("Email valide")
else:
    print("Email invalide")
```



## **11. Validation manuelle sans RegEx (approche pédagogique)**

### But : Comprendre la logique interne d’un validateur

```python
accepted_chars = list('abcdefghijklmnopqrstuvwxyz@!.%0123456789')

def verif_email(email):
    for c in email:
        if c not in accepted_chars:
            print(f"Caractère {c} non accepté")
            return False
    if '@' not in email:
        print("Il manque '@'")
        return False
    prefix, domain = email.split('@')
    if '.' not in domain:
        print("Il manque '.' dans le domaine")
        return False
    return True
```



## **12. Valider un mot de passe manuellement**

```python
def verif_password(password):
    for c in password:
        if c not in accepted_chars:
            print(f"Caractère {c} non accepté")
            return False
    if len(password) < 6:
        print("Mot de passe trop court")
        return False
    if password.lower() == password:
        print("Il faut au moins une majuscule")
        return False
    return True
```


## **13. Exemples supplémentaires (Snippets avancés)**

### Expression générale : accepter uniquement certains caractères

```python
pattern = r"^[A-Za-z0-9!@#\\$&*~^\"':.><=\+\-_\?\|%]*$"
```



## **14. Résumé des notions essentielles**

* Une expression régulière est une **règle de recherche**
* On peut chercher, extraire, valider ou remplacer du texte
* On combine **métacaractères** et **quantificateurs**
* Python fournit les fonctions : `search`, `findall`, `match`, `sub`, `split`
* Il est crucial de tester vos motifs dans différents cas concrets

