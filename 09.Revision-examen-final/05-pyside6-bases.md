# Exercice 5 - PySide6 Bases

## Objectifs
- Maitriser les concepts fondamentaux de PySide6
- Comprendre l'architecture Qt et les widgets de base
- Implementer des interfaces modernes avec PySide6

## Exercice 5.1 - Application de Gestion de Notes

### Enonce
Developpez une application de gestion de notes d'etudiants avec PySide6 :

**Interface requise :**
- QLineEdit pour le nom de l'etudiant
- QSpinBox pour la note (0-100)
- QComboBox pour la matiere
- QPushButton pour ajouter la note
- QListWidget pour afficher les notes
- QLabel pour afficher la moyenne
- QPushButton pour calculer la moyenne
- QPushButton pour supprimer la note selectionnee

**Fonctionnalites :**
- Ajout de notes avec validation
- Calcul de moyenne automatique
- Suppression de notes
- Sauvegarde des donnees dans un fichier JSON

### Code de Base
```python
import sys
import json
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout, 
                              QHBoxLayout, QLineEdit, QSpinBox, QComboBox, 
                              QPushButton, QListWidget, QLabel, QMessageBox)
from PySide6.QtCore import Qt

class GestionnaireNotes(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Gestionnaire de Notes")
        self.setGeometry(100, 100, 600, 500)
        
        self.notes = []  # Liste des notes
        self.fichier_donnees = "notes.json"
        
        self.init_ui()
        self.charger_donnees()
    
    def init_ui(self):
        """Initialise l'interface utilisateur"""
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # Layout principal
        main_layout = QVBoxLayout(central_widget)
        
        # Votre interface ici
        pass
    
    def ajouter_note(self):
        """Ajoute une note a la liste"""
        pass
    
    def supprimer_note(self):
        """Supprime la note selectionnee"""
        pass
    
    def calculer_moyenne(self):
        """Calcule et affiche la moyenne"""
        pass
    
    def mettre_a_jour_affichage(self):
        """Met a jour l'affichage de la liste"""
        pass
    
    def sauvegarder_donnees(self):
        """Sauvegarde les donnees dans un fichier JSON"""
        pass
    
    def charger_donnees(self):
        """Charge les donnees depuis le fichier JSON"""
        pass

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = GestionnaireNotes()
    window.show()
    sys.exit(app.exec())
```

### Criteres de Validation
- [ ] Interface complete avec tous les widgets
- [ ] Ajout de notes avec validation
- [ ] Calcul de moyenne correct
- [ ] Suppression de notes fonctionnelle
- [ ] Sauvegarde/chargement des donnees
- [ ] Interface responsive et moderne

## Exercice 5.2 - Calculatrice Scientifique

### Enonce
Creez une calculatrice scientifique avec PySide6 :

**Fonctions de base :**
- Operations arithmetiques (+, -, *, /)
- Fonctions scientifiques (sin, cos, tan, log, ln)
- Constantes (π, e)
- Parentheses et priorites
- Historique des calculs

**Interface :**
- QLineEdit pour l'affichage
- QPushButton pour chaque fonction
- QTextEdit pour l'historique
- QTabWidget pour organiser les fonctions

### Structure Recommandee
```python
import sys
import math
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QGridLayout, QPushButton, QLineEdit,
                              QTextEdit, QTabWidget, QMessageBox)
from PySide6.QtCore import Qt

class CalculatriceScientifique(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Calculatrice Scientifique")
        self.setGeometry(100, 100, 400, 600)
        
        self.expression_actuelle = ""
        self.historique = []
        
        self.init_ui()
    
    def init_ui(self):
        """Interface avec onglets"""
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        layout = QVBoxLayout(central_widget)
        
        # Affichage
        self.affichage = QLineEdit()
        self.affichage.setReadOnly(True)
        self.affichage.setStyleSheet("font-size: 18px; padding: 10px;")
        layout.addWidget(self.affichage)
        
        # Onglets
        tabs = QTabWidget()
        
        # Onglet de base
        tab_base = self.creer_onglet_base()
        tabs.addTab(tab_base, "Base")
        
        # Onglet scientifique
        tab_scientifique = self.creer_onglet_scientifique()
        tabs.addTab(tab_scientifique, "Scientifique")
        
        # Onglet historique
        tab_historique = self.creer_onglet_historique()
        tabs.addTab(tab_historique, "Historique")
        
        layout.addWidget(tabs)
    
    def creer_onglet_base(self):
        """Cree l'onglet des operations de base"""
        pass
    
    def creer_onglet_scientifique(self):
        """Cree l'onglet des fonctions scientifiques"""
        pass
    
    def creer_onglet_historique(self):
        """Cree l'onglet de l'historique"""
        pass
    
    def ajouter_caractere(self, caractere):
        """Ajoute un caractere a l'expression"""
        pass
    
    def calculer_resultat(self):
        """Calcule le resultat de l'expression"""
        pass
    
    def fonction_scientifique(self, fonction):
        """Applique une fonction scientifique"""
        pass
    
    def effacer_tout(self):
        """Efface tout"""
        pass
    
    def ajouter_a_historique(self, expression, resultat):
        """Ajoute un calcul a l'historique"""
        pass
```

### Criteres de Validation
- [ ] Operations de base fonctionnelles
- [ ] Fonctions scientifiques correctes
- [ ] Gestion des parentheses et priorites
- [ ] Historique des calculs
- [ ] Interface organisee avec onglets
- [ ] Gestion des erreurs mathematiques

## Exercice 5.3 - Gestionnaire de Contacts

### Enonce
Developpez un gestionnaire de contacts complet :

**Fonctionnalites :**
- Ajout/modification/suppression de contacts
- Recherche et filtrage
- Import/export CSV
- Groupes de contacts
- Interface avec QTableWidget

**Structure des contacts :**
- Nom, prenom, telephone, email
- Adresse complete
- Date de naissance
- Groupe (famille, travail, amis)
- Photo (optionnel)

### Code de Depart
```python
import sys
import csv
from datetime import datetime
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QTableWidget, QTableWidgetItem,
                              QPushButton, QLineEdit, QComboBox, QLabel,
                              QDialog, QFormLayout, QDateEdit, QTextEdit,
                              QFileDialog, QMessageBox, QHeaderView)
from PySide6.QtCore import Qt, QDate

class Contact:
    def __init__(self, nom="", prenom="", telephone="", email="", 
                 adresse="", date_naissance=None, groupe=""):
        self.nom = nom
        self.prenom = prenom
        self.telephone = telephone
        self.email = email
        self.adresse = adresse
        self.date_naissance = date_naissance
        self.groupe = groupe

class GestionnaireContacts(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Gestionnaire de Contacts")
        self.setGeometry(100, 100, 900, 600)
        
        self.contacts = []
        self.contacts_filtres = []
        
        self.init_ui()
    
    def init_ui(self):
        """Interface principale avec tableau"""
        pass
    
    def ajouter_contact(self):
        """Ouvre la fenetre d'ajout de contact"""
        pass
    
    def modifier_contact(self):
        """Modifie le contact selectionne"""
        pass
    
    def supprimer_contact(self):
        """Supprime le contact selectionne"""
        pass
    
    def rechercher_contacts(self):
        """Filtre les contacts selon la recherche"""
        pass
    
    def mettre_a_jour_tableau(self):
        """Met a jour l'affichage du tableau"""
        pass
    
    def exporter_csv(self):
        """Exporte les contacts en CSV"""
        pass
    
    def importer_csv(self):
        """Importe des contacts depuis CSV"""
        pass

class DialogueContact(QDialog):
    """Fenetre de saisie/modification de contact"""
    def __init__(self, contact=None, parent=None):
        super().__init__(parent)
        self.contact = contact
        self.init_ui()
    
    def init_ui(self):
        """Interface du dialogue"""
        pass
    
    def valider_donnees(self):
        """Valide les donnees saisies"""
        pass
```

### Criteres de Validation
- [ ] CRUD complet des contacts
- [ ] Recherche et filtrage efficaces
- [ ] Import/export CSV fonctionnel
- [ ] Interface claire avec QTableWidget
- [ ] Validation des donnees
- [ ] Gestion des groupes

## Exercice 5.4 - Lecteur de Fichiers Multimedia

### Enonce
Creez un lecteur multimedia simple :

**Fonctionnalites :**
- Lecture de fichiers audio/video
- Controles de lecture (play, pause, stop)
- Barre de progression
- Controle du volume
- Playlist simple
- Informations sur le fichier

**Widgets utilises :**
- QMediaPlayer pour la lecture
- QVideoWidget pour l'affichage video
- QSlider pour la progression et le volume
- QListWidget pour la playlist
- QPushButton pour les controles

### Structure de Base
```python
import sys
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QPushButton, QSlider, QLabel,
                              QListWidget, QFileDialog, QMessageBox)
from PySide6.QtMultimedia import QMediaPlayer, QAudioOutput
from PySide6.QtMultimediaWidgets import QVideoWidget
from PySide6.QtCore import Qt, QUrl

class LecteurMultimedia(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Lecteur Multimedia")
        self.setGeometry(100, 100, 800, 600)
        
        self.media_player = QMediaPlayer()
        self.audio_output = QAudioOutput()
        self.media_player.setAudioOutput(self.audio_output)
        
        self.playlist = []
        self.index_actuel = 0
        
        self.init_ui()
        self.connecter_signaux()
    
    def init_ui(self):
        """Interface du lecteur"""
        pass
    
    def connecter_signaux(self):
        """Connecte les signaux et slots"""
        pass
    
    def ouvrir_fichier(self):
        """Ouvre un fichier multimedia"""
        pass
    
    def jouer_pause(self):
        """Alterne entre lecture et pause"""
        pass
    
    def arreter(self):
        """Arrete la lecture"""
        pass
    
    def changer_position(self, position):
        """Change la position de lecture"""
        pass
    
    def changer_volume(self, volume):
        """Change le volume"""
        pass
    
    def fichier_suivant(self):
        """Passe au fichier suivant"""
        pass
    
    def fichier_precedent(self):
        """Revient au fichier precedent"""
        pass
```

### Criteres de Validation
- [ ] Lecture de fichiers multimedia
- [ ] Controles de lecture fonctionnels
- [ ] Barre de progression interactive
- [ ] Gestion du volume
- [ ] Playlist operationnelle
- [ ] Interface intuitive

## Questions de Reflexion

1. **Architecture Qt** : Quelles sont les differences principales entre Tkinter et PySide6 ?

2. **Signaux et slots** : Comment ce mecanisme ameliore-t-il la gestion des evenements ?

3. **Layouts** : Quels sont les avantages des layouts Qt par rapport aux systemes de positionnement ?

4. **Performance** : Comment PySide6 gere-t-il les performances avec de gros volumes de donnees ?

## Ressources Complementaires

- **Cours de reference** : `11. PyQt501 - Introduction`, `14. Introduction complete a Qt6, PySide6`
- **Qt Designer** : `15. PySide6 et Qt Designer`
- **Documentation** : Qt for Python Documentation

## Conseils

1. **Utilisez les layouts** : Ne positionnez jamais manuellement les widgets
2. **Gerez les ressources** : Fermez proprement les fichiers et connexions
3. **Testez sur differentes tailles** : Votre interface doit etre responsive
4. **Suivez les conventions Qt** : Nommage et organisation du code
5. **Exploitez les signaux** : Utilisez le mecanisme signal/slot au maximum

Bon developpement avec PySide6 !
