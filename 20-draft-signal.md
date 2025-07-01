## Objectif

Créer une petite interface graphique avec :

* Un **champ de texte** (QLineEdit)
* Un **bouton** (QPushButton)
* Un **label** (QLabel)

Quand on clique sur le bouton, le texte du champ est affiché dans le label.



## Étapes

### 1. Installer PySide6 (si ce n’est pas encore fait)

Ouvre ton terminal (console) et tape :

```bash
pip install pyside6
```



### 2. Code complet dans `mini_app.py`

```python
import sys
from PySide6.QtWidgets import QApplication, QWidget, QPushButton, QLineEdit, QLabel, QVBoxLayout

class MiniWindow(QWidget):
    def __init__(self):
        super().__init__()

        # Titre de la fenêtre
        self.setWindowTitle("Exemple Signal / Slot")

        # Créer les composants
        self.input_field = QLineEdit()         # Champ de texte
        self.button = QPushButton("Afficher")  # Bouton
        self.label = QLabel("")                # Label vide au départ

        # Layout (organisation verticale)
        layout = QVBoxLayout()
        layout.addWidget(self.input_field)
        layout.addWidget(self.button)
        layout.addWidget(self.label)

        # Appliquer le layout à la fenêtre
        self.setLayout(layout)

        # Connecter le bouton à la fonction afficher_texte
        self.button.clicked.connect(self.afficher_texte)

    def afficher_texte(self):
        # Récupère le texte du champ et l'affiche dans le label
        texte = self.input_field.text()
        self.label.setText(f"Tu as écrit : {texte}")

if __name__ == "__main__":
    app = QApplication(sys.argv)

    # Créer et afficher la fenêtre
    fenetre = MiniWindow()
    fenetre.show()

    # Lancer la boucle de l'application
    sys.exit(app.exec())
```



### 3. Résultat

Quand tu exécutes ce script (`python mini_app.py`) :

* Une fenêtre s’ouvre avec :

  * Un champ vide
  * Un bouton "Afficher"
  * Un label vide
* Quand tu tapes du texte et cliques sur le bouton :

  * Le texte s’affiche dans le label en dessous


## Analyse simple

| Élément                         | Description                                                         |
| ------------------------------- | ------------------------------------------------------------------- |
| `self.button.clicked`           | C’est le **signal** envoyé quand on clique sur le bouton            |
| `.connect(self.afficher_texte)` | C’est la **connexion** du signal à la fonction `afficher_texte()`   |
| `self.label.setText(...)`       | C’est l’effet visible de l’action : on met à jour le texte du label |


