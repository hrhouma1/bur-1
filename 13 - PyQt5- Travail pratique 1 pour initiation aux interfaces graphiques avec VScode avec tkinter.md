
# PyQt5 - Travail pratique 1 pour initiation aux interfaces graphiques

##  Table des matières


<ul>
  <li><a href="#exercice-1--creer-une-fenetre-avec-qmainwindow">Exercice 1 – Créer une fenêtre avec QMainWindow</a></li>
  <li><a href="#exercice-2--ajouter-un-bouton-qui-modifie-un-label">Exercice 2 – Ajouter un bouton qui modifie un label</a></li>
  <li><a href="#exercice-3--creer-un-mini-formulaire-avec-validation">Exercice 3 – Créer un mini formulaire avec validation</a></li>
  <li><a href="#exercice-4--interface-designer--interaction">Exercice 4 – Interface Designer + interaction</a></li>
  <li><a href="#exercice-5--mini-projet--calculatrice-simple-addition">Exercice 5 – Mini-projet : calculatrice simple (addition)</a></li>
  <li><a href="#exercice-6--mini-projet--formulaire-de-contact">Exercice 6 – Mini-projet : formulaire de contact</a></li>
  <li><a href="#tp-final--interface-de-gestion-de-produits">TP Final – Interface de Gestion de Produits</a></li>
</ul>








<br/>
<br/>
<h1 id="exercice-1--creer-une-fenetre-avec-qmainwindow"> 1. Créer une fenêtre avec QMainWindow </h1>


### Énoncé de l’exercice :

Créer une fenêtre PyQt5 avec une structure orientée objet basée sur `QMainWindow`.  
Définir un titre et une taille de 400x200 pixels.


### Correction

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow

class MaFenetre(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exercice 1 – Fenêtre simple")
        self.setGeometry(100, 100, 400, 200)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    fenetre = MaFenetre()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Création d’une classe héritée de `QMainWindow`.
2. Définition du titre de la fenêtre avec `.setWindowTitle()`.
3. Définition des dimensions et de la position avec `.setGeometry()`.












[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercice-2--ajouter-un-bouton-qui-modifie-un-label"> 2. Ajouter un bouton qui modifie un label </h1>


### Énoncé de l’exercice :

Ajouter un bouton et une étiquette dans une fenêtre.  
Lorsqu’on clique sur le bouton, le texte du label doit être modifié dynamiquement.

### Correction

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QPushButton, QLabel

class FenetreBouton(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exercice 2 – Bouton et Label")
        self.setGeometry(100, 100, 400, 200)

        self.label = QLabel("Texte initial", self)
        self.label.move(150, 60)

        self.bouton = QPushButton("Changer le texte", self)
        self.bouton.move(130, 100)
        self.bouton.clicked.connect(self.changer_texte)

    def changer_texte(self):
        self.label.setText("Texte modifié !")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    fenetre = FenetreBouton()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Un `QLabel` positionné dans la fenêtre.
2. Un `QPushButton` avec une action connectée.
3. Une méthode `changer_texte()` qui modifie dynamiquement le texte du label.








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercice-3--creer-un-mini-formulaire-avec-validation"> 3. Créer un mini formulaire avec validation </h1>


### Énoncé de l’exercice :

Créer un petit formulaire avec :
- un champ pour entrer le nom,
- un bouton "Valider",
- un message qui s’affiche sous le bouton si le champ est vide ou affiche "Bonjour <nom>" si le champ est rempli.



### Correction

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QPushButton, QLineEdit

class Formulaire(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exercice 3 – Formulaire simple")
        self.setGeometry(100, 100, 400, 200)

        self.label_nom = QLabel("Votre nom :", self)
        self.label_nom.move(50, 40)

        self.champ_nom = QLineEdit(self)
        self.champ_nom.move(150, 40)

        self.label_resultat = QLabel("", self)
        self.label_resultat.move(50, 130)

        self.btn_valider = QPushButton("Valider", self)
        self.btn_valider.move(150, 80)
        self.btn_valider.clicked.connect(self.afficher_nom)

    def afficher_nom(self):
        texte = self.champ_nom.text()
        if texte.strip():
            self.label_resultat.setText("Bonjour " + texte)
        else:
            self.label_resultat.setText("Veuillez entrer un nom.")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    fenetre = Formulaire()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Un `QLineEdit` pour la saisie du nom.
2. Un bouton qui déclenche une fonction de validation.
3. Un `QLabel` affichant un message personnalisé ou une erreur si le champ est vide.








[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercice-4--interface-designer--interaction"> 4. Interface Designer + interaction </h1>


### Prérequis

Créer une interface avec **Qt Designer** contenant :
- un `QLineEdit` (nommé `lineEdit`)
- un `QPushButton` (nommé `pushButton`)
- un `QLabel` (nommé `label`)

Sauvegarder le fichier sous le nom : `interface.ui`


### Énoncé de l’exercice :

Avec Qt Designer, créer une interface contenant :
- une zone de texte (`QLineEdit`)
- un bouton
- un label pour l’affichage

Dans le script Python, charger l’interface `.ui` et afficher "Bonjour <nom>" dans le label au clic du bouton.

### Correction (Python)

```python
import sys
from PyQt5 import uic
from PyQt5.QtWidgets import QApplication, QMainWindow

class FenetreDesigner(QMainWindow):
    def __init__(self):
        super().__init__()
        uic.loadUi("interface.ui", self)

        self.pushButton.clicked.connect(self.afficher)

    def afficher(self):
        nom = self.lineEdit.text()
        if nom.strip():
            self.label.setText(f"Bonjour {nom}")
        else:
            self.label.setText("Champ vide")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    fenetre = FenetreDesigner()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Chargement d’une interface `.ui` générée par Qt Designer via `uic.loadUi(...)`.
2. Accès aux composants par leur nom (`lineEdit`, `label`, `pushButton`).
3. Liaison entre le bouton et une méthode personnalisée pour interagir avec les champs.






[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercice-5--mini-projet--calculatrice-simple-addition"> 5. Mini-projet : calculatrice simple (addition) </h1>

### Énoncé de l’exercice :

Créer une interface avec deux zones de saisie pour des nombres, un bouton "Calculer", et un label qui affiche la somme.  
Afficher un message d’erreur si l’une des deux entrées n’est pas un nombre.


### Correction

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QPushButton, QLineEdit

class Calculatrice(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exercice 5 – Addition")
        self.setGeometry(100, 100, 400, 200)

        self.entree1 = QLineEdit(self)
        self.entree1.move(50, 30)

        self.entree2 = QLineEdit(self)
        self.entree2.move(200, 30)

        self.bouton = QPushButton("Calculer", self)
        self.bouton.move(150, 70)
        self.bouton.clicked.connect(self.calculer)

        self.resultat = QLabel("Résultat :", self)
        self.resultat.move(150, 120)

    def calculer(self):
        try:
            a = float(self.entree1.text())
            b = float(self.entree2.text())
            self.resultat.setText(f"Résultat : {a + b}")
        except ValueError:
            self.resultat.setText("Entrée invalide")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    fenetre = Calculatrice()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Deux zones de saisie pour les nombres (`QLineEdit`).
2. Une gestion d’exception pour éviter les erreurs d’entrée.
3. Un bouton déclenchant une addition et affichant le résultat.











[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="exercice-6--mini-projet--formulaire-de-contact"> 6. Mini-projet : formulaire de contact </h1>


### Énoncé de l’exercice :

Créer une interface de formulaire de contact avec :
- un champ pour le nom
- un champ pour l’email
- une grande zone pour écrire un message  
Ajouter un bouton "Envoyer" qui affiche un message de confirmation si tous les champs sont remplis.




### Correction

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QPushButton, QLineEdit, QTextEdit

class ContactForm(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exercice 6 – Formulaire de contact")
        self.setGeometry(100, 100, 450, 300)

        QLabel("Nom :", self).move(30, 30)
        self.nom = QLineEdit(self)
        self.nom.move(100, 30)

        QLabel("Email :", self).move(30, 70)
        self.email = QLineEdit(self)
        self.email.move(100, 70)

        QLabel("Message :", self).move(30, 110)
        self.message = QTextEdit(self)
        self.message.setGeometry(100, 110, 300, 100)

        self.btn = QPushButton("Envoyer", self)
        self.btn.move(180, 230)
        self.btn.clicked.connect(self.envoyer)

        self.label_resultat = QLabel("", self)
        self.label_resultat.move(100, 260)

    def envoyer(self):
        if self.nom.text().strip() and self.email.text().strip():
            self.label_resultat.setText("Message envoyé.")
        else:
            self.label_resultat.setText("Veuillez remplir tous les champs obligatoires.")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    fenetre = ContactForm()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans cet exercice :

1. Un champ `QTextEdit` pour le message.
2. Un formulaire structuré avec plusieurs champs.
3. Une validation minimale pour s’assurer que nom et email sont renseignés.
4. Une simulation d'envoi avec retour utilisateur.






[Revenir à la Table des matières](#table-des-matières)

<br/>
<br/>
<h1 id="tp-final--interface-de-gestion-de-produits"> TP Final – Interface de Gestion de Produits </h1>


### Énoncé du TP :

Créer une interface graphique avec **PyQt5** permettant de gérer dynamiquement une liste de produits achetés.

L’interface doit permettre :

* d’ajouter un produit avec un **nom** (`QLineEdit`) et un **prix** (`QDoubleSpinBox`),
* d’afficher les produits dans une `QListWidget` sous la forme `Nom – $ prix`,
* de **supprimer** un produit sélectionné,
* de **mettre à jour automatiquement le total cumulé** (`QLabel`),
* de valider les entrées (pas de nom vide, pas de prix nul),
* de structurer l’application en **orienté objet** via `QMainWindow`.

### Correction

```python
import sys
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QLabel, QPushButton,
    QLineEdit, QListWidget, QDoubleSpinBox
)

class GestionProduits(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("TP Final – Gestion de Produits")
        self.setGeometry(100, 100, 500, 400)

        self.total = 0.0

        # Champ pour le nom
        QLabel("Produit :", self).move(30, 30)
        self.input_nom = QLineEdit(self)
        self.input_nom.move(100, 30)
        self.input_nom.resize(200, 25)

        # Champ pour le prix
        QLabel("Prix :", self).move(30, 70)
        self.input_prix = QDoubleSpinBox(self)
        self.input_prix.setMaximum(10000)
        self.input_prix.setPrefix("$ ")
        self.input_prix.move(100, 70)

        # Bouton Ajouter
        self.btn_ajouter = QPushButton("Ajouter", self)
        self.btn_ajouter.move(320, 30)
        self.btn_ajouter.clicked.connect(self.ajouter_produit)

        # Liste des produits
        self.liste = QListWidget(self)
        self.liste.move(30, 120)
        self.liste.resize(300, 200)

        # Bouton Supprimer
        self.btn_supprimer = QPushButton("Supprimer", self)
        self.btn_supprimer.move(350, 120)
        self.btn_supprimer.clicked.connect(self.supprimer_selection)

        # Total
        self.label_total = QLabel("Total : $ 0.00", self)
        self.label_total.move(30, 340)
        self.label_total.resize(300, 30)

    def ajouter_produit(self):
        nom = self.input_nom.text().strip()
        prix = self.input_prix.value()
        if nom and prix > 0:
            ligne = f"{nom} – $ {prix:.2f}"
            self.liste.addItem(ligne)
            self.total += prix
            self.mettre_a_jour_total()
            self.input_nom.clear()
            self.input_prix.setValue(0)
        else:
            self.label_total.setText("Erreur : nom ou prix invalide.")

    def supprimer_selection(self):
        item = self.liste.currentItem()
        if item:
            texte = item.text()
            try:
                prix = float(texte.split("$")[-1])
                self.total -= prix
                self.liste.takeItem(self.liste.currentRow())
                self.mettre_a_jour_total()
            except:
                pass

    def mettre_a_jour_total(self):
        self.label_total.setText(f"Total : $ {self.total:.2f}")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    fenetre = GestionProduits()
    fenetre.show()
    sys.exit(app.exec_())
```

### Ce qu’on a ajouté dans ce TP :

1. Une architecture orientée objet avec `QMainWindow`.
2. Un champ `QLineEdit` pour le nom du produit.
3. Un champ `QDoubleSpinBox` pour entrer un prix numérique.
4. Un `QPushButton` pour ajouter un produit à la liste.
5. Une `QListWidget` pour afficher dynamiquement les produits.
6. Un bouton **Supprimer** pour retirer un produit sélectionné.
7. Un `QLabel` pour le total cumulé, mis à jour en temps réel.
8. Une validation d’entrée (pas de nom vide, pas de prix nul).
9. Une remise à zéro automatique des champs après ajout.

[Revenir à la Table des matières](#table-des-matières)

