# Exercice 2 - Tkinter Widgets Avances

## Objectifs
- Maitriser les widgets avances de Tkinter
- Comprendre la gestion des layouts
- Implementer des interfaces complexes avec plusieurs composants

## Exercice 2.1 - Formulaire d'Inscription Complet

### Enonce
Developpez un formulaire d'inscription avec validation complete :

**Composants requis :**
- Entry pour nom, prenom, email
- Radiobuttons pour le genre (Homme/Femme/Autre)
- Checkbuttons pour les hobbies (Lecture, Sport, Musique, Cinema)
- Combobox pour le pays
- Scale pour l'age (18-100)
- Text widget pour commentaires
- Buttons Valider/Annuler

**Validations :**
- Champs obligatoires : nom, prenom, email
- Format email valide
- Au moins un hobby selectionne
- Age entre 18 et 100 ans

### Structure de Base
```python
import tkinter as tk
from tkinter import ttk, messagebox
import re

class FormulaireInscription:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Formulaire d'Inscription")
        self.root.geometry("500x600")
        
        # Variables Tkinter
        self.nom_var = tk.StringVar()
        self.prenom_var = tk.StringVar()
        self.email_var = tk.StringVar()
        self.genre_var = tk.StringVar(value="Homme")
        self.age_var = tk.IntVar(value=25)
        
        # Variables pour hobbies
        self.hobby_lecture = tk.BooleanVar()
        self.hobby_sport = tk.BooleanVar()
        self.hobby_musique = tk.BooleanVar()
        self.hobby_cinema = tk.BooleanVar()
        
        self.create_widgets()
    
    def create_widgets(self):
        # Votre interface ici
        pass
    
    def valider_email(self, email):
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return re.match(pattern, email) is not None
    
    def valider_formulaire(self):
        # Logique de validation
        pass
    
    def soumettre(self):
        # Traitement de la soumission
        pass
    
    def annuler(self):
        # Reset du formulaire
        pass
```

### Criteres de Validation
- [ ] Tous les widgets requis presents
- [ ] Validation des champs obligatoires
- [ ] Validation du format email
- [ ] Gestion des radiobuttons et checkbuttons
- [ ] Utilisation correcte du Scale
- [ ] Interface bien organisee avec layouts

## Exercice 2.2 - Explorateur de Fichiers Simple

### Enonce
Creez un mini-explorateur de fichiers :

**Composants :**
- Treeview pour afficher l'arborescence des dossiers
- Listbox pour afficher les fichiers du dossier selectionne
- Label pour afficher le chemin actuel
- Buttons pour naviguer (Precedent, Suivant, Accueil)
- Entry pour rechercher des fichiers

**Fonctionnalites :**
- Navigation dans l'arborescence
- Affichage des fichiers et dossiers
- Recherche simple par nom
- Historique de navigation

### Code de Depart
```python
import tkinter as tk
from tkinter import ttk
import os

class ExplorateurFichiers:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Explorateur de Fichiers")
        self.root.geometry("800x600")
        
        self.chemin_actuel = os.path.expanduser("~")  # Dossier utilisateur
        self.historique = []
        self.index_historique = -1
        
        self.create_widgets()
        self.charger_dossier(self.chemin_actuel)
    
    def create_widgets(self):
        # Interface avec Treeview et Listbox
        pass
    
    def charger_dossier(self, chemin):
        # Charger le contenu d'un dossier
        pass
    
    def on_dossier_select(self, event):
        # Gestion de la selection dans le Treeview
        pass
    
    def naviguer_precedent(self):
        # Navigation precedente
        pass
    
    def naviguer_suivant(self):
        # Navigation suivante
        pass
    
    def rechercher_fichiers(self):
        # Recherche de fichiers
        pass
```

### Criteres de Validation
- [ ] Treeview fonctionnel pour les dossiers
- [ ] Listbox affichant les fichiers
- [ ] Navigation avec historique
- [ ] Fonction de recherche
- [ ] Interface responsive et intuitive

## Exercice 2.3 - Editeur de Texte avec Menu

### Enonce
Developpez un editeur de texte simple avec menu :

**Composants :**
- Menu principal (Fichier, Edition, Aide)
- Text widget pour l'edition
- Barre d'outils avec boutons (Nouveau, Ouvrir, Sauvegarder)
- Barre de statut avec informations

**Fonctionnalites Menu Fichier :**
- Nouveau document
- Ouvrir fichier
- Sauvegarder
- Sauvegarder sous
- Quitter

**Fonctionnalites Edition :**
- Couper, Copier, Coller
- Rechercher et remplacer
- Selectionner tout

### Structure Recommandee
```python
import tkinter as tk
from tkinter import filedialog, messagebox

class EditeurTexte:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Editeur de Texte")
        self.root.geometry("800x600")
        
        self.fichier_actuel = None
        self.contenu_modifie = False
        
        self.create_menu()
        self.create_toolbar()
        self.create_text_area()
        self.create_statusbar()
    
    def create_menu(self):
        # Creation du menu principal
        pass
    
    def create_toolbar(self):
        # Barre d'outils
        pass
    
    def create_text_area(self):
        # Zone de texte principale
        pass
    
    def create_statusbar(self):
        # Barre de statut
        pass
    
    def nouveau_fichier(self):
        # Nouveau document
        pass
    
    def ouvrir_fichier(self):
        # Ouvrir un fichier
        pass
    
    def sauvegarder_fichier(self):
        # Sauvegarder
        pass
    
    def rechercher_remplacer(self):
        # Fenetre de recherche/remplacement
        pass
```

### Criteres de Validation
- [ ] Menu complet et fonctionnel
- [ ] Operations sur fichiers (nouveau, ouvrir, sauvegarder)
- [ ] Fonctions d'edition (couper, copier, coller)
- [ ] Recherche et remplacement
- [ ] Barre de statut informative
- [ ] Gestion des modifications non sauvegardees

## Exercice 2.4 - Application de Dessin

### Enonce
Creez une application de dessin simple :

**Composants :**
- Canvas pour dessiner
- Palette de couleurs
- Outils de dessin (pinceau, ligne, rectangle, cercle)
- Controles pour la taille du pinceau
- Boutons Effacer et Sauvegarder

**Fonctionnalites :**
- Dessin libre a la souris
- Selection de couleurs
- Differents outils de dessin
- Ajustement de la taille
- Sauvegarde en image

### Code de Base
```python
import tkinter as tk
from tkinter import colorchooser, filedialog
from PIL import Image, ImageDraw

class ApplicationDessin:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Application de Dessin")
        self.root.geometry("900x700")
        
        self.couleur_actuelle = "black"
        self.taille_pinceau = 2
        self.outil_actuel = "pinceau"
        self.dernier_x = None
        self.dernier_y = None
        
        self.create_widgets()
        self.bind_events()
    
    def create_widgets(self):
        # Interface avec Canvas et controles
        pass
    
    def bind_events(self):
        # Liaison des evenements souris
        pass
    
    def dessiner(self, event):
        # Fonction de dessin
        pass
    
    def choisir_couleur(self):
        # Selection de couleur
        pass
    
    def changer_outil(self, nouvel_outil):
        # Changement d'outil
        pass
    
    def sauvegarder_image(self):
        # Sauvegarde de l'image
        pass
```

### Criteres de Validation
- [ ] Canvas fonctionnel pour le dessin
- [ ] Selection de couleurs
- [ ] Differents outils de dessin
- [ ] Controle de la taille du pinceau
- [ ] Sauvegarde d'images
- [ ] Interface intuitive et responsive

## Questions de Reflexion

1. **Layouts complexes** : Comment organiser efficacement une interface avec de nombreux widgets ?

2. **Gestion des evenements** : Quelles sont les bonnes pratiques pour gerer les evenements souris et clavier ?

3. **Variables Tkinter** : Pourquoi utiliser StringVar, IntVar, BooleanVar plutot que des variables Python classiques ?

4. **Performance** : Comment optimiser les performances d'une interface Tkinter complexe ?

## Ressources Complementaires

- **Cours de reference** : `02. Tkinter - Comprendre Composants Widgets et les Evenements Graphiques`
- **Travaux pratiques** : `04. Tkinter - Travail pratique 2`, `05. Tkinter - Travail pratique 3`
- **Layouts** : Documentation Tkinter sur grid, pack, et place

## Conseils Avances

1. **Planifiez l'interface** : Dessinez d'abord votre interface sur papier
2. **Utilisez les layouts** : Maitrisez grid() pour des interfaces complexes
3. **Separez la logique** : Gardez la logique metier separee de l'interface
4. **Testez l'ergonomie** : Verifiez que l'interface est intuitive
5. **Gerez les ressources** : Attention aux performances avec de gros widgets

Bon developpement !
