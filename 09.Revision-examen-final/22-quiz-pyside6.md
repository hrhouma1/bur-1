# Quiz 22 - PySide6

## Questions a Choix Multiples

### Question 1
Quelle est la difference principale entre PySide6 et PyQt6 ?

a) PySide6 est plus rapide que PyQt6  
b) PySide6 a une licence LGPL, PyQt6 a une licence GPL/commerciale  
c) PySide6 est developpe par Nokia, PyQt6 par Riverbank  
d) Il n'y a aucune difference  

**Reponse correcte :** b) PySide6 a une licence LGPL, PyQt6 a une licence GPL/commerciale

---

### Question 2
Comment creer une application PySide6 basique ?

a) `app = QApplication(); window = QMainWindow()`  
b) `app = QApplication(sys.argv); window = QMainWindow()`  
c) `app = PySide6.Application(); window = PySide6.MainWindow()`  
d) `app = Qt.Application(); window = Qt.MainWindow()`  

**Reponse correcte :** b) `app = QApplication(sys.argv); window = QMainWindow()`

---

### Question 3
Qu'est-ce qu'un signal dans PySide6 ?

a) Une variable globale  
b) Une fonction callback  
c) Un mecanisme de communication entre objets  
d) Une classe de base  

**Reponse correcte :** c) Un mecanisme de communication entre objets

---

### Question 4
Comment connecter un signal a un slot ?

a) `signal.connect(slot)`  
b) `signal >> slot`  
c) `connect(signal, slot)`  
d) `signal.bind(slot)`  

**Reponse correcte :** a) `signal.connect(slot)`

---

### Question 5
Quelle est la syntaxe correcte pour creer un signal personnalise ?

a) `mon_signal = Signal()`  
b) `mon_signal = QtCore.Signal()`  
c) `mon_signal = QSignal()`  
d) `mon_signal = pyqtSignal()`  

**Reponse correcte :** b) `mon_signal = QtCore.Signal()` (ou `from PySide6.QtCore import Signal`)

---

### Question 6
Quel layout est le plus approprie pour creer une grille de widgets ?

a) QVBoxLayout  
b) QHBoxLayout  
c) QGridLayout  
d) QFormLayout  

**Reponse correcte :** c) QGridLayout

---

### Question 7
Comment obtenir le texte d'un QLineEdit ?

a) `line_edit.text`  
b) `line_edit.getText()`  
c) `line_edit.text()`  
d) `line_edit.value()`  

**Reponse correcte :** c) `line_edit.text()`

---

### Question 8
Quelle est la methode correcte pour definir le texte d'un QLabel ?

a) `label.text = "Nouveau texte"`  
b) `label.setText("Nouveau texte")`  
c) `label.setContent("Nouveau texte")`  
d) `label.content("Nouveau texte")`  

**Reponse correcte :** b) `label.setText("Nouveau texte")`

---

### Question 9
Comment creer un QMessageBox d'information ?

a) `QMessageBox.info(parent, "Titre", "Message")`  
b) `QMessageBox.information(parent, "Titre", "Message")`  
c) `QMessageBox.showInfo(parent, "Titre", "Message")`  
d) Les reponses a et b sont correctes  

**Reponse correcte :** d) Les reponses a et b sont correctes

---

### Question 10
Qu'est-ce que Qt Designer ?

a) Un editeur de code pour PySide6  
b) Un outil visuel pour creer des interfaces graphiques  
c) Un debugger pour les applications Qt  
d) Un compilateur pour les fichiers .ui  

**Reponse correcte :** b) Un outil visuel pour creer des interfaces graphiques

---

## Questions Ouvertes

### Question 11
Expliquez le concept de signaux et slots dans PySide6. Donnez un exemple concret.

**Reponse attendue :**
Les signaux et slots constituent le mecanisme de communication entre objets dans PySide6 :

- **Signal** : Emis quand quelque chose d'interessant se produit (clic de bouton, changement de texte, etc.)
- **Slot** : Fonction qui peut etre connectee a un signal pour reagir a l'evenement

**Avantages :**
- Couplage faible entre objets
- Communication asynchrone
- Un signal peut etre connecte a plusieurs slots
- Un slot peut recevoir plusieurs signaux

**Exemple :**
```python
from PySide6.QtWidgets import QApplication, QMainWindow, QPushButton, QLabel
from PySide6.QtCore import Signal
import sys

class MaFenetre(QMainWindow):
    # Signal personnalise
    message_envoye = Signal(str)
    
    def __init__(self):
        super().__init__()
        
        # Widgets
        self.bouton = QPushButton("Cliquez-moi")
        self.label = QLabel("Pas encore clique")
        
        # Connexions signal/slot
        self.bouton.clicked.connect(self.bouton_clique)  # Signal predefinit
        self.message_envoye.connect(self.afficher_message)  # Signal personnalise
    
    def bouton_clique(self):
        """Slot pour le clic de bouton"""
        self.message_envoye.emit("Bouton clique!")
    
    def afficher_message(self, message):
        """Slot pour afficher un message"""
        self.label.setText(message)
```

---

### Question 12
Quelles sont les differences entre les differents layouts dans PySide6 ? Quand utiliser chacun ?

**Reponse attendue :**

**QVBoxLayout** - Disposition verticale
- Widgets empiles verticalement
- Utilisation : barres d'outils, listes d'elements

**QHBoxLayout** - Disposition horizontale
- Widgets alignes horizontalement
- Utilisation : boutons d'action, barres de statut

**QGridLayout** - Disposition en grille
- Widgets places dans une grille (lignes/colonnes)
- Utilisation : formulaires complexes, calculatrices

**QFormLayout** - Disposition de formulaire
- Paires label/widget automatiquement organisees
- Utilisation : formulaires de saisie

**QStackedLayout** - Disposition empilee
- Un seul widget visible a la fois
- Utilisation : onglets, assistants

**Exemple de QGridLayout :**
```python
layout = QGridLayout()
layout.addWidget(QLabel("Nom:"), 0, 0)
layout.addWidget(QLineEdit(), 0, 1)
layout.addWidget(QLabel("Email:"), 1, 0)
layout.addWidget(QLineEdit(), 1, 1)
layout.addWidget(QPushButton("Valider"), 2, 0, 1, 2)  # span 2 colonnes
```

---

### Question 13
Comment gerer les evenements clavier et souris dans PySide6 ?

**Reponse attendue :**

**Methode 1 - Redefinir les methodes d'evenements :**
```python
class MaFenetre(QMainWindow):
    def keyPressEvent(self, event):
        if event.key() == Qt.Key_Escape:
            self.close()
        elif event.key() == Qt.Key_Return:
            self.valider_formulaire()
        super().keyPressEvent(event)
    
    def mousePressEvent(self, event):
        if event.button() == Qt.LeftButton:
            print(f"Clic gauche en ({event.x()}, {event.y()})")
        super().mousePressEvent(event)
```

**Methode 2 - Utiliser les signaux predefinits :**
```python
# Pour les widgets specifiques
line_edit = QLineEdit()
line_edit.returnPressed.connect(self.valider_saisie)
line_edit.textChanged.connect(self.texte_modifie)

button = QPushButton()
button.clicked.connect(self.bouton_clique)
```

**Methode 3 - Installer un filtre d'evenements :**
```python
class MonFiltre(QObject):
    def eventFilter(self, obj, event):
        if event.type() == QEvent.KeyPress:
            if event.key() == Qt.Key_F1:
                self.afficher_aide()
                return True
        return super().eventFilter(obj, event)

# Installation du filtre
filtre = MonFiltre()
widget.installEventFilter(filtre)
```

---

### Question 14
Expliquez comment utiliser QThread pour les operations longues. Pourquoi est-ce important ?

**Reponse attendue :**

**Pourquoi utiliser QThread :**
- Eviter le blocage de l'interface utilisateur
- Maintenir la responsivite de l'application
- Permettre les operations en arriere-plan

**Probleme sans QThread :**
```python
# MAUVAIS - Bloque l'interface
def operation_longue(self):
    for i in range(1000000):
        # Calcul complexe
        result = math.sqrt(i)
    self.afficher_resultat(result)
```

**Solution avec QThread :**
```python
from PySide6.QtCore import QThread, Signal

class TravailleurThread(QThread):
    # Signaux pour communiquer avec l'interface
    progres_change = Signal(int)
    resultat_pret = Signal(object)
    
    def run(self):
        """Execute dans un thread separe"""
        for i in range(1000000):
            if i % 10000 == 0:
                self.progres_change.emit(i // 10000)
            result = math.sqrt(i)
        
        self.resultat_pret.emit(result)

class MaFenetre(QMainWindow):
    def __init__(self):
        super().__init__()
        self.thread_travailleur = None
    
    def demarrer_operation(self):
        self.thread_travailleur = TravailleurThread()
        
        # Connecter les signaux
        self.thread_travailleur.progres_change.connect(self.mettre_a_jour_progres)
        self.thread_travailleur.resultat_pret.connect(self.afficher_resultat)
        self.thread_travailleur.finished.connect(self.operation_terminee)
        
        # Demarrer le thread
        self.thread_travailleur.start()
    
    def mettre_a_jour_progres(self, valeur):
        self.progress_bar.setValue(valeur)
    
    def afficher_resultat(self, resultat):
        self.label_resultat.setText(f"Resultat: {resultat}")
```

**Regles importantes :**
- Ne jamais modifier l'interface depuis un thread
- Utiliser les signaux pour communiquer
- Nettoyer les threads correctement

---

### Question 15
Comment implementer un systeme de validation en temps reel dans un formulaire PySide6 ?

**Reponse attendue :**

**Approche complete avec validation en temps reel :**

```python
from PySide6.QtWidgets import *
from PySide6.QtCore import *
from PySide6.QtGui import *
import re

class ValidateurEmail(QValidator):
    """Validateur personnalise pour les emails"""
    
    def validate(self, input_str, pos):
        if not input_str:
            return QValidator.Intermediate, input_str, pos
        
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        if re.match(pattern, input_str):
            return QValidator.Acceptable, input_str, pos
        elif '@' in input_str:
            return QValidator.Intermediate, input_str, pos
        else:
            return QValidator.Invalid, input_str, pos

class FormulaireAvecValidation(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Formulaire avec Validation")
        self.setupUI()
        self.connecter_validations()
    
    def setupUI(self):
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        layout = QFormLayout(central_widget)
        
        # Champ nom
        self.line_edit_nom = QLineEdit()
        self.label_erreur_nom = QLabel()
        self.label_erreur_nom.setStyleSheet("color: red; font-size: 10px;")
        layout.addRow("Nom:", self.line_edit_nom)
        layout.addRow("", self.label_erreur_nom)
        
        # Champ email
        self.line_edit_email = QLineEdit()
        self.line_edit_email.setValidator(ValidateurEmail())
        self.label_erreur_email = QLabel()
        self.label_erreur_email.setStyleSheet("color: red; font-size: 10px;")
        layout.addRow("Email:", self.line_edit_email)
        layout.addRow("", self.label_erreur_email)
        
        # Champ age
        self.spin_box_age = QSpinBox()
        self.spin_box_age.setRange(0, 120)
        layout.addRow("Age:", self.spin_box_age)
        
        # Bouton de soumission
        self.bouton_soumettre = QPushButton("Soumettre")
        self.bouton_soumettre.setEnabled(False)
        layout.addRow(self.bouton_soumettre)
    
    def connecter_validations(self):
        # Validation en temps reel
        self.line_edit_nom.textChanged.connect(self.valider_nom)
        self.line_edit_email.textChanged.connect(self.valider_email)
        
        # Validation globale
        self.line_edit_nom.textChanged.connect(self.valider_formulaire)
        self.line_edit_email.textChanged.connect(self.valider_formulaire)
        self.spin_box_age.valueChanged.connect(self.valider_formulaire)
        
        # Soumission
        self.bouton_soumettre.clicked.connect(self.soumettre_formulaire)
    
    def valider_nom(self, texte):
        if len(texte.strip()) < 2:
            self.label_erreur_nom.setText("Le nom doit contenir au moins 2 caracteres")
            self.line_edit_nom.setStyleSheet("border: 2px solid red;")
            return False
        else:
            self.label_erreur_nom.setText("")
            self.line_edit_nom.setStyleSheet("border: 2px solid green;")
            return True
    
    def valider_email(self, texte):
        validator = self.line_edit_email.validator()
        state, _, _ = validator.validate(texte, 0)
        
        if state == QValidator.Acceptable:
            self.label_erreur_email.setText("")
            self.line_edit_email.setStyleSheet("border: 2px solid green;")
            return True
        elif state == QValidator.Intermediate:
            self.label_erreur_email.setText("Email incomplet")
            self.line_edit_email.setStyleSheet("border: 2px solid orange;")
            return False
        else:
            self.label_erreur_email.setText("Format d'email invalide")
            self.line_edit_email.setStyleSheet("border: 2px solid red;")
            return False
    
    def valider_formulaire(self):
        nom_valide = self.valider_nom(self.line_edit_nom.text())
        email_valide = self.valider_email(self.line_edit_email.text())
        age_valide = self.spin_box_age.value() > 0
        
        self.bouton_soumettre.setEnabled(nom_valide and email_valide and age_valide)
    
    def soumettre_formulaire(self):
        # Traitement final
        QMessageBox.information(self, "Succes", "Formulaire soumis avec succes!")
```

**Elements cles :**
- Validateurs personnalises
- Validation en temps reel avec signaux
- Feedback visuel immediat
- Activation/desactivation du bouton selon la validite

---

## Questions de Code

### Question 16
Corrigez les erreurs dans ce code PySide6 :

```python
import PySide6
from PySide6.QtWidgets import QApplication, QMainWindow, QPushButton

class MaFenetre(QMainWindow):
    def __init__(self):
        self.setWindowTitle("Ma Fenetre")
        
        bouton = QPushButton("Cliquer")
        bouton.clicked.connect(self.bouton_clique())
        
        self.setCentralWidget(bouton)
    
    def bouton_clique(self):
        print("Bouton clique!")

app = QApplication()
fenetre = MaFenetre()
fenetre.show()
app.exec()
```

**Reponse correcte :**
```python
import sys
from PySide6.QtWidgets import QApplication, QMainWindow, QPushButton

class MaFenetre(QMainWindow):
    def __init__(self):
        super().__init__()  # Appel du constructeur parent
        self.setWindowTitle("Ma Fenetre")
        
        bouton = QPushButton("Cliquer")
        bouton.clicked.connect(self.bouton_clique)  # Sans parentheses
        
        self.setCentralWidget(bouton)
    
    def bouton_clique(self):
        print("Bouton clique!")

app = QApplication(sys.argv)  # Passer sys.argv
fenetre = MaFenetre()
fenetre.show()
sys.exit(app.exec())  # Utiliser sys.exit pour une fermeture propre
```

**Erreurs corrigees :**
1. Ajout de `import sys`
2. Ajout de `super().__init__()` dans le constructeur
3. `QApplication(sys.argv)` au lieu de `QApplication()`
4. `bouton.clicked.connect(self.bouton_clique)` sans parentheses
5. `sys.exit(app.exec())` pour une fermeture propre

---

### Question 17
Implementez une classe qui cree une fenetre avec un QTableWidget affichant des donnees et permettant l'ajout/suppression de lignes.

**Reponse attendue :**
```python
import sys
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QTableWidget, QTableWidgetItem,
                              QPushButton, QInputDialog, QMessageBox, QHeaderView)
from PySide6.QtCore import Qt

class GestionnaireTableau(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Gestionnaire de Tableau")
        self.setGeometry(100, 100, 600, 400)
        
        # Donnees initiales
        self.donnees = [
            ["Alice", "25", "Developpeur"],
            ["Bob", "30", "Designer"],
            ["Charlie", "28", "Manager"]
        ]
        
        self.setupUI()
        self.charger_donnees()
    
    def setupUI(self):
        # Widget central
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # Layout principal
        layout = QVBoxLayout(central_widget)
        
        # Tableau
        self.table = QTableWidget()
        self.table.setColumnCount(3)
        self.table.setHorizontalHeaderLabels(["Nom", "Age", "Poste"])
        
        # Ajuster la largeur des colonnes
        header = self.table.horizontalHeader()
        header.setSectionResizeMode(QHeaderView.Stretch)
        
        layout.addWidget(self.table)
        
        # Boutons
        boutons_layout = QHBoxLayout()
        
        self.btn_ajouter = QPushButton("Ajouter Ligne")
        self.btn_supprimer = QPushButton("Supprimer Ligne")
        self.btn_modifier = QPushButton("Modifier Ligne")
        
        boutons_layout.addWidget(self.btn_ajouter)
        boutons_layout.addWidget(self.btn_supprimer)
        boutons_layout.addWidget(self.btn_modifier)
        boutons_layout.addStretch()
        
        layout.addLayout(boutons_layout)
        
        # Connecter les signaux
        self.btn_ajouter.clicked.connect(self.ajouter_ligne)
        self.btn_supprimer.clicked.connect(self.supprimer_ligne)
        self.btn_modifier.clicked.connect(self.modifier_ligne)
        
        # Signal de selection
        self.table.itemSelectionChanged.connect(self.selection_changee)
        
        # Desactiver le bouton supprimer au debut
        self.btn_supprimer.setEnabled(False)
        self.btn_modifier.setEnabled(False)
    
    def charger_donnees(self):
        """Charge les donnees dans le tableau"""
        self.table.setRowCount(len(self.donnees))
        
        for row, donnee in enumerate(self.donnees):
            for col, valeur in enumerate(donnee):
                item = QTableWidgetItem(str(valeur))
                self.table.setItem(row, col, item)
    
    def ajouter_ligne(self):
        """Ajoute une nouvelle ligne"""
        # Dialogues pour saisir les donnees
        nom, ok1 = QInputDialog.getText(self, "Nouveau", "Nom:")
        if not ok1 or not nom.strip():
            return
        
        age, ok2 = QInputDialog.getText(self, "Nouveau", "Age:")
        if not ok2 or not age.strip():
            return
        
        # Validation de l'age
        try:
            int(age)
        except ValueError:
            QMessageBox.warning(self, "Erreur", "L'age doit etre un nombre")
            return
        
        poste, ok3 = QInputDialog.getText(self, "Nouveau", "Poste:")
        if not ok3 or not poste.strip():
            return
        
        # Ajouter aux donnees
        self.donnees.append([nom, age, poste])
        
        # Ajouter au tableau
        row = self.table.rowCount()
        self.table.insertRow(row)
        self.table.setItem(row, 0, QTableWidgetItem(nom))
        self.table.setItem(row, 1, QTableWidgetItem(age))
        self.table.setItem(row, 2, QTableWidgetItem(poste))
        
        # Selectionner la nouvelle ligne
        self.table.selectRow(row)
    
    def supprimer_ligne(self):
        """Supprime la ligne selectionnee"""
        ligne_actuelle = self.table.currentRow()
        
        if ligne_actuelle >= 0:
            # Confirmation
            reponse = QMessageBox.question(
                self, "Confirmation",
                "Etes-vous sur de vouloir supprimer cette ligne?",
                QMessageBox.Yes | QMessageBox.No
            )
            
            if reponse == QMessageBox.Yes:
                # Supprimer des donnees
                del self.donnees[ligne_actuelle]
                
                # Supprimer du tableau
                self.table.removeRow(ligne_actuelle)
    
    def modifier_ligne(self):
        """Modifie la ligne selectionnee"""
        ligne_actuelle = self.table.currentRow()
        
        if ligne_actuelle >= 0:
            donnee_actuelle = self.donnees[ligne_actuelle]
            
            # Dialogues avec valeurs actuelles
            nom, ok1 = QInputDialog.getText(
                self, "Modifier", "Nom:", text=donnee_actuelle[0]
            )
            if not ok1:
                return
            
            age, ok2 = QInputDialog.getText(
                self, "Modifier", "Age:", text=donnee_actuelle[1]
            )
            if not ok2:
                return
            
            # Validation de l'age
            try:
                int(age)
            except ValueError:
                QMessageBox.warning(self, "Erreur", "L'age doit etre un nombre")
                return
            
            poste, ok3 = QInputDialog.getText(
                self, "Modifier", "Poste:", text=donnee_actuelle[2]
            )
            if not ok3:
                return
            
            # Mettre a jour les donnees
            self.donnees[ligne_actuelle] = [nom, age, poste]
            
            # Mettre a jour le tableau
            self.table.setItem(ligne_actuelle, 0, QTableWidgetItem(nom))
            self.table.setItem(ligne_actuelle, 1, QTableWidgetItem(age))
            self.table.setItem(ligne_actuelle, 2, QTableWidgetItem(poste))
    
    def selection_changee(self):
        """Gere le changement de selection"""
        a_selection = self.table.currentRow() >= 0
        self.btn_supprimer.setEnabled(a_selection)
        self.btn_modifier.setEnabled(a_selection)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    fenetre = GestionnaireTableau()
    fenetre.show()
    
    sys.exit(app.exec())
```

---

## Bareme de Correction

| Section | Points | Details |
|---------|--------|---------|
| QCM (Questions 1-10) | 20 points | 2 points par question |
| Questions ouvertes (11-15) | 40 points | 8 points par question |
| Questions de code (16-17) | 40 points | 20 points par question |
| **Total** | **100 points** | |

### Criteres d'evaluation :
- **Exactitude technique** : La reponse est-elle techniquement correcte ?
- **Completude** : Tous les aspects sont-ils couverts ?
- **Bonnes pratiques** : Respect des conventions PySide6/Qt
- **Gestion d'erreurs** : Prise en compte des cas d'erreur
- **Clarte du code** : Code lisible et bien structure
- **Fonctionnalite** : Le code fonctionne-t-il correctement ?

### Notes de passage :
- **Excellent** : 90-100 points (90-100%)
- **Bien** : 80-89 points (80-89%)
- **Satisfaisant** : 70-79 points (70-79%)
- **Insuffisant** : < 70 points (< 70%)

Bonne chance pour votre quiz PySide6 !
