# Exercice 0 - Introduction Tres Facile

## Objectifs
- Premiers pas avec Python et les interfaces graphiques
- Comprendre les concepts de base sans complexite
- Prendre confiance avec des exercices simples

## Exercice 0.1 - Ma Premiere Fenetre

### Enonce
Creez votre toute premiere fenetre avec Tkinter. C'est tres simple !

**Ce que vous devez faire :**
- Creer une fenetre
- Lui donner un titre
- Afficher la fenetre

### Code Complet (Exemple)
```python
import tkinter as tk

# Creer la fenetre
fenetre = tk.Tk()

# Donner un titre
fenetre.title("Ma Premiere Fenetre")

# Definir la taille
fenetre.geometry("300x200")

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
Copiez ce code et executez-le. Vous devriez voir une fenetre vide s'ouvrir !

### Criteres de Validation
- [ ] La fenetre s'ouvre
- [ ] Le titre est affiche
- [ ] La fenetre a la bonne taille

---

## Exercice 0.2 - Afficher du Texte

### Enonce
Ajoutons du texte dans notre fenetre !

**Ce que vous devez faire :**
- Creer une fenetre
- Ajouter un texte qui dit "Bonjour le monde !"
- Afficher la fenetre

### Code a Completer
```python
import tkinter as tk

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Bonjour le monde")
fenetre.geometry("300x200")

# Creer un texte (Label)
texte = tk.Label(fenetre, text="Bonjour le monde !")
texte.pack()

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Changez le texte pour dire "Bonjour [votre prenom] !"

### Criteres de Validation
- [ ] Le texte s'affiche dans la fenetre
- [ ] Vous avez personnalise le message

---

## Exercice 0.3 - Mon Premier Bouton

### Enonce
Ajoutons un bouton qui fait quelque chose quand on clique dessus !

### Code Complet
```python
import tkinter as tk

def quand_on_clique():
    print("Vous avez clique sur le bouton !")

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Mon Premier Bouton")
fenetre.geometry("300x200")

# Creer un bouton
bouton = tk.Button(fenetre, text="Cliquez-moi !", command=quand_on_clique)
bouton.pack(pady=20)

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Cliquez sur le bouton et regardez dans la console
4. Changez le texte du bouton

### Criteres de Validation
- [ ] Le bouton s'affiche
- [ ] Un message apparait dans la console quand vous cliquez
- [ ] Vous avez personnalise le texte du bouton

---

## Exercice 0.4 - Bouton qui Change le Texte

### Enonce
Faisons un bouton qui change le texte dans la fenetre !

### Code a Completer
```python
import tkinter as tk

def changer_texte():
    # Cette fonction va changer le texte
    texte_label.config(text="Le texte a change !")

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Changer le Texte")
fenetre.geometry("300x200")

# Creer un texte
texte_label = tk.Label(fenetre, text="Texte original")
texte_label.pack(pady=10)

# Creer un bouton
bouton = tk.Button(fenetre, text="Changer le texte", command=changer_texte)
bouton.pack(pady=10)

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Cliquez sur le bouton pour voir le texte changer
4. Essayez de changer le nouveau texte

### Criteres de Validation
- [ ] Le texte original s'affiche
- [ ] Le texte change quand vous cliquez sur le bouton
- [ ] Vous comprenez comment ca fonctionne

---

## Exercice 0.5 - Compteur Simple

### Enonce
Creeons un compteur qui augmente quand on clique sur un bouton !

### Code Complet
```python
import tkinter as tk

# Variable pour compter
compteur = 0

def augmenter_compteur():
    global compteur
    compteur = compteur + 1
    texte_compteur.config(text=f"Compteur: {compteur}")

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Compteur Simple")
fenetre.geometry("300x200")

# Affichage du compteur
texte_compteur = tk.Label(fenetre, text="Compteur: 0", font=("Arial", 16))
texte_compteur.pack(pady=20)

# Bouton pour augmenter
bouton_plus = tk.Button(fenetre, text="+1", command=augmenter_compteur, font=("Arial", 14))
bouton_plus.pack(pady=10)

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Cliquez plusieurs fois sur le bouton
4. Regardez le compteur augmenter

### Defis Bonus (Optionnel)
- Ajoutez un bouton "-1" pour diminuer le compteur
- Ajoutez un bouton "Reset" pour remettre a zero
- Changez la couleur du texte

### Criteres de Validation
- [ ] Le compteur commence a 0
- [ ] Le compteur augmente a chaque clic
- [ ] Vous comprenez le principe

---

## Exercice 0.6 - Saisir son Nom

### Enonce
Faisons une application qui demande votre nom et vous salue !

### Code Complet
```python
import tkinter as tk

def dire_bonjour():
    nom = entree_nom.get()
    if nom:
        message_label.config(text=f"Bonjour {nom} !")
    else:
        message_label.config(text="Veuillez saisir votre nom")

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Dire Bonjour")
fenetre.geometry("400x200")

# Instructions
instruction = tk.Label(fenetre, text="Saisissez votre nom:")
instruction.pack(pady=10)

# Zone de saisie
entree_nom = tk.Entry(fenetre, font=("Arial", 12))
entree_nom.pack(pady=5)

# Bouton
bouton_bonjour = tk.Button(fenetre, text="Dire Bonjour", command=dire_bonjour)
bouton_bonjour.pack(pady=10)

# Message de reponse
message_label = tk.Label(fenetre, text="", font=("Arial", 12), fg="blue")
message_label.pack(pady=10)

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Saisissez votre nom dans la zone de texte
4. Cliquez sur "Dire Bonjour"
5. Essayez de laisser vide pour voir le message d'erreur

### Criteres de Validation
- [ ] Vous pouvez saisir votre nom
- [ ] L'application vous salue avec votre nom
- [ ] Un message d'erreur apparait si vous ne saisissez rien

---

## Exercice 0.7 - Calculatrice Ultra Simple

### Enonce
Faisons une calculatrice qui additionne deux nombres !

### Code a Completer
```python
import tkinter as tk

def additionner():
    try:
        # Recuperer les nombres
        nombre1 = float(entree1.get())
        nombre2 = float(entree2.get())
        
        # Calculer
        resultat = nombre1 + nombre2
        
        # Afficher le resultat
        label_resultat.config(text=f"Resultat: {resultat}")
        
    except ValueError:
        label_resultat.config(text="Veuillez saisir des nombres valides")

# Creer la fenetre
fenetre = tk.Tk()
fenetre.title("Calculatrice Simple")
fenetre.geometry("300x250")

# Premier nombre
tk.Label(fenetre, text="Premier nombre:").pack(pady=5)
entree1 = tk.Entry(fenetre)
entree1.pack(pady=5)

# Deuxieme nombre
tk.Label(fenetre, text="Deuxieme nombre:").pack(pady=5)
entree2 = tk.Entry(fenetre)
entree2.pack(pady=5)

# Bouton calculer
bouton_calculer = tk.Button(fenetre, text="Additionner", command=additionner)
bouton_calculer.pack(pady=10)

# Resultat
label_resultat = tk.Label(fenetre, text="Resultat: ", font=("Arial", 12), fg="green")
label_resultat.pack(pady=10)

# Afficher la fenetre
fenetre.mainloop()
```

### A Faire
1. Copiez ce code
2. Executez-le
3. Saisissez deux nombres
4. Cliquez sur "Additionner"
5. Essayez avec des nombres decimaux (ex: 3.5)
6. Essayez de saisir du texte pour voir l'erreur

### Criteres de Validation
- [ ] L'addition fonctionne correctement
- [ ] Les nombres decimaux sont acceptes
- [ ] Un message d'erreur apparait pour les saisies invalides

---

## Questions Tres Faciles

### Question 1
Que fait cette ligne de code ?
```python
fenetre = tk.Tk()
```

**Reponse :** Elle cree une nouvelle fenetre

### Question 2
Comment afficher une fenetre ?

**Reponse :** Avec `fenetre.mainloop()`

### Question 3
Que fait `pack()` ?

**Reponse :** Ca place un widget dans la fenetre

### Question 4
Comment changer le texte d'un Label ?

**Reponse :** Avec `label.config(text="nouveau texte")`

### Question 5
Comment recuperer le texte saisi dans un Entry ?

**Reponse :** Avec `entry.get()`

---

## Conseils pour Reussir

### 1. Copiez-Collez d'Abord
- Copiez le code tel quel
- Executez-le pour voir comment ca marche
- Puis modifiez petit a petit

### 2. Experimentez
- Changez les textes
- Changez les couleurs
- Changez les tailles

### 3. Lisez les Messages d'Erreur
- Si ca ne marche pas, lisez l'erreur
- Souvent c'est juste une faute de frappe

### 4. Amusez-vous !
- C'est normal de faire des erreurs
- Chaque erreur vous apprend quelque chose
- L'important c'est d'essayer !

---

## Prochaines Etapes

Une fois que vous maitrisez ces exercices tres faciles, vous pouvez passer a :
- `01-tkinter-bases.md` : Exercices un peu plus avances
- `02-tkinter-widgets.md` : Widgets plus complexes

## Aide

Si vous etes bloque :
1. Relisez le code ligne par ligne
2. Verifiez que vous avez bien copie tout le code
3. Assurez-vous que l'indentation est correcte (les espaces au debut des lignes)
4. Demandez de l'aide a votre enseignant

Bon courage et amusez-vous bien avec vos premieres interfaces graphiques !
