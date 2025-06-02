# **Interface Tkinter pas à pas**

## Objectifs

* Créer un environnement virtuel Python
* Lancer une première interface Tkinter
* Ajouter progressivement des composants (Label, Entry, Button)
* Comprendre la logique événementielle de Tkinter


## **Étape 0 – Préparation de l’environnement**

1. Créer un dossier nommé `tp_tkinter_base`.
2. Depuis le terminal, créer un environnement virtuel Python.

```bash
......................................................
```

## Réponse 

```bash
mkdir tp_tkinter_base
cd tp_tkinter_base
python -m venv venv
```







3. Activer l’environnement virtuel.

```bash
......................................................
```

## Réponse 

```bash
venv\Scripts\activate      # Windows
source venv/bin/activate   # macOS / Linux
```

---

4. Installer Tkinter.

```bash
......................................................
```

## Réponse 

### Pas besoin d'installer thinkter , il est déja intégré sinon sur linux (optionnel) ==>

```bash
sudo apt install python3-tk
```

---

5. Créer un fichier `main.py` dans ce dossier.

---

## **Étape 1 – Créer une fenêtre vide**

> Écris ici le code minimal permettant d’afficher une fenêtre vide intitulée "Ma première fenêtre" de taille 300x200.

```python
......................................................
......................................................
......................................................
......................................................
```

## Réponse Étape 1

```python
import tkinter as tk
root = tk.Tk()
root.title("Ma première fenêtre")
root.geometry("300x200")
root.mainloop()
```










## **Étape 2 – Ajouter un Label**

> Modifie le fichier `main.py` pour qu’un `Label` contenant le texte `"Bonjour, Tkinter !"` apparaisse au centre de la fenêtre.

```python
......................................................
......................................................
......................................................
```

## Réponse Étape 2

```python
label = tk.Label(root, text="Bonjour, Tkinter !")
label.pack()
```

---

## **Étape 3 – Ajouter un champ de texte**

> Ajoute un champ de texte `Entry` sous le `Label`.

```python
......................................................
......................................................
```

## Réponse Étape 3

```python
champ = tk.Entry(root)
champ.pack()
```







## **Étape 4 – Ajouter un bouton**

> Ajoute un bouton intitulé "Valider" qui, lorsqu’on clique dessus, récupère le contenu du champ de texte et l’affiche dans le `Label`.

1. Écris la fonction appelée lors du clic :

```python
def afficher_message():
    ......................................................
    ......................................................
```

## Réponse Étape 4.1

```python
def afficher_message():
    texte = champ.get()
    label.config(text=texte)
```

---

2. Puis ajoute le bouton :

```python
......................................................
```

## Réponse Étape 4.2

```python
bouton = tk.Button(root, text="Valider", command=afficher_message)
bouton.pack()
```








## **Étape 5 – Vider le champ de texte après validation**

> Modifie la fonction `afficher_message()` pour qu’elle vide le champ `Entry` après l’affichage.

```python
def afficher_message():
    ......................................................
    ......................................................
    ......................................................
```

## Réponse Étape 5

```python
def afficher_message():
    texte = champ.get()
    label.config(text=texte)
    champ.delete(0, tk.END)
```










<br/>
<br/>







## **Étape 6 – Restreindre la saisie avec une condition simple**


Tu vas maintenant apprendre à **contrôler ce que l’utilisateur peut taper** dans le champ de texte.
Tu vas faire en sorte que :

* L’utilisateur **ne puisse entrer que des lettres**
* Le texte saisi ne dépasse pas **10 caractères**
* Toute autre saisie sera automatiquement **rejetée** (non affichée dans le champ)


### 6.1 – Ajouter une fonction de validation

> Crée une fonction appelée `valider_texte` qui recevra un texte (celui que l’utilisateur essaie d’écrire).
> Cette fonction devra retourner `True` si le texte est correct, et `False` s’il contient autre chose que des lettres, ou s’il est trop long.

```python
def valider_texte(texte):
    if ......................................................:
        return True
    else:
        return False
```

## Réponse Étape 6.1

```python
def valider_texte(texte):
    if texte.isalpha() and len(texte) <= 10:
        return True
    else:
        return False
```

---

### 6.2 – Enregistrer cette fonction pour Tkinter

```python
valide = root.register(......................................................)
```

## Réponse Étape 6.2

```python
valide = root.register(valider_texte)
```

---

### 6.3 – Appliquer la validation au champ `Entry`

```python
champ = tk.Entry(root,
                 validate="key",
                 validatecommand=(valide, "%P"))
champ.pack()
```

## Réponse Étape 6.3

```python
champ = tk.Entry(root, validate="key", validatecommand=(valide, "%P"))
champ.pack()
```






### Ce que tu dois observer :

* Si tu tapes un **mot uniquement avec des lettres**, il s’affiche.
* Si tu tapes un chiffre ou un symbole, **rien ne se passe** (la frappe est bloquée).
* Si tu dépasses 10 lettres, **la frappe est aussi bloquée**.








<br/>
<br/>





## **Étape 7 – Afficher un message et changer la couleur du champ**



Tu vas maintenant **améliorer l'expérience utilisateur** en ajoutant :

* Un **message** qui indique si la saisie est correcte ou non
* Un **changement de couleur du champ** selon le résultat
















## **Étape 7 – Afficher un message et changer la couleur du champ**

### 7.1 – Créer une variable pour le message

```python
message = ......................................................
```

## Réponse Étape 7.1

```python
message = tk.StringVar()
```

---

### 7.2 – Créer un `Label` qui utilise cette variable

```python
label_message = tk.Label(root, textvariable=message)
label_message.pack()
```

## Réponse Étape 7.2

```python
label_message = tk.Label(root, textvariable=message)
label_message.pack()
```

---

### 7.3 – Créer une fonction appelée **après chaque frappe**

```python
def verifier_texte(event):
    texte = champ.get()
    if ......................................................:
        message.set("Saisie correcte.")
        champ.config(bg="white")
    else:
        message.set("Erreur : lettres uniquement, 10 caractères max.")
        champ.config(bg="misty rose")
```

## Réponse Étape 7.3

```python
def verifier_texte(event):
    texte = champ.get()
    if texte.isalpha() and len(texte) <= 10:
        message.set("Saisie correcte.")
        champ.config(bg="white")
    else:
        message.set("Erreur : lettres uniquement, 10 caractères max.")
        champ.config(bg="misty rose")
```

---

### 7.4 – Lier cette fonction au champ

```python
champ.bind("<KeyRelease>", ................................)
```

## Réponse Étape 7.4

```python
champ.bind("<KeyRelease>", verifier_texte)
```





### Ce que tu dois observer :

* Le **champ reste vide** si la saisie est invalide (bloqué par `validate`)
* Mais le **message** en dessous s’actualise à chaque frappe
* Si le texte est valide → fond blanc, message "Saisie correcte."
* Si le texte est invalide → fond rosé, message d’erreur









<br/>
<br/>





# **Étape 8 – Ajouter un bouton "Effacer"**



Tu vas maintenant ajouter un **deuxième bouton**, appelé **"Effacer"**, qui :

* Vide le contenu du champ `Entry`
* Efface le message affiché
* Réinitialise la couleur du champ (en blanc)











## **Étape 8 – Ajouter un bouton "Effacer"**

### 8.1 – Créer une fonction `effacer_champ`

```python
def effacer_champ():
    ......................................................
    ......................................................
    ......................................................
```

## Réponse Étape 8.1

```python
def effacer_champ():
    champ.delete(0, tk.END)
    message.set("")
    champ.config(bg="white")
```

---

### 8.2 – Créer le bouton "Effacer"

```python
bouton_effacer = tk.Button(root, text="Effacer", command=................................)
bouton_effacer.pack()
```

## Réponse Étape 8.2

```python
bouton_effacer = tk.Button(root, text="Effacer", command=effacer_champ)
bouton_effacer.pack()
```










### Ce que tu dois observer :

* Après avoir tapé du texte (correct ou incorrect), tu peux cliquer sur **Effacer**
* Le champ de saisie devient vide
* Le fond du champ redevient blanc
* Le message en dessous disparaît



### Vérifications à faire :

* Que se passe-t-il si tu tapes une erreur, puis cliques sur Effacer ?
* Que se passe-t-il si tu tapes un texte valide, puis cliques sur Effacer ?
* Le champ et le message sont-ils bien réinitialisés ?






<br/>
<br/>
<br/>


# Annexe -  **main.py — Version commentée**

```python
# Importation du module Tkinter
import tkinter as tk

# Fonction de validation : accepte uniquement des lettres, max 10 caractères
def valider_texte(texte):
    return texte.isalpha() and len(texte) <= 10

# Fonction appelée lors du clic sur le bouton "Valider"
# Elle récupère le texte, l'affiche dans le Label, puis vide le champ
def afficher_message():
    texte = champ.get()
    label.config(text=texte)
    champ.delete(0, tk.END)

# Fonction appelée après chaque frappe au clavier
# Elle affiche un message et change la couleur du champ selon la validité
def verifier_texte(event):
    texte = champ.get()
    if texte.isalpha() and len(texte) <= 10:
        message.set("Saisie correcte.")
        champ.config(bg="white")
    else:
        message.set("Erreur : lettres uniquement, 10 caractères max.")
        champ.config(bg="misty rose")

# Fonction appelée par le bouton "Effacer"
# Elle vide le champ, supprime le message, et remet la couleur par défaut
def effacer_champ():
    champ.delete(0, tk.END)
    message.set("")
    champ.config(bg="white")

# Création de la fenêtre principale
root = tk.Tk()
root.title("Ma première fenêtre")
root.geometry("300x200")

# Label initial affiché en haut
label = tk.Label(root, text="Bonjour, Tkinter !")
label.pack()

# Enregistrement de la fonction de validation
valide = root.register(valider_texte)

# Champ de saisie avec validation à chaque frappe
champ = tk.Entry(root, validate="key", validatecommand=(valide, "%P"))
champ.pack()

# Liaison : après chaque touche, on appelle la fonction de vérification
champ.bind("<KeyRelease>", verifier_texte)

# Message dynamique sous le champ
message = tk.StringVar()
label_message = tk.Label(root, textvariable=message)
label_message.pack()

# Bouton "Valider"
bouton = tk.Button(root, text="Valider", command=afficher_message)
bouton.pack()

# Bouton "Effacer"
bouton_effacer = tk.Button(root, text="Effacer", command=effacer_champ)
bouton_effacer.pack()

# Lancement de la boucle principale de l'interface
root.mainloop()
```





