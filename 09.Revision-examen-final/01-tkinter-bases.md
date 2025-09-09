# Exercice 1 - Tkinter Bases

## Objectifs
- Maitriser les composants de base de Tkinter
- Comprendre la gestion des evenements
- Implementer une interface simple et fonctionnelle

## Exercice 1.1 - Interface de Connexion

### Enonce
Creez une application Tkinter qui simule un ecran de connexion avec les elements suivants :

**Composants requis :**
- 1 Label pour le titre "Connexion Utilisateur"
- 1 Label et 1 Entry pour le nom d'utilisateur
- 1 Label et 1 Entry pour le mot de passe (masque)
- 1 Button "Se connecter"
- 1 Button "Annuler"
- 1 Label pour afficher les messages (succes/erreur)

**Fonctionnalites :**
- Validation des champs vides
- Verification des identifiants (utilisateur: "admin", mot de passe: "123456")
- Affichage de messages appropries
- Bouton Annuler efface les champs

### Code de Base
```python
import tkinter as tk
from tkinter import messagebox

class LoginApp:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Connexion Utilisateur")
        self.root.geometry("300x200")
        
        # Votre code ici
        
    def connecter(self):
        # Votre logique de connexion ici
        pass
        
    def annuler(self):
        # Votre logique d'annulation ici
        pass
        
    def run(self):
        self.root.mainloop()

if __name__ == "__main__":
    app = LoginApp()
    app.run()
```

### Criteres de Validation
- [ ] Interface complete avec tous les composants
- [ ] Validation des champs vides
- [ ] Verification des identifiants
- [ ] Messages d'erreur et de succes
- [ ] Fonctionnalite d'annulation
- [ ] Interface claire et bien organisee

## Exercice 1.2 - Calculatrice Simple

### Enonce
Developpez une calculatrice simple avec Tkinter :

**Interface :**
- 1 Entry pour afficher les nombres et resultats
- Boutons pour les chiffres 0-9
- Boutons pour les operations +, -, *, /
- Bouton = pour calculer
- Bouton C pour effacer

**Fonctionnalites :**
- Saisie de nombres multi-chiffres
- Operations arithmetiques de base
- Gestion des erreurs (division par zero)
- Effacement complet

### Structure Recommandee
```python
class Calculatrice:
    def __init__(self):
        self.root = tk.Tk()
        self.expression = ""
        self.create_widgets()
    
    def create_widgets(self):
        # Creation de l'interface
        pass
    
    def click_chiffre(self, chiffre):
        # Gestion des clics sur les chiffres
        pass
    
    def click_operation(self, operation):
        # Gestion des operations
        pass
    
    def calculer(self):
        # Calcul du resultat
        pass
    
    def effacer(self):
        # Effacement
        pass
```

### Criteres de Validation
- [ ] Interface complete avec tous les boutons
- [ ] Saisie correcte des nombres
- [ ] Operations arithmetiques fonctionnelles
- [ ] Gestion des erreurs
- [ ] Fonction d'effacement
- [ ] Affichage correct des resultats

## Exercice 1.3 - Gestionnaire de Taches

### Enonce
Creez un gestionnaire de taches simple :

**Composants :**
- 1 Entry pour saisir une nouvelle tache
- 1 Button "Ajouter"
- 1 Listbox pour afficher les taches
- 1 Button "Supprimer"
- 1 Button "Marquer comme terminee"

**Fonctionnalites :**
- Ajouter des taches a la liste
- Supprimer la tache selectionnee
- Marquer une tache comme terminee (changement de couleur ou texte)
- Validation des entrees vides

### Code de Depart
```python
class GestionnaireTaches:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Gestionnaire de Taches")
        self.taches = []
        self.create_widgets()
    
    def create_widgets(self):
        # Interface utilisateur
        pass
    
    def ajouter_tache(self):
        # Ajouter une tache
        pass
    
    def supprimer_tache(self):
        # Supprimer la tache selectionnee
        pass
    
    def marquer_terminee(self):
        # Marquer comme terminee
        pass
```

### Criteres de Validation
- [ ] Ajout de taches fonctionnel
- [ ] Suppression de taches
- [ ] Marquage des taches terminees
- [ ] Validation des entrees
- [ ] Interface intuitive
- [ ] Gestion des selections dans la Listbox

## Questions de Reflexion

1. **Gestion des evenements** : Comment Tkinter gere-t-il les evenements utilisateur ?

2. **Organisation du code** : Pourquoi est-il recommande d'utiliser des classes pour les interfaces Tkinter ?

3. **Validation des donnees** : Quelles sont les meilleures pratiques pour valider les entrees utilisateur ?

4. **Gestion des erreurs** : Comment gerer proprement les erreurs dans une interface graphique ?

## Ressources Complementaires

- **Cours de reference** : `01. Tkinter - Introduction a la librairie Tkinter`
- **Widgets avances** : `02. Tkinter - Comprendre Composants Widgets et les Evenements Graphiques`
- **Travaux pratiques** : `03. Tkinter - Travail pratique 1`

## Conseils

1. **Commencez simple** : Implementez d'abord la structure de base
2. **Testez frequemment** : Verifiez chaque fonctionnalite au fur et a mesure
3. **Organisez le code** : Utilisez des methodes separees pour chaque fonctionnalite
4. **Gerez les erreurs** : Prevoyez les cas d'erreur courants
5. **Soignez l'interface** : Une interface claire ameliore l'experience utilisateur

Bon travail !
