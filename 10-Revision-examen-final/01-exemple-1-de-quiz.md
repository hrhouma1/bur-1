# Quiz PySide6 – Interfaces Graphiques

## A. Bases & cycle de vie (Q1–Q6)

1. (QCM) Quel est l’ordre minimal correct pour lancer une app PySide6 ?

* A. `QMainWindow → QApplication.exec() → QApplication`
* B. `QApplication → Création des widgets → show() → app.exec()`
* C. `QWidget → app.show() → QApplication.exec()`
* D. `QApplication → app.exec() → show()`

2. (Réponse courte) Différence principale entre `QWidget` et `QMainWindow` dans PySide6 ?

3. (V/F) `QApplication` doit être instanciée **exactement une fois** par processus GUI.

4. (QCM) Quelle méthode ferme proprement une fenêtre principale ?

* A. `close()`
* B. `hide()`
* C. `deleteLater()`
* D. `quit()`

5. (Réponse courte) À quoi sert `QApplication.instance()` et dans quel cas s’en servir ?

6. (V/F) Appeler `show()` sur un widget enfant suffit pour afficher toute l’application même sans fenêtre principale.

---

## B. Widgets & Layouts (Q7–Q12)

7. (QCM) Quel layout s’adapte le mieux à une barre d’outils verticale à gauche et un contenu principal à droite ?

* A. `QVBoxLayout`
* B. `QHBoxLayout`
* C. `QGridLayout`
* D. `QStackedLayout`

8. (Réponse courte) Citer deux avantages de `QFormLayout` par rapport à `QGridLayout` pour des formulaires.

9. (Lecture de code) Que fait ce code ?

```python
w = QWidget()
lay = QGridLayout(w)
btn = QPushButton("OK")
lay.addWidget(btn, 0, 0, 1, 2)
w.show()
```

Réponds en une phrase.

10. (QCM) Quel widget pour afficher du texte riche non éditable avec liens cliquables ?

* A. `QLabel`
* B. `QLineEdit`
* C. `QTextBrowser`
* D. `QPlainTextEdit`

11. (V/F) `QStackedWidget` permet de conserver l’état de chaque page lorsqu’on change de vue.

12. (Réponse courte) Comment forcer un widget à s’étirer horizontalement dans un `QHBoxLayout` ?

---

## C. Signaux & Slots (Q13–Q16)

13. (QCM) Associer correctement :

1) `clicked` →
2) `textChanged` →
3) `valueChanged` →
   Widgets : A. `QLineEdit` B. `QPushButton` C. `QSpinBox`
   Propose la correspondance (ex. 1-B, 2-A, 3-C).

14. (V/F) On peut connecter plusieurs slots au même signal.

15. (Réponse courte) Quelle est la signature Python (style PySide6) pour connecter un signal à une méthode d’instance ?

16. (Lecture de code) Que se passe-t-il à l’exécution ?

```python
btn = QPushButton("Run")
def handler():
    btn.setText("Done")
btn.clicked.connect(handler)
btn.clicked.emit()
```

Décris l’état final du bouton et la portée de l’émission manuelle.

---

## D. Événements, focus & clavier (Q17–Q19)

17. (QCM) Pour intercepter toutes les pressions clavier d’un widget, on redéfinit :

* A. `paintEvent()`
* B. `keyPressEvent()`
* C. `eventFilter()`
* D. `mousePressEvent()`

18. (V/F) Un `eventFilter` posé sur `QApplication` peut intercepter les événements de tous les widgets.

19. (Réponse courte) Différence entre `accept()` et `ignore()` dans un gestionnaire d’événement ?

---

## E. Threads, tâches & réactivité (Q20–Q22)

20. (QCM) Quelle combinaison est la plus sûre pour un traitement long ?

* A. Boucle `for` lente dans le slot de `clicked`
* B. `QThread` + signaux/slots pour reporter les résultats au thread GUI
* C. `time.sleep()` dans le thread principal
* D. Calcul lourd dans `paintEvent()`

21. (V/F) Mettre à jour directement l’UI depuis un `QThread` est recommandé si on verrouille avec `QMutex`.

22. (Réponse courte) Rôle de `QTimer` dans les tâches périodiques sans bloquer l’UI.

---

## F. Modèle/Vue (Model–View) (Q23–Q25)

23. (QCM) Pour afficher une liste simple d’items éditables avec MVC, on utilise :

* A. `QListWidget`
* B. `QListView` + `QStringListModel`
* C. `QTableWidget`
* D. `QTreeWidget`

24. (Réponse courte) Avantage d’un `QAbstractItemModel` custom par rapport à `QTableWidget`.

25. (Lecture de code) Que va afficher la cellule (0,1) ?

```python
model = QStandardItemModel(2, 2)
model.setItem(0, 1, QStandardItem("Hello"))
view = QTableView()
view.setModel(model)
view.show()
```

---

## G. Styles & ressources (QSS, icônes, traductions) (Q26–Q28)

26. (QCM) Quel sélecteur QSS cible uniquement les `QPushButton` d’une boîte de dialogue nommée `LoginDialog` ?

* A. `QPushButton`
* B. `#LoginDialog QPushButton`
* C. `.LoginDialog QPushButton`
* D. `QDialog QPushButton#LoginDialog`

27. (Réponse courte) À quoi sert un fichier `.qrc` et comment est-il exploité à l’exécution avec PySide6 ?

28. (V/F) Les traductions via `QTranslator` peuvent être chargées dynamiquement après `app.exec()`.

---

## H. Fenêtres, dialogues & navigation (Q29–Q30)

29. (QCM) Quel choix pour un dialogue modal standard “Ouvrir un fichier” ?

* A. `QFileDialog.getOpenFileName(...)`
* B. `QDialog().exec()` + `QLineEdit` pour le chemin
* C. `QMessageBox.information(...)`
* D. `QWizard`

30. (Lecture de code) Décris le flux :

```python
class Win(QMainWindow):
    def __init__(self):
        super().__init__()
        self.btn = QPushButton("Ask", self)
        self.btn.clicked.connect(self.ask)

    def ask(self):
        ret = QMessageBox.question(self, "Confirm", "Proceed?")
        if ret == QMessageBox.Yes:
            self.statusBar().showMessage("OK")
        else:
            self.statusBar().showMessage("Cancelled")
```

Que s’affiche dans la barre de statut selon la réponse ?
