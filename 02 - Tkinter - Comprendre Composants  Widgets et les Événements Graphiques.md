
# Comprendre les composants  les Widgets et les Événements Graphiques avec Tkinter




#  Table des matières tkinter

* [1. Introduction et Rappel à Tkinter](#1-Rappel-Tkinter)
* [2. L’objet Tk](#2-lobjet-tk)
* [3. Redimensionnement et Titre de la Fenêtre](#3-redimensionnement-et-titre-de-la-fenêtre)
* [4. Exemple complet : Affichage d’une étiquette et d’un bouton](#4-exemple-complet--affichage-dune-étiquette-et-dun-bouton)
* [5. Composants Tkinter : Définition et Ajout](#5-composants-tkinter--définition-et-ajout)
* [6. Paramètres communs des composants](#6-paramètres-communs-des-composants)
* [7. Composant Label (Étiquette)](#7-composant-label-Étiquette)
* [8. Composant Button (Bouton)](#8-composant-button-bouton)
* [9. Composant Entry (Saisie utilisateur)](#9-composant-entry-saisie-utilisateur)
* [10. Composant Radiobutton (Bouton radio)](#10-composant-radiobutton-bouton-radio)
* [11. Composant Checkbutton (Case à cocher)](#11-composant-checkbutton-case-à-cocher)
* [12. Composant Listbox (Liste déroulante)](#12-composant-listbox-liste-déroulante)
* [13. Composant OptionMenu (Menu déroulant)](#13-composant-optionmenu-menu-déroulant)
* [14. Variables Tkinter](#14-variables-tkinter)
* [15. Changer dynamiquement une variable Tkinter](#15-changer-dynamiquement-une-variable-tkinter)
* [16. Évènements de Clavier et de Souris](#16-évènements-de-clavier-et-de-souris)
* [17. Conclusion](#17-conclusion)
* [⚙ Préparation de l’environnement sous Windows](#18-conclusion)




<br/>

# 1. Rappel Tkinter

**Tkinter** est une bibliothèque graphique intégrée à Python, permettant de créer des interfaces utilisateurs (GUI).


[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 2. L’objet Tk

Pour démarrer avec Tkinter, il faut créer une fenêtre principale, appelée **objet racine** (`root`).

```python
import tkinter as tk

# Création de l'élément racine
root = tk.Tk()

# Affichage de la fenêtre
root.mainloop()

print("Ce code sera exécuté à la fermeture de la fenêtre Tkinter")
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 3. Redimensionnement et Titre de la Fenêtre

### a) Définir une taille pour la fenêtre
```python
import tkinter as tk
root = tk.Tk()
root.geometry("200x100")
root.mainloop()
```

### b) Définir un titre pour la fenêtre
```python
import tkinter as tk
root = tk.Tk()
root.title("Mon premier programme")
root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 4. Exemple complet : Affichage d’une étiquette et d’un bouton

```python
import tkinter as tk

# Création de la fenêtre racine
root = tk.Tk()
root.geometry("500x500")
root.title('Fenêtre')

# Création d'une étiquette
texte1 = tk.Label(root, text='Bonjour tout le monde !', fg='red')
texte1.pack()

# Création d'un bouton
bouton1 = tk.Button(root, text='Quitter', command=root.destroy)
bouton1.pack()

# Affichage de la fenêtre
root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 5. Composants Tkinter : Définition et Ajout

Un **composant** (ou **widget**) est un élément d’interface, comme un bouton, une étiquette, une zone de texte.

Exemple :
```python
import tkinter as tk

root = tk.Tk()

# Bouton
btn = tk.Button(root, text="Récupérer sélection", bg="#2E75FF", fg="white")
btn.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 6. Paramètres communs des composants

- `text` : texte affiché
- `bg` : couleur de fond
- `fg` : couleur du texte
- `font` : police et taille du texte

Exemple :
```python
import tkinter as tk

root = tk.Tk()
btn = tk.Button(root, text="Récupérer sélection", bg="#2E75FF", fg="white")
btn.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 7. Composant Label (Étiquette)

```python
import tkinter as tk

root = tk.Tk()

texte1 = tk.Label(root, text="Hello World!", fg="red", bg="black", font=("Arial", 16))
texte1.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 8. Composant Button (Bouton)

### a) Bouton avec fonction de rappel
```python
import tkinter as tk

root = tk.Tk()

def clic():
    print("Tu m'as cliqué!")

btn = tk.Button(root, text="Clic-moi!", command=clic, bg="#2E75FF", fg="white")
btn.pack()

root.mainloop()
```

### b) Bouton pour fermer la fenêtre
```python
import tkinter as tk

root = tk.Tk()

btn_quit = tk.Button(root, text="Quitter", command=root.destroy, bg="red", fg="white")
btn_quit.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 9. Composant Entry (Saisie utilisateur)

```python
import tkinter as tk

root = tk.Tk()
root.title("Entry")
root.geometry("200x40")

saisie = tk.Entry(root, bg="gray", fg="white")
saisie.pack()

root.mainloop()
```

### Entry avec affichage de la saisie :

```python
import tkinter as tk

root = tk.Tk()
root.title("Entry")
root.geometry("200x40")

def afficher_saisie():
    print("\n\nVous avez entré :", saisie.get(), '\n\n')

label = tk.Label(root, text="Entrez votre nom :")
label.pack()

saisie = tk.Entry(root, bg="gray", fg="white")
saisie.pack()

btn = tk.Button(root, text="Valider", command=afficher_saisie, bg="#2E75FF", fg="white")
btn.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>




# 10. Composant Radiobutton (Bouton radio)

```python
import tkinter as tk

root = tk.Tk()
root.title("Radio")
root.geometry("200x100")

var = tk.IntVar()

radio_btn_1 = tk.Radiobutton(root, variable=var, text="Choix 1", value=1)
radio_btn_2 = tk.Radiobutton(root, variable=var, text="Choix 2", value=2)
radio_btn_3 = tk.Radiobutton(root, variable=var, text="Choix 3", value=3)

radio_btn_1.pack()
radio_btn_2.pack()
radio_btn_3.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 11. Composant Checkbutton (Case à cocher)

```python
import tkinter as tk

root = tk.Tk()
root.title("Checkbutton")
root.geometry("200x100")

check_btn_1 = tk.Checkbutton(root, text="Choix 1", onvalue=1, offvalue=0)
check_btn_2 = tk.Checkbutton(root, text="Choix 2", onvalue=1, offvalue=0)
check_btn_3 = tk.Checkbutton(root, text="Choix 3", onvalue=1, offvalue=0)

check_btn_1.pack()
check_btn_2.pack()
check_btn_3.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 12. Composant Listbox (Liste déroulante)

```python
import tkinter as tk

root = tk.Tk()
root.title("Listbox")
root.geometry("200x100")

listbox = tk.Listbox(root, selectmode="multiple")
listbox.insert(1, "JavaScript")
listbox.insert(2, "Python")
listbox.insert(3, "Java")
listbox.insert(4, "Rust")

listbox.pack()

root.mainloop()
```

Avec sélection multiple :
```python
import tkinter as tk

root = tk.Tk()
root.title("Listbox")
root.geometry("200x100")

def on_select():
    print(listbox.curselection())
    for i in listbox.curselection():
        print(listbox.get(i))

listbox = tk.Listbox(root, selectmode="multiple")
listbox.insert(1, "JavaScript")
listbox.insert(2, "Python")
listbox.insert(3, "Java")
listbox.insert(4, "Rust")
listbox.pack()

btn = tk.Button(root, text="Récupérer sélection", command=on_select, bg="#2E75FF", fg="white")
btn.pack()

root.mainloop()
```

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 13. Composant OptionMenu (Menu déroulant)

```python
import tkinter as tk

root = tk.Tk()
root.title("Menu")
root.geometry("200x100")

couleurs = ["Rouge", "Vert", "Bleu"]
couleur_selectionne = tk.StringVar(value="Sélectionnez une couleur")

menu_couleur = tk.OptionMenu(root, couleur_selectionne, *couleurs)
menu_couleur.pack()

root.mainloop()
```

Avec callback sur le choix :
```python
import tkinter as tk

root = tk.Tk()
root.title("Menu")
root.geometry("200x100")

def menu_change(couleur):
    print("Couleur sélectionnée:", couleur)

couleurs = ["Rouge", "Vert", "Bleu"]
couleur_selectionne = tk.StringVar(value="Sélectionnez une couleur")

menu_couleur = tk.OptionMenu(root, couleur_selectionne, command=menu_change, *couleurs)
menu_couleur.pack()

root.mainloop()
```


# Explications

#### 13.1. Importation de la bibliothèque

```python
import tkinter as tk
```

Cette ligne importe la bibliothèque `tkinter` qui permet de créer des interfaces graphiques en Python.  
Le mot-clé `as tk` signifie que l'on va utiliser le préfixe `tk.` pour accéder aux éléments de la bibliothèque, ce qui rend le code plus lisible.



#### 13.2. Création de la fenêtre principale

```python
root = tk.Tk()
```

Cette ligne crée la fenêtre principale de l'application graphique.  
Cette fenêtre est souvent appelée `root`, et tous les éléments graphiques (boutons, menus, champs de texte) y seront attachés.


#### 13.3. Configuration de la fenêtre

```python
root.title("Menu")
```

Définit le titre de la fenêtre qui s’affichera dans la barre du haut.

```python
root.geometry("200x100")
```

Définit la taille de la fenêtre en pixels : 200 de largeur et 100 de hauteur.



#### 13.4. Définition des options du menu déroulant

```python
couleurs = ["Rouge", "Vert", "Bleu"]
```

Création d'une liste Python contenant les différentes couleurs qui seront proposées dans le menu déroulant.


#### 13.5. Création d'une variable de sélection

```python
couleur_selectionne = tk.StringVar(value="Sélectionnez une couleur")
```

On crée une variable spéciale (`StringVar`) propre à Tkinter.  
Elle servira à stocker dynamiquement la couleur choisie par l'utilisateur dans le menu déroulant.  
Sa valeur initiale est "Sélectionnez une couleur".



#### 13.6. Création du menu déroulant

```python
menu_couleur = tk.OptionMenu(root, couleur_selectionne, *couleurs)
```

Cette ligne crée un menu déroulant (`OptionMenu`) :
- `root` : le parent de l’élément, ici la fenêtre principale.
- `couleur_selectionne` : la variable qui sera automatiquement mise à jour lorsque l’utilisateur choisira une couleur.
- `*couleurs` : cette syntaxe permet de déplier la liste `["Rouge", "Vert", "Bleu"]` en trois arguments distincts.  
  Cela revient à écrire : `tk.OptionMenu(root, couleur_selectionne, "Rouge", "Vert", "Bleu")`.

- Pourquoi *couleurs ?
- Sans *, on passerait la liste entière comme un seul argument.
- Avec *, on passe chaque élément de la liste comme un argument individuel.



#### 13.7. Ajout du menu dans la fenêtre

```python
menu_couleur.pack()
```

Permet d’ajouter visuellement le menu déroulant dans la fenêtre.  
`pack()` est un gestionnaire de placement automatique.



#### 13.8. Lancement de l’interface

```python
root.mainloop()
```

Cette instruction lance la boucle principale de l’interface.  
Elle attend les actions de l’utilisateur (clics, sélections) et met à jour l’affichage en conséquence.  
C’est cette ligne qui rend la fenêtre interactive et visible à l’écran.




<br/>

# 14. Variables Tkinter

Tkinter utilise des variables spéciales : `StringVar`, `IntVar`, `DoubleVar`, `BooleanVar`.

Exemples :

```python
import tkinter as tk

my_var = tk.StringVar(value="valeur")
my_int_var = tk.IntVar(value=12)
my_double_var = tk.DoubleVar(value=2.2)
my_bool_var = tk.BooleanVar(value=False)

# Exemple d'utilisation
my_label = tk.Label(root, textvariable=my_var)
```




### 14.1. Variables Tkinter – Explication détaillée

Tkinter ne fonctionne pas directement avec les types standards de Python (`str`, `int`, `float`, `bool`) lorsqu'il s'agit de **liens dynamiques entre les widgets et les valeurs**.  
Pour cela, il utilise **des classes spéciales** appelées :

- `StringVar` pour les chaînes de caractères
- `IntVar` pour les entiers
- `DoubleVar` pour les nombres à virgule
- `BooleanVar` pour les booléens

Ces classes permettent à Tkinter :
- de **lier dynamiquement** une valeur à un widget (par exemple un `Label`, un `Entry`, un `Checkbutton`, etc.)
- de **mettre à jour automatiquement** l’interface lorsque la valeur change
- ou de **récupérer automatiquement** la valeur quand l'utilisateur interagit avec un widget



### 14.2. Exemple commenté

```python
import tkinter as tk
```

On importe Tkinter avec l'alias `tk`.

```python
my_var = tk.StringVar(value="valeur")
```

On crée une variable de type `StringVar`, initialisée à `"valeur"`.  
Elle peut être liée à un widget texte, comme une étiquette (`Label`) ou un champ de saisie (`Entry`).

```python
my_int_var = tk.IntVar(value=12)
```

Variable de type `IntVar`, initialisée à 12.  
Utile pour lier des widgets comme des `Spinbox`, `Scale`, ou `Radiobutton`.

```python
my_double_var = tk.DoubleVar(value=2.2)
```

Variable pour stocker un nombre à virgule flottante.

```python
my_bool_var = tk.BooleanVar(value=False)
```

Variable booléenne (vrai ou faux), très utile pour des cases à cocher (`Checkbutton`).



### 14.3. Utilisation dans un widget

```python
my_label = tk.Label(root, textvariable=my_var)
```

Ce `Label` n'affichera pas un texte fixe donné par `text="..."`,  
mais il affichera **le contenu de la variable `my_var`**.

Ainsi, si plus tard on exécute :

```python
my_var.set("Bonjour")
```

Le texte affiché par le `Label` sera automatiquement mis à jour avec `"Bonjour"`.



### 14.4. Conclusion

Les variables Tkinter (`StringVar`, etc.) sont essentielles pour :
- **créer des interfaces interactives**
- **lier les données de votre programme aux éléments graphiques**
- **mettre à jour automatiquement l’affichage sans intervention manuelle**



[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 15. Changer dynamiquement une variable Tkinter

```python
import tkinter as tk

def update_label():
    label_text.set(entry.get())

root = tk.Tk()
root.title("Exemple avec tkinter")

label_text = tk.StringVar()

entry = tk.Entry(root, textvariable=label_text)
entry.pack(pady=10)

label = tk.Label(root, textvariable=label_text) # Ce qui est le plus correcte ici c'est label = tk.Label(root, text="test") , pourquoi?
label.pack()

update_button = tk.Button(root, text="Mettre à jour l'étiquette", command=update_label)
update_button.pack(pady=10)

root.mainloop()
```


# Explications


## 15.1. Objectif du code

Ce code montre **comment lier un champ de saisie (`Entry`) à une étiquette (`Label`) via une variable `StringVar`**, et comment **mettre à jour dynamiquement le texte affiché** en appuyant sur un bouton.



## 15.2. Analyse ligne par ligne

```python
import tkinter as tk
```
On importe la bibliothèque Tkinter avec l'alias `tk`.



### 15.3. Fonction de mise à jour

```python
def update_label():
    label_text.set(entry.get())
```

Cette fonction est exécutée lorsqu'on clique sur le bouton.  
Elle récupère le texte saisi dans le champ `entry` avec `entry.get()`  
et le **copie dans la variable Tkinter `label_text`** avec `set(...)`.

Puisque le `Label` est lié à `label_text`, son contenu affiché sera **mis à jour automatiquement**.



### 15.4. Fenêtre principale

```python
root = tk.Tk()
root.title("Exemple avec tkinter")
```

Création de la fenêtre principale et définition d’un titre.



### 15.5. Création de la variable partagée

```python
label_text = tk.StringVar()
```

On crée une variable Tkinter qui pourra être utilisée à la fois par l’entrée (`Entry`) et par l’étiquette (`Label`).



### 15.6. Champ de saisie

```python
entry = tk.Entry(root, textvariable=label_text)
entry.pack(pady=10)
```

Création d’un champ de saisie (`Entry`) **lié à la variable `label_text`**.  
Ce que l’utilisateur tape mettra automatiquement à jour cette variable.



### 15.7. Étiquette affichant le contenu

```python
label = tk.Label(root, textvariable=label_text)
label.pack()
```

Ce `Label` affiche **la valeur actuelle de `label_text`**.  
Il est donc mis à jour dynamiquement dès que la variable change.

> **Commentaire dans le code** :
> ```python
> # Ce qui est le plus correct ici c'est label = tk.Label(root, text="test")
> ```
> Cette remarque est **fausse dans le contexte** du comportement dynamique recherché.

Explication :
- Si on utilise `text="test"` → le `Label` affiche **un texte fixe** ("test") et **ne changera jamais**, même si `label_text` est modifié.
- Si on utilise `textvariable=label_text` → le `Label` est **lié à la variable**, et donc réagira automatiquement à tout changement de `label_text`.

Donc, dans ce programme, **`textvariable=label_text` est le bon choix**.



### 15.8. Bouton pour déclencher la mise à jour

```python
update_button = tk.Button(root, text="Mettre à jour l'étiquette", command=update_label)
update_button.pack(pady=10)
```

Ce bouton déclenche la fonction `update_label()` lorsque l'utilisateur clique dessus.  
Il met donc à jour le `Label` avec le contenu actuel de l’entrée.



### 15.9. Lancement de l’application

```python
root.mainloop()
```

Démarre la boucle principale de l’interface graphique.



### 15.10. Conclusion

- Utiliser `text="..."` crée un `Label` statique.
- Utiliser `textvariable=...` permet de **lier le `Label` à une variable dynamique** (comme une entrée utilisateur).
- Le bouton permet de **synchroniser manuellement** la saisie et l’affichage, ce qui est utile quand on ne veut pas que le label change instantanément à chaque frappe.



[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 16. Évènements de Clavier et de Souris

### Clavier : Détecter l'appui d’une touche<
```python
import tkinter as tk

def fonction_de_rappel(event):
    print("Touche enfoncée :", event.keysym)

root = tk.Tk()
root.bind("<KeyPress>", fonction_de_rappel)

root.mainloop()
```

### Souris : Détecter le mouvement ou le clic
```python
import tkinter as tk

def fonction_de_rappel(event):
    print("Position de la souris :", event.x, event.y)

root = tk.Tk()
root.bind("<Motion>", fonction_de_rappel)

root.mainloop()
```




# Explications:

## 16.1. Objectif général

Tkinter permet de **réagir à des actions de l'utilisateur** telles que :
- l'appui sur une touche du clavier
- le clic ou le déplacement de la souris

On appelle cela **la gestion des événements**.  
On utilise pour cela la méthode `bind()` qui **lie un événement** à une **fonction de rappel (callback)**.



## 16.2. Partie 1 : Événement Clavier

### Code :

```python
import tkinter as tk

def fonction_de_rappel(event):
    print("Touche enfoncée :", event.keysym)

root = tk.Tk()
root.bind("<KeyPress>", fonction_de_rappel)

root.mainloop()
```

### Explication détaillée :

1. `import tkinter as tk`  
   On importe Tkinter.

2. `def fonction_de_rappel(event):`  
   Déclaration d’une fonction appelée **à chaque fois qu’un événement est détecté**.  
   L’argument `event` contient des informations comme la touche pressée, la position de la souris, etc.

3. `print("Touche enfoncée :", event.keysym)`  
   Affiche le **nom de la touche appuyée** (par exemple : `"a"`, `"Shift_L"`, `"Return"`, etc.).

4. `root = tk.Tk()`  
   Création de la fenêtre principale.

5. `root.bind("<KeyPress>", fonction_de_rappel)`  
   On demande à Tkinter :  
   *“À chaque fois qu’une touche est appuyée, appelle `fonction_de_rappel`.”*

6. `root.mainloop()`  
   Lance l’interface graphique et attend les interactions.



## 16.3. Partie 2 : Événement Souris

### Code :

```python
import tkinter as tk

def fonction_de_rappel(event):
    print("Position de la souris :", event.x, event.y)

root = tk.Tk()
root.bind("<Motion>", fonction_de_rappel)

root.mainloop()
```

### Explication détaillée :

1. `def fonction_de_rappel(event):`  
   Fonction appelée dès que la souris bouge dans la fenêtre.

2. `event.x`, `event.y`  
   Coordonnées du pointeur de la souris au moment du mouvement.

3. `root.bind("<Motion>", fonction_de_rappel)`  
   On lie le mouvement de la souris (`<Motion>`) à la fonction.  
   Chaque déplacement déclenche l’appel de la fonction.



## 16.4. Liste d'événements courants (à retenir) :

| Événement        | Description                                 |
|------------------|---------------------------------------------|
| `<KeyPress>`     | Touche du clavier enfoncée                  |
| `<KeyRelease>`   | Touche relâchée                             |
| `<Button-1>`     | Clic gauche de la souris                    |
| `<Button-3>`     | Clic droit de la souris                     |
| `<Double-Button-1>` | Double clic gauche                      |
| `<Motion>`       | Mouvement de la souris                      |
| `<Enter>`        | Curseur entre dans un widget                |
| `<Leave>`        | Curseur quitte un widget                    |



[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 17. Conclusion

Avec Tkinter, on peut créer des interfaces riches et interactives en Python grâce :
- Aux **widgets**,
- À la **gestion des évènements**,
- Et aux **variables dynamiques** (`StringVar`, `IntVar`, etc.).

[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>

# 18. Annexe


# ⚙ **Préparation de l’environnement sous Windows**

Avant de commencer, crée un environnement virtuel pour isoler le projet :

```bash
# Ouvre l'invite de commandes Windows (cmd) ou PowerShell
# 1. Crée un dossier pour ton projet
mkdir tkinter_projets
cd tkinter_projets

# 2. Crée un environnement virtuel
python -m venv venv

# 3. Active l’environnement virtuel
venv\Scripts\activate

# 4. Tu peux maintenant créer tes fichiers .py dans ce dossier
# (Tkinter est déjà inclus avec Python, pas besoin d'installation)
```


[Revenir à la Table des matières – Tkinter](#table-des-matières-tkinter)

<br/>
