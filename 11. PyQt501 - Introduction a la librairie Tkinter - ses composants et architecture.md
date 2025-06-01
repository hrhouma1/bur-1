# Introduction à la bibliothèque **PyQt5**

<br/>

# 1. Qu’est-ce que **PyQt5** ?

**PyQt5** est un binding Python de la bibliothèque C++ **Qt 5**, développée par *The Qt Company*.
Il permet de créer des interfaces graphiques modernes, multiplateformes et très riches en fonctionnalités.

* **Langage :** Python
* **Bibliothèque sous-jacente :** Qt 5 (C++)
* **Licence :** GPL ou commerciale
* **Avantages :**

  * Apparence professionnelle et moderne
  * Riche ensemble de widgets
  * Puissant système de signaux/slots (communication entre composants)
  * Large communauté et documentation abondante

<br/>

# 2. Objectifs de **PyQt5**

PyQt5 permet de :

* Créer des interfaces graphiques complexes et réactives
* Organiser des interfaces via des layouts flexibles (QVBoxLayout, QGridLayout, etc.)
* Gérer les événements avec le système de signaux/slots
* Créer des dialogues, fenêtres, menus, barres d’outils, formulaires
* Intégrer des widgets personnalisés, images, graphiques, vidéos, etc.

<br/>

# 3. Principaux composants (widgets)

Voici les **widgets** de base disponibles avec PyQt5 :

| Widget                   | Rôle                                   |
| ------------------------ | -------------------------------------- |
| `QLabel`                 | Affiche un texte ou une image          |
| `QPushButton`            | Bouton cliquable                       |
| `QLineEdit`              | Champ de saisie à une ligne            |
| `QTextEdit`              | Champ de texte multiligne              |
| `QCheckBox`              | Case à cocher                          |
| `QRadioButton`           | Bouton radio                           |
| `QListWidget`            | Liste avec éléments sélectionnables    |
| `QComboBox`              | Menu déroulant (liste déroulante)      |
| `QTableWidget`           | Tableau avec cellules modifiables      |
| `QSlider`                | Curseur pour valeurs numériques        |
| `QSpinBox`               | Champ numérique avec flèches           |
| `QDialog`, `QMainWindow` | Fenêtres principales ou secondaires    |
| `QMenuBar`, `QToolBar`   | Menus, barres d’outils                 |
| `QWidget`                | Composant de base, conteneur universel |

<br/>

# 4. Architecture de base d'une application PyQt5

1. Création de l’application : `app = QApplication(sys.argv)`
2. Définition d’une classe personnalisée (héritée de `QWidget` ou `QMainWindow`)
3. Ajout et configuration des widgets
4. Organisation avec des layouts (`QVBoxLayout`, `QGridLayout`, etc.)
5. Connexion des signaux/slots pour gérer les événements
6. Affichage de la fenêtre avec `.show()`
7. Lancement de la boucle principale avec `sys.exit(app.exec_())`

<br/>

# 5. Cas d’usage typiques

PyQt5 est utilisé pour :

* Applications de bureau professionnelles
* Interfaces d’administration (outils de monitoring, tableaux de bord)
* Éditeurs de texte, outils graphiques, logiciels scientifiques
* Logiciels avec base de données, formulaires complexes
* Applications multi-fenêtres avec navigation structurée

<br/>

# 6. Avantages et limites

### Avantages :

* Apparence moderne, intégration native aux systèmes d’exploitation
* Extrêmement personnalisable
* Large écosystème (Qt Designer, Qt Charts, QML)
* Utilisé en production dans des projets commerciaux

### Limites :

* Courbe d’apprentissage plus élevée (classes, signaux/slots, layouts complexes)
* Licence GPL/commerciale à prendre en compte pour les applications fermées
* Nécessite une installation préalable (`pip install pyqt5`)

<br/>

# 7. Exemples de projets réalisables avec PyQt5

* Application de prise de notes avec recherche dynamique
* Interface de gestion de base de données SQLite
* Tableur léger avec options de filtrage
* Editeur de code ou de texte
* Visualiseur d’images ou de vidéos
* Tableaux de bord scientifiques interactifs
* Interface GUI pour scripts Python (traitement d’image, IA, etc.)


