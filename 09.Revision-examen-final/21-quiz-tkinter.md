# Quiz 21 - Tkinter

## Questions a Choix Multiples

### Question 1
Quelle est la methode correcte pour creer une fenetre principale en Tkinter ?

a) `window = tkinter.Window()`  
b) `window = tk.Tk()`  
c) `window = tkinter.MainWindow()`  
d) `window = tk.Frame()`  

**Reponse correcte :** b) `window = tk.Tk()`

---

### Question 2
Comment definir le titre d'une fenetre Tkinter ?

a) `window.set_title("Mon Titre")`  
b) `window.title = "Mon Titre"`  
c) `window.title("Mon Titre")`  
d) `window.caption("Mon Titre")`  

**Reponse correcte :** c) `window.title("Mon Titre")`

---

### Question 3
Quelle est la difference entre `pack()`, `grid()` et `place()` ?

a) Ils font la meme chose  
b) `pack()` pour un seul widget, `grid()` pour plusieurs, `place()` pour les images  
c) Ce sont trois gestionnaires de disposition differents  
d) `pack()` est obsolete, il faut utiliser `grid()` ou `place()`  

**Reponse correcte :** c) Ce sont trois gestionnaires de disposition differents

---

### Question 4
Comment lier un evenement de clic de souris a une fonction ?

a) `widget.onclick(ma_fonction)`  
b) `widget.bind("<Button-1>", ma_fonction)`  
c) `widget.click(ma_fonction)`  
d) `widget.on_click = ma_fonction`  

**Reponse correcte :** b) `widget.bind("<Button-1>", ma_fonction)`

---

### Question 5
Quelle est la syntaxe correcte pour creer un bouton avec une commande ?

a) `Button(root, text="OK", onclick=ma_fonction)`  
b) `Button(root, text="OK", command=ma_fonction)`  
c) `Button(root, text="OK", action=ma_fonction)`  
d) `Button(root, text="OK", callback=ma_fonction)`  

**Reponse correcte :** b) `Button(root, text="OK", command=ma_fonction)`

---

### Question 6
Comment obtenir le texte saisi dans un widget Entry ?

a) `entry.text`  
b) `entry.get_text()`  
c) `entry.get()`  
d) `entry.value`  

**Reponse correcte :** c) `entry.get()`

---

### Question 7
Quelle est la methode pour effacer le contenu d'un widget Entry ?

a) `entry.clear()`  
b) `entry.delete(0, 'end')`  
c) `entry.remove_all()`  
d) `entry.text = ""`  

**Reponse correcte :** b) `entry.delete(0, 'end')`

---

### Question 8
Comment creer une variable Tkinter pour un StringVar ?

a) `var = tk.String()`  
b) `var = tk.StringVariable()`  
c) `var = tk.StringVar()`  
d) `var = tkinter.StringVar()`  

**Reponse correcte :** c) `var = tk.StringVar()`

---

### Question 9
Quelle est l'utilite des variables Tkinter (StringVar, IntVar, etc.) ?

a) Elles sont plus rapides que les variables Python normales  
b) Elles permettent la liaison bidirectionnelle avec les widgets  
c) Elles sont obligatoires pour tous les widgets  
d) Elles occupent moins de memoire  

**Reponse correcte :** b) Elles permettent la liaison bidirectionnelle avec les widgets

---

### Question 10
Comment afficher une boite de dialogue de message d'information ?

a) `tkinter.showinfo("Titre", "Message")`  
b) `messagebox.showinfo("Titre", "Message")`  
c) `tk.info("Titre", "Message")`  
d) `dialog.info("Titre", "Message")`  

**Reponse correcte :** b) `messagebox.showinfo("Titre", "Message")`

---

## Questions Ouvertes

### Question 11
Expliquez la difference entre un widget Label et un widget Entry. Donnez un exemple d'utilisation pour chacun.

**Reponse attendue :**
- **Label** : Widget d'affichage de texte statique, non modifiable par l'utilisateur. Utilise pour afficher des titres, des descriptions, des resultats.
- **Entry** : Widget de saisie de texte sur une seule ligne, modifiable par l'utilisateur. Utilise pour les champs de formulaire, la saisie de donnees.

**Exemples :**
```python
# Label pour afficher un titre
titre = tk.Label(root, text="Formulaire de connexion")

# Entry pour saisir un nom d'utilisateur
nom_utilisateur = tk.Entry(root)
```

---

### Question 12
Qu'est-ce que la boucle d'evenements (`mainloop()`) et pourquoi est-elle necessaire ?

**Reponse attendue :**
La boucle d'evenements (`mainloop()`) est le mecanisme qui permet a l'application Tkinter de rester active et de repondre aux interactions utilisateur. Elle :
- Maintient la fenetre ouverte
- Ecoute les evenements (clics, saisie clavier, etc.)
- Traite les evenements et met a jour l'interface
- Continue jusqu'a ce que l'application soit fermee

Sans `mainloop()`, la fenetre se fermerait immediatement apres sa creation.

---

### Question 13
Comment validez-vous les entrees utilisateur dans un formulaire Tkinter ? Donnez deux methodes differentes.

**Reponse attendue :**

**Methode 1 - Validation lors de la soumission :**
```python
def valider_formulaire():
    nom = entry_nom.get()
    if not nom.strip():
        messagebox.showerror("Erreur", "Le nom est obligatoire")
        return False
    return True

bouton_valider = tk.Button(root, text="Valider", command=valider_formulaire)
```

**Methode 2 - Validation en temps reel avec validatecommand :**
```python
def valider_nombre(char):
    return char.isdigit()

vcmd = (root.register(valider_nombre), '%S')
entry_age = tk.Entry(root, validate='key', validatecommand=vcmd)
```

---

### Question 14
Expliquez comment gerer les exceptions dans une application Tkinter avec un exemple.

**Reponse attendue :**
Il est important de gerer les exceptions pour eviter que l'application plante et pour informer l'utilisateur des erreurs :

```python
import tkinter as tk
from tkinter import messagebox

def diviser():
    try:
        a = float(entry_a.get())
        b = float(entry_b.get())
        resultat = a / b
        label_resultat.config(text=f"Resultat: {resultat}")
    except ValueError:
        messagebox.showerror("Erreur", "Veuillez saisir des nombres valides")
    except ZeroDivisionError:
        messagebox.showerror("Erreur", "Division par zero impossible")
    except Exception as e:
        messagebox.showerror("Erreur", f"Erreur inattendue: {str(e)}")
```

---

### Question 15
Comment organisez-vous le code d'une application Tkinter complexe ? Proposez une structure de classe.

**Reponse attendue :**
Pour une application complexe, il est recommande d'utiliser une approche orientee objet :

```python
import tkinter as tk
from tkinter import messagebox

class MonApplication:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Mon Application")
        self.init_variables()
        self.creer_interface()
        self.connecter_evenements()
    
    def init_variables(self):
        """Initialise les variables de l'application"""
        self.donnees = []
        self.index_courant = 0
    
    def creer_interface(self):
        """Cree l'interface utilisateur"""
        # Frame principal
        self.frame_principal = tk.Frame(self.root)
        self.frame_principal.pack(padx=10, pady=10)
        
        # Widgets
        self.creer_widgets()
        self.organiser_widgets()
    
    def creer_widgets(self):
        """Cree tous les widgets"""
        self.label_titre = tk.Label(self.frame_principal, text="Titre")
        self.entry_saisie = tk.Entry(self.frame_principal)
        self.bouton_action = tk.Button(self.frame_principal, text="Action")
    
    def organiser_widgets(self):
        """Organise les widgets avec grid ou pack"""
        self.label_titre.grid(row=0, column=0, sticky="w")
        self.entry_saisie.grid(row=1, column=0, pady=5)
        self.bouton_action.grid(row=2, column=0, pady=10)
    
    def connecter_evenements(self):
        """Connecte les evenements aux methodes"""
        self.bouton_action.config(command=self.action_bouton)
        self.entry_saisie.bind('<Return>', lambda e: self.action_bouton())
    
    def action_bouton(self):
        """Action executee lors du clic sur le bouton"""
        # Logique de l'action
        pass
    
    def run(self):
        """Lance l'application"""
        self.root.mainloop()

if __name__ == "__main__":
    app = MonApplication()
    app.run()
```

---

## Questions de Code

### Question 16
Corrigez les erreurs dans ce code Tkinter :

```python
import tkinter as tk

def calculer():
    a = entry_a.get()
    b = entry_b.get()
    resultat = a + b
    label_resultat.text = resultat

root = tk.Tk()
root.title = "Calculatrice"

entry_a = tk.Entry(root)
entry_b = tk.Entry(root)
bouton = tk.Button(root, text="Calculer", command=calculer())
label_resultat = tk.Label(root, text="Resultat")

entry_a.pack()
entry_b.pack()
bouton.pack()
label_resultat.pack()

root.mainloop()
```

**Reponse correcte :**
```python
import tkinter as tk

def calculer():
    try:
        a = float(entry_a.get())  # Conversion en nombre
        b = float(entry_b.get())  # Conversion en nombre
        resultat = a + b
        label_resultat.config(text=f"Resultat: {resultat}")  # Utiliser config()
    except ValueError:
        label_resultat.config(text="Erreur: nombres invalides")

root = tk.Tk()
root.title("Calculatrice")  # Appel de methode, pas assignation

entry_a = tk.Entry(root)
entry_b = tk.Entry(root)
bouton = tk.Button(root, text="Calculer", command=calculer)  # Sans parentheses
label_resultat = tk.Label(root, text="Resultat")

entry_a.pack()
entry_b.pack()
bouton.pack()
label_resultat.pack()

root.mainloop()
```

**Erreurs corrigees :**
1. `root.title("Calculatrice")` au lieu de `root.title = "Calculatrice"`
2. `command=calculer` au lieu de `command=calculer()`
3. Conversion des entrees en float
4. `label_resultat.config(text=...)` au lieu de `label_resultat.text = ...`
5. Gestion des erreurs avec try/except

---

### Question 17
Ecrivez un code Tkinter pour creer un formulaire de contact avec validation.

**Reponse attendue :**
```python
import tkinter as tk
from tkinter import messagebox
import re

class FormulaireContact:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Formulaire de Contact")
        self.root.geometry("400x300")
        
        self.creer_interface()
    
    def creer_interface(self):
        # Frame principal
        frame = tk.Frame(self.root, padx=20, pady=20)
        frame.pack(fill="both", expand=True)
        
        # Champs du formulaire
        tk.Label(frame, text="Nom:").grid(row=0, column=0, sticky="w", pady=5)
        self.entry_nom = tk.Entry(frame, width=30)
        self.entry_nom.grid(row=0, column=1, pady=5)
        
        tk.Label(frame, text="Email:").grid(row=1, column=0, sticky="w", pady=5)
        self.entry_email = tk.Entry(frame, width=30)
        self.entry_email.grid(row=1, column=1, pady=5)
        
        tk.Label(frame, text="Message:").grid(row=2, column=0, sticky="nw", pady=5)
        self.text_message = tk.Text(frame, width=30, height=5)
        self.text_message.grid(row=2, column=1, pady=5)
        
        # Boutons
        frame_boutons = tk.Frame(frame)
        frame_boutons.grid(row=3, column=0, columnspan=2, pady=20)
        
        tk.Button(frame_boutons, text="Envoyer", command=self.valider_envoyer).pack(side="left", padx=5)
        tk.Button(frame_boutons, text="Effacer", command=self.effacer_formulaire).pack(side="left", padx=5)
    
    def valider_email(self, email):
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return re.match(pattern, email) is not None
    
    def valider_envoyer(self):
        nom = self.entry_nom.get().strip()
        email = self.entry_email.get().strip()
        message = self.text_message.get("1.0", "end-1c").strip()
        
        # Validations
        if not nom:
            messagebox.showerror("Erreur", "Le nom est obligatoire")
            self.entry_nom.focus()
            return
        
        if not email:
            messagebox.showerror("Erreur", "L'email est obligatoire")
            self.entry_email.focus()
            return
        
        if not self.valider_email(email):
            messagebox.showerror("Erreur", "L'email n'est pas valide")
            self.entry_email.focus()
            return
        
        if not message:
            messagebox.showerror("Erreur", "Le message est obligatoire")
            self.text_message.focus()
            return
        
        # Si tout est valide
        messagebox.showinfo("Succes", f"Message envoye!\nNom: {nom}\nEmail: {email}")
        self.effacer_formulaire()
    
    def effacer_formulaire(self):
        self.entry_nom.delete(0, "end")
        self.entry_email.delete(0, "end")
        self.text_message.delete("1.0", "end")
        self.entry_nom.focus()
    
    def run(self):
        self.root.mainloop()

if __name__ == "__main__":
    app = FormulaireContact()
    app.run()
```

---

## Bareme de Correction

| Section | Points | Details |
|---------|--------|---------|
| QCM (Questions 1-10) | 20 points | 2 points par question |
| Questions ouvertes (11-15) | 30 points | 6 points par question |
| Questions de code (16-17) | 30 points | 15 points par question |
| **Total** | **80 points** | |

### Criteres d'evaluation :
- **Exactitude** : La reponse est-elle correcte ?
- **Completude** : Tous les elements sont-ils abordes ?
- **Clarte** : L'explication est-elle claire et bien structuree ?
- **Bonnes pratiques** : Le code respecte-t-il les bonnes pratiques ?
- **Gestion d'erreurs** : Les cas d'erreur sont-ils pris en compte ?

### Notes de passage :
- **Excellent** : 72-80 points (90-100%)
- **Bien** : 64-71 points (80-89%)
- **Satisfaisant** : 56-63 points (70-79%)
- **Insuffisant** : < 56 points (< 70%)

Bonne chance pour votre quiz Tkinter !
