# Quiz simple – Interfaces graphiques avec PySide6 (15 questions)

### Partie A – Bases

1. (Vrai/Faux) Une application PySide6 commence toujours par créer un objet `QApplication`.
2. Quel mot-clé utilise-t-on pour afficher une fenêtre ?

   * A. `open()`
   * B. `show()`
   * C. `display()`
3. Dans PySide6, `QMainWindow` est utilisé pour :

   * A. Créer une simple boîte de texte
   * B. Créer une fenêtre principale avec menus, barres d’outils et statut
   * C. Fermer l’application

<br/>

### Partie B – Widgets

4. (Vrai/Faux) Un `QPushButton` sert à créer un bouton cliquable.
5. Comment afficher du texte que l’utilisateur peut modifier ?

   * A. `QLabel`
   * B. `QLineEdit`
   * C. `QProgressBar`
6. Donne un exemple de widget utile pour saisir plusieurs lignes de texte.

<br/>

### Partie C – Layouts

7. (QCM) Quel layout place les widgets **les uns en dessous des autres** ?

   * A. `QHBoxLayout`
   * B. `QVBoxLayout`
   * C. `QGridLayout`
8. (Vrai/Faux) Un layout permet de redimensionner les widgets automatiquement si on agrandit la fenêtre.
9. Comment s’appelle le layout qui organise les widgets en tableau (lignes/colonnes) ?

<br/>

### Partie D – Signaux et Slots

10. (Vrai/Faux) Quand on clique sur un bouton, un **signal** est envoyé.
11. Complète la phrase :

```python
button.clicked.____(ma_fonction)
```

12. Donne un exemple d’action qu’on peut déclencher avec un signal (par ex. quand on clique un bouton).

<br/>

### Partie E – Dialogues et boîtes de messages

13. (QCM) Quelle classe permet d’afficher une boîte d’information (message “Bonjour !”) ?

* A. `QMessageBox`
* B. `QDialog`
* C. `QTextEdit`

14. (Vrai/Faux) `QFileDialog` permet d’ouvrir ou de sauvegarder un fichier via une fenêtre standard.
15. Cite une situation pratique où tu utiliserais une boîte de dialogue.

