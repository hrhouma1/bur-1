# Travail pratique 1 pour initiation aux interfaces graphiques

##  Table des matières


- [1. Préparation de l’environnement](#1-préparation-de-lenvironnement)
  - [a) Créer un dossier pour le projet](#a-créer-un-dossier-pour-le-projet)
  - [b) Créer un environnement virtuel](#b-créer-un-environnement-virtuel)
  - [c) Activer l’environnement virtuel](#c-activer-lenvironnement-virtuel)
- [2. Structure recommandée du projet](#2-structure-recommandée-du-projet)
- [3. Contenu de base de `main.py`](#3-contenu-de-base-de-mainpy)
- [4. Utilisation avec Visual Studio Code](#4-utilisation-avec-visual-studio-code)
- [5. Remarques](#5-remarques)


<br/>
<br/>

# 1. Préparation de l’environnement

* *La première méthode pour exécuter votre code consiste à utiliser le terminal, que ce soit dans VS Code ou en dehors (via l’invite de commandes `cmd`).*
* *La deuxième méthode consiste à installer l’extension Python dans VS Code. Une fois installée, un bouton vert “Run” apparaîtra pour exécuter votre script.*




## a) Créer un dossier pour le projet
```bash
mkdir projet_tkinter
cd projet_tkinter
```

## b) Créer un environnement virtuel
```bash
python3 -m venv env
```

## c) Activer l’environnement virtuel

- **Sur Linux/macOS** :
```bash
source env/bin/activate
```

- **Sur Windows** :
```cmd
.\env\Scripts\activate
```

[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>



# 2. Structure recommandée du projet

```
projet_tkinter/
│
├── env/                  # Environnement virtuel
├── main.py               # Script principal avec interface Tkinter
└── README.md             # Description du projet
```


[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>



# 3. Contenu de base de `main.py`

Voici un exemple simple d’interface graphique en Tkinter :

```python
import tkinter as tk
from tkinter import messagebox

def dire_bonjour():
    nom = entree_nom.get()
    messagebox.showinfo("Bienvenue", f"Bonjour, {nom} !")

# Création de la fenêtre principale
fenetre = tk.Tk()
fenetre.title("Ma première application Tkinter")
fenetre.geometry("300x150")

# Widgets
label_nom = tk.Label(fenetre, text="Entrez votre nom :")
label_nom.pack(pady=5)

entree_nom = tk.Entry(fenetre)
entree_nom.pack(pady=5)

bouton_saluer = tk.Button(fenetre, text="Dire Bonjour", command=dire_bonjour)
bouton_saluer.pack(pady=10)

# Lancement de la boucle principale
fenetre.mainloop()
```

[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>



# 4. Utilisation avec Visual Studio Code

1. Ouvrir le projet avec VS Code :
```bash
code .
```

2. Dans le terminal intégré de VS Code, activer l’environnement virtuel.
3. Exécuter le fichier principal :
```bash
python main.py
```

4. Vérifier que VS Code utilise bien l’interpréteur de l’environnement virtuel :
   - `Ctrl+Shift+P` → `Python: Select Interpreter` → Choisir l’environnement `./env`

[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>



# 5. Remarques

- Tkinter est intégré à Python, donc aucune installation supplémentaire n’est nécessaire.
- Ce projet peut évoluer avec des widgets supplémentaires : `Canvas`, `Frame`, `Menu`, `ttk`, etc.
- La première méthode d'exécuter votre code est d'utiliser lignes de commandes dans VsCode ou à l'extérieur de VScode (cmd)
- La deuxième façon d'exécuter du code est d'installer python comme extension dans VScode et vous allez voir le bouton vert apparaître.



[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>

