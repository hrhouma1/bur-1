

## 

<h1 id="table-des-matières">Table des matières – Introduction à PyQt5</h1>

<ul>
  <li><a href="#introduction-a-pyqt5">1. Introduction à PyQt5</a></li>
  <li><a href="#preparation-de-lenvironnement-sous-windows">2. Préparation de l’environnement sous Windows</a></li>
  <li><a href="#creation-dune-fenetre-pyqt5">3. Création d'une fenêtre PyQt5</a></li>
  <li><a href="#ajout-dun-label-dans-la-fenetre">4. Ajout d'un Label dans la fenêtre</a></li>
  <li><a href="#bouton-avec-action-signal-slot">5. Bouton avec action (signal / slot)</a></li>
  <li><a href="#champ-de-saisie-qlineedit-et-recuperation-du-texte">6. Champ de saisie (QLineEdit) et récupération du texte</a></li>
  <li><a href="#organisation-des-composants-avec-les-layouts">7. Organisation des composants avec les Layouts</a></li>
  <li><a href="#qradiobutton-boutons-radio">8. QRadioButton (boutons radio)</a></li>
  <li><a href="#qcheckbox-cases-a-cocher">9. QCheckBox (cases à cocher)</a></li>
  <li><a href="#qcombobox-menu-deroulant">10. QComboBox (menu déroulant)</a></li>
  <li><a href="#qlistwidget-liste-avec-selection">11. QListWidget (liste avec sélection)</a></li>
  <li><a href="#structure-orientee-objet-avec-qmainwindow">12. Structure orientée objet avec QMainWindow</a></li>
  <li><a href="#interface-graphique-avec-qt-designer">13. Interface graphique avec Qt Designer</a></li>
  <li><a href="#charger-un-fichier-ui-avec-uicloadui">14. Charger un fichier .ui avec uic.loadUi()</a></li>
  <li><a href="#exercices-pratiques-pyqt5-a-venir">15. Exercices pratiques PyQt5 (à venir)</a></li>
</ul>

















<h1 id="introduction-a-pyqt5">1. Introduction à PyQt5</h1>

**PyQt5** est un binding Python de la bibliothèque C++ **Qt5**, utilisée pour développer des interfaces graphiques multiplateformes (Windows, macOS, Linux).  
Elle permet de concevoir des interfaces modernes, réactives et robustes.

PyQt5 est très utilisé dans l’industrie et supporte des composants avancés (boîtes de dialogue, menus, layouts, modèles MVC, etc.).

[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>

<h1 id="preparation-de-lenvironnement-sous-windows"> 2. Préparation de l’environnement sous Windows </h1>

Avant de commencer, installe et configure ton environnement virtuel :

```bash
# Étapes sous Windows (cmd ou PowerShell)

# 1. Créer un dossier pour le projet
mkdir pyqt5_projets
cd pyqt5_projets

# 2. Créer un environnement virtuel
python -m venv venv

# 3. Activer l’environnement
venv\Scripts\activate

# 4. Installer PyQt5
pip install PyQt5
```


[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="creation-dune-fenetre-pyqt5"> 3. Création d'une fenêtre PyQt5 </h1>

### Exemple minimal :

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget

# Création de l'application Qt
app = QApplication(sys.argv)

# Création d'une fenêtre
fenetre = QWidget()
fenetre.setWindowTitle("Ma première fenêtre PyQt5")
fenetre.resize(300, 100)  # Largeur x Hauteur

# Affichage de la fenêtre
fenetre.show()

# Boucle principale
sys.exit(app.exec_())
```






[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="ajout-dun-label-dans-la-fenetre"> 4. Ajout d'un Label dans la fenêtre </h1>


## 4. Ajout d'un Label dans la fenêtre

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QLabel

app = QApplication(sys.argv)

fenetre = QWidget()
fenetre.setWindowTitle("Label simple")
fenetre.resize(300, 120)

# Création d'un label avec un texte
label = QLabel("Bonjour avec PyQt5 !", parent=fenetre)
label.move(80, 50)  # Position dans la fenêtre

fenetre.show()
sys.exit(app.exec_())
```








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="bouton-avec-action-signal-slot"> 5. Bouton avec action (signal / slot) </h1>


## 5. Bouton avec action (signal / slot)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton

def clic():
    print("Bouton cliqué")

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("Bouton")
fenetre.resize(300, 100)

# Création du bouton
btn = QPushButton("Clique ici", parent=fenetre)
btn.move(100, 40)

# Connexion du signal au slot
btn.clicked.connect(clic)

fenetre.show()
sys.exit(app.exec_())
```








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="champ-de-saisie-qlineedit-et-recuperation-du-texte"> 6. Champ de saisie (QLineEdit) et récupération du texte </h1>

## 6. Champ de saisie (QLineEdit) et récupération du texte

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QLineEdit, QLabel, QPushButton

def afficher():
    texte = champ.text()
    label.setText(f"Bonjour, {texte}")

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("Champ de saisie")
fenetre.resize(300, 150)

champ = QLineEdit(parent=fenetre)
champ.move(80, 20)

label = QLabel("", parent=fenetre)
label.move(80, 60)

btn = QPushButton("Valider", parent=fenetre)
btn.move(80, 100)
btn.clicked.connect(afficher)

fenetre.show()
sys.exit(app.exec_())
```









[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="organisation-des-composants-avec-les-layouts"> 7. Organisation des composants avec les Layouts </h1>


## 7. Organisation des composants avec les Layouts

Qt recommande de ne pas utiliser `.move(x, y)` pour positionner les éléments.  
À la place, on utilise des **layouts** (dispositions automatiques).

### a) Vertical layout (QVBoxLayout)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QVBoxLayout")

layout = QVBoxLayout()

label = QLabel("Texte au-dessus")
btn = QPushButton("Bouton en dessous")

layout.addWidget(label)
layout.addWidget(btn)

fenetre.setLayout(layout)
fenetre.resize(300, 100)
fenetre.show()
sys.exit(app.exec_())
```

### b) Horizontal layout (QHBoxLayout)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QHBoxLayout, QLabel, QPushButton

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QHBoxLayout")

layout = QHBoxLayout()

label = QLabel("À gauche")
btn = QPushButton("À droite")

layout.addWidget(label)
layout.addWidget(btn)

fenetre.setLayout(layout)
fenetre.resize(300, 100)
fenetre.show()
sys.exit(app.exec_())
```









[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="qradiobutton-boutons-radio"> 8. QRadioButton (boutons radio) </h1>


## 8. QRadioButton (boutons radio)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QRadioButton, QPushButton

def afficher():
    if rb1.isChecked():
        print("Choix : Homme")
    elif rb2.isChecked():
        print("Choix : Femme")
    else:
        print("Choix : Autre")

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QRadioButton")

layout = QVBoxLayout()

rb1 = QRadioButton("Homme")
rb2 = QRadioButton("Femme")
rb3 = QRadioButton("Autre")

btn = QPushButton("Valider")
btn.clicked.connect(afficher)

layout.addWidget(rb1)
layout.addWidget(rb2)
layout.addWidget(rb3)
layout.addWidget(btn)

fenetre.setLayout(layout)
fenetre.show()
sys.exit(app.exec_())
```







[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="qcheckbox-cases-a-cocher"> 9. QCheckBox (cases à cocher) </h1>


## 9. QCheckBox (cases à cocher)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QCheckBox, QPushButton

def afficher():
    print("Langages sélectionnés :")
    if cb1.isChecked(): print("- Python")
    if cb2.isChecked(): print("- Java")
    if cb3.isChecked(): print("- C++")

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QCheckBox")

layout = QVBoxLayout()

cb1 = QCheckBox("Python")
cb2 = QCheckBox("Java")
cb3 = QCheckBox("C++")

btn = QPushButton("Afficher")
btn.clicked.connect(afficher)

layout.addWidget(cb1)
layout.addWidget(cb2)
layout.addWidget(cb3)
layout.addWidget(btn)

fenetre.setLayout(layout)
fenetre.show()
sys.exit(app.exec_())
```








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="qcombobox-menu-deroulant"> 10. QComboBox (menu déroulant) </h1>


## 10. QComboBox (menu déroulant)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QComboBox, QLabel

def afficher():
    label.setText("Couleur : " + combo.currentText())

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QComboBox")

layout = QVBoxLayout()

combo = QComboBox()
combo.addItems(["Rouge", "Vert", "Bleu"])
combo.currentIndexChanged.connect(afficher)

label = QLabel("")

layout.addWidget(combo)
layout.addWidget(label)

fenetre.setLayout(layout)
fenetre.show()
sys.exit(app.exec_())
```







[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="qlistwidget-liste-avec-selection"> 11. QListWidget (liste avec sélection) </h1>


## 11. QListWidget (liste avec sélection)

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QListWidget, QPushButton

def afficher():
    items = listbox.selectedItems()
    print("Sélection :")
    for item in items:
        print("-", item.text())

app = QApplication(sys.argv)
fenetre = QWidget()
fenetre.setWindowTitle("QListWidget")

layout = QVBoxLayout()

listbox = QListWidget()
listbox.addItems(["Pomme", "Banane", "Cerise", "Orange"])
listbox.setSelectionMode(listbox.MultiSelection)

btn = QPushButton("Afficher sélection")
btn.clicked.connect(afficher)

layout.addWidget(listbox)
layout.addWidget(btn)

fenetre.setLayout(layout)
fenetre.show()
sys.exit(app.exec_())
```








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="structure-orientee-objet-avec-qmainwindow"> 12. Structure orientée objet avec QMainWindow </h1>


## 12. Structure orientée objet avec QMainWindow

Jusqu’à maintenant, nous avons utilisé des fonctions simples et `QWidget`.  
Pour structurer un projet réel, on crée une **classe personnalisée** héritant de `QMainWindow`.

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel

class MaFenetre(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Fenêtre orientée objet")
        self.setGeometry(100, 100, 300, 150)

        self.label = QLabel("Bonjour dans une classe QMainWindow", self)
        self.label.move(50, 60)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    fenetre = MaFenetre()
    fenetre.show()
    sys.exit(app.exec_())
```






[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="interface-graphique-avec-qt-designer"> 13. Interface graphique avec Qt Designer </h1>


## 13. Interface graphique avec Qt Designer

Qt Designer est un outil visuel qui permet de créer des interfaces graphiques en glissant-déposant des composants.

### a) Lancer Qt Designer (si installé avec `pyqt5-tools`) :

```bash
# Si pyqt5-tools est installé :
pyqt5-tools designer
```

### b) Étapes dans Qt Designer :

1. Créer une nouvelle fenêtre (type : "Main Window").
2. Ajouter des composants (boutons, labels, zones de texte, etc.).
3. Sauvegarder sous un nom, par exemple `interface.ui`.

> Ce fichier `.ui` est un fichier XML qui décrit la structure graphique.








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="charger-un-fichier-ui-avec-uicloadui"> 14. Charger un fichier .ui avec uic.loadUi() </h1>


## 14. Charger un fichier `.ui` avec `uic.loadUi()`

PyQt permet de charger un fichier `.ui` sans le convertir en `.py`.

### Exemple :

```python
import sys
from PyQt5 import uic
from PyQt5.QtWidgets import QApplication, QMainWindow

class FenetreAvecUI(QMainWindow):
    def __init__(self):
        super().__init__()
        uic.loadUi("interface.ui", self)  # Charger le fichier UI

        # Exemples d'accès à des composants (assurez-vous d'avoir mis des noms dans Qt Designer)
        self.pushButton.clicked.connect(self.afficher_texte)

    def afficher_texte(self):
        texte = self.lineEdit.text()
        self.label.setText("Bonjour " + texte)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    fenetre = FenetreAvecUI()
    fenetre.show()
    sys.exit(app.exec_())
```

### Remarques :
- Tous les composants doivent avoir un **nom (`objectName`)** dans Qt Designer.
- Les attributs deviennent accessibles par `self.nomDuWidget` dans le code.





[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercices-pratiques-pyqt5-a-venir"> 15. Exercices pratiques PyQt5 (à venir) </h1>


## 15. Exercices pratiques PyQt5 (à venir)


1. Créer une fenêtre avec QMainWindow  
2. Ajouter un bouton qui modifie un label  
3. Créer un formulaire avec validation  
4. Interface Designer + slot personnalisé  
5. Mini-projet : calculatrice simple  
6. Mini-projet : formulaire de contact  
7. TP final : interface de gestion de produits

