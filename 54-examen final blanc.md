
# Résumé des **10 thèmes**

1. **Introduction PySide6** — Structure et cycle de vie d’une application Qt.
2. **Widgets & Layouts** — Utilisation des widgets de base et organisation avec layouts.
3. **Signaux & Slots** — Programmation événementielle et signaux personnalisés.
4. **Qt Designer** — Création et intégration d’interfaces `.ui` dans PySide6.
5. **Menus & Dialogues** — Barres de menus, boîtes de dialogue et interactions.
6. **Événements & Interactions** — Gestion clavier/souris, drag & drop, context menus.
7. **Tableaux & Modèles** — QTableWidget/View et pattern Model/View personnalisés.
8. **SQLAlchemy ORM** — Modélisation, relations et persistance des données.
9. **FastAPI** — Création d’API REST, validation Pydantic et communication avec PySide6.
10. **Architecture 3-tiers** — Séparation UI/Services/BD avec évolutivité et maintenabilité.




<br/>
<br/>

# Partie 1



### 1. Introduction PySide6

**Q1.** Quelle classe lance une application PySide6 ?
- a) `QWidget`
- b) `QApplication`
- c) `QMainWindow`
- d) `QCoreApplication`

**Q2.** Quelle méthode démarre la boucle d’événements Qt ?
- a) `start()`
- b) `run()`
- c) `exec()`
- d) `main()`

---

### 2. Widgets & Layouts

**Q3.** `QVBoxLayout` organise les widgets :
- a) Horizontalement
- b) Verticalement
- c) En grille
- d) En pile avec onglets

**Q4.** Quel widget est adapté à une saisie de texte simple ?
- a) `QTextEdit`
- b) `QLineEdit`
- c) `QLabel`
- d) `QPushButton`

---

### 3. Signaux & Slots

**Q5.** Quelle syntaxe connecte un bouton à une fonction ?
- a) `button.link(func)`
- b) `button.connect(func)`
- c) `button.clicked.connect(func)`
- d) `button.run(func)`

**Q6.** Où peut-on définir un signal personnalisé ?
- a) Dans un `QWidget` sans héritage
- b) Dans une classe héritant de `QObject`
- c) Uniquement dans Qt Designer
- d) Impossible en PySide6

---

### 4. Qt Designer

**Q7.** À quoi sert Qt Designer ?
- a) Compiler le code
- b) Dessiner des interfaces graphiques
- c) Déboguer du Python
- d) Générer des modèles de données

**Q8.** Quelle commande convertit un `.ui` en `.py` ?
- a) `pyside6-compile`
- b) `pyside6-uic`
- c) `qt-convert`
- d) `pyuic5`

---

### 5. Menus & Dialogues

**Q9.** Quelle classe gère une barre de menus ?
- a) `QToolBar`
- b) `QMenuBar`
- c) `QMenuBox`
- d) `QMenuDialog`

**Q10.** Pour ouvrir un fichier, on utilise :
- a) `QFileDialog.getOpenFileName()`
- b) `QInputDialog.getFile()`
- c) `QDialog.open()`
- d) `QMessageBox.getFile()`

---

### 6. Événements & Interactions

**Q11.** Quelle méthode redéfinir pour une touche clavier ?
- a) `mousePressEvent`
- b) `keyPressEvent`
- c) `eventKey`
- d) `keyboardEvent`

**Q12.** Comment activer un menu contextuel sur clic droit ?
- a) `setContextMenuPolicy(Qt.CustomContextMenu)`
- b) `enableRightClickMenu()`
- c) `activateContextMenu()`
- d) `useMenu(True)`

---

### 7. Tableaux & Modèles

**Q13.** `QTableWidget` vs `QTableView` :
- a) `QTableWidget` utilise un modèle intégré
- b) `QTableView` ne peut afficher que du texte
- c) `QTableWidget` est plus rapide
- d) Aucun ne supporte l’édition

**Q14.** Pourquoi préférer Model/View ?
- a) Pour limiter le code
- b) Pour gérer de grandes données efficacement
- c) Car `QTableWidget` est obsolète
- d) Parce que Qt Designer l’impose

---

### 8. SQLAlchemy ORM

**Q15.** Quelle fonction initialise les modèles ORM ?
a) `Base = declarative_base()`
b) `Base = create_session()`
c) `Base = db.Model()`
d) `Base = orm_start()`

**Q16.** Relation One-to-Many :
- a) `ForeignKey` + `relationship()`
- b) `OneToManyField()`
- c) `ManyLink()`
- d) `connect_tables()`

---

### 9. FastAPI

**Q17.** Quelle syntaxe définit une route GET ?
- a) `@app.route("/items")`
- b) `@app.get("/items")`
- c) `@route.get("/items")`
- d) `@get.app("/items")`

**Q18.** Quelle bibliothèque gère la validation de schémas ?
- a) `marshmallow`
- b) `pydantic`
- c) `sqlalchemy`
- d) `validators`

---

### 10. Architecture 3-tiers

**Q19.** Quelles sont les trois couches ?
- a) Client – Serveur – Internet
- b) Présentation – Logique – Données
- c) UI – Threads – API
- d) Vue – Modèle – Contrôleur

**Q20.** Un avantage clé du 3-tiers est :
- a) Réduit le nombre de fichiers
- b) Sépare les responsabilités
- c) Évite l’usage d’API
- d) Supprime la base de données


<br/>
<br/>


# Partie 2


### 1. Introduction PySide6

* Q1. Quelle est la classe principale qui initialise une application PySide6 ?
* Q2. Quelle méthode lance la boucle d’événements principale d’une application Qt ?

---

### 2. Widgets & Layouts

* Q3. Quelle différence entre `QVBoxLayout` et `QHBoxLayout` ?
* Q4. Quel widget utiliser pour une saisie de texte sur une seule ligne ?

---

### 3. Signaux & Slots

* Q5. Quel mécanisme permet de connecter un bouton à une fonction en PySide6 ?
* Q6. Peut-on créer des signaux personnalisés dans une classe héritant de `QObject` ?

---

### 4. Qt Designer

* Q7. À quoi sert l’outil **Qt Designer** ?
* Q8. Quelle commande Python convertit un fichier `.ui` en `.py` ?

---

### 5. Menus & Dialogues

* Q9. Quelle classe permet de créer une barre de menus dans PySide6 ?
* Q10. Quel dialogue utiliser pour ouvrir un fichier ?

---

### 6. Événements & Interactions

* Q11. Quelle méthode redéfinir pour capturer une touche clavier dans une fenêtre ?
* Q12. Comment activer un menu contextuel sur un clic droit ?

---

### 7. Tableaux & Modèles

* Q13. Quelle différence entre `QTableWidget` et `QTableView` ?
* Q14. Pourquoi utiliser le pattern Model/View plutôt que des widgets simples ?

---

### 8. SQLAlchemy ORM

* Q15. Quelle fonction de SQLAlchemy sert à définir la base des modèles ORM ?
* Q16. Comment définir une relation One-to-Many entre deux classes ORM ?

---

### 9. FastAPI

* Q17. Quel décorateur est utilisé pour définir une route GET dans FastAPI ?
* Q18. Quelle bibliothèque FastAPI utilise pour la validation des données ?

---

### 10. Architecture 3-tiers

* Q19. Quelles sont les trois couches principales d’une architecture 3-tiers ?
* Q20. Cite un avantage clé de l’architecture 3-tiers par rapport au monolithique.




<br/>
<br/>



<br/>
<br/>

# Partie 3 (Choisir 3 exercices)

### 1) Introduction PySide6 — “Hello Lifecycle”

**Objectif** : afficher une fenêtre, gérer `QApplication` et la fermeture propre.
**Concept clé** : cycle de vie Qt.
**Périmètre** : `QApplication` + `QMainWindow` minimal, `statusBar()` qui affiche “Ready”.
**Étapes** : créer la fenêtre, brancher `aboutToQuit`, log dans la console.
**Acceptation** : lancer → fenêtre titre “Hello Lifecycle”, fermer → log “Goodbye”.

---

### 2) Widgets & Layouts — “FormFlex”

**Objectif** : petit formulaire réactif (Nom, Email, Bouton).
**Concept clé** : `QVBoxLayout`, `QGridLayout`.
**Périmètre** : `QLineEdit`, `QPushButton`, layout qui s’adapte au redimensionnement.
**Étapes** : labels + champs, bouton “Valider” qui affiche un `QLabel` récap.
**Acceptation** : redimensionner ne chevauche rien, clic → récap exact.

---

### 3) Signaux & Slots — “ChronoSignal”

**Objectif** : chrono start/stop/reset.
**Concept clé** : signaux Qt natifs + **signal personnalisé** `tick(int ms)`.
**Périmètre** : `QTimer`, boutons Start/Stop/Reset, label mm\:ss.
**Étapes** : `QTimer.timeout` → émettre `tick`; slot met à jour le label.
**Acceptation** : Start incrémente, Stop fige, Reset remet à 00:00.

---

### 4) Qt Designer — “Login.ui”

**Objectif** : boîte de dialogue de connexion produite dans Designer.
**Concept clé** : charger `.ui` avec `pyside6-uic` (ou `QUiLoader`).
**Périmètre** : champs user/password, bouton “Se connecter”.
**Étapes** : dessiner `.ui`, générer `.py`, connecter le bouton pour valider non-vide.
**Acceptation** : compilation `.ui` → `.py`, clic vide → `QMessageBox` d’erreur.

---

### 5) Menus & Dialogues — “MiniPad”

**Objectif** : éditeur texte minimal.
**Concept clé** : `QMenuBar`, `QAction`, `QFileDialog`.
**Périmètre** : actions Fichier > Nouveau/Ouvrir/Enregistrer/Quitter, `QPlainTextEdit`.
**Étapes** : ouvrir lit un fichier `.txt`, enregistrer écrit le contenu.
**Acceptation** : ouvrir/enregistrer fonctionnent, astérisque dans le titre si non sauvegardé.

---

### 6) Événements & Interactions — “DropGallery”

**Objectif** : déposer des images pour les lister et prévisualiser.
**Concept clé** : drag\&drop (`dragEnterEvent`, `dropEvent`), menu contextuel.
**Périmètre** : zone drop, `QListWidget` des noms de fichiers, aperçu à droite.
**Étapes** : accepter fichiers `.png/.jpg`, clic droit → “Ouvrir dans l’explorateur”.
**Acceptation** : glisser 2 images → 2 lignes, aperçu change à la sélection.

---

### 7) Tables & Modèles — “UsersView”

**Objectif** : afficher une table via **Model/View**.
**Concept clé** : `QAbstractTableModel` personnalisé.
**Périmètre** : colonnes (ID, Username, Email), 10 lignes mock.
**Étapes** : implémenter `rowCount`, `columnCount`, `data`, `headerData`.
**Acceptation** : tri activé sur Username, double-clic n’édite pas (lecture seule).

---

### 8) SQLAlchemy ORM — “ProductsLite (SQLite)”

**Objectif** : CRUD minimal produits (nom, prix).
**Concept clé** : `declarative_base`, `Session`, transactions.
**Périmètre** : bouton Ajouter (ouvre mini dialog), table en lecture via `QTableView`.
**Étapes** : modèle `Product(id, name, price)`, créer DB SQLite, lister/ajouter/supprimer.
**Acceptation** : ajout persistant (relaunch conserve), suppression retire la ligne et DB.

---

### 9) FastAPI — “PingTasks API + Client”

**Objectif** : exposer une API et l’appeler depuis PySide6.
**Concept clé** : routes `GET /ping`, `GET /tasks`, `POST /tasks`.
**Périmètre** : API FastAPI en local, client PySide6 avec `httpx` en thread.
**Étapes** : bouton “Ping API” affiche réponse; ajouter une tâche et relire la liste.
**Acceptation** : Ping renvoie `{"status":"ok"}`, liste se met à jour après POST.

---

### 10) Architecture 3-tiers — “DeskBooks-Mini”

**Objectif** : bout-en-bout 3-tiers (UI ↔ API ↔ ORM).
**Concept clé** : séparation Présentation / Services / Données.
**Périmètre** : FastAPI (Book: id, title), SQLAlchemy (SQLite), PySide6 (liste + ajout).
**Étapes** : `POST /books`, `GET /books`; côté client : formulaire titre, liste via modèle.
**Acceptation** : ajouter un livre → persiste en DB, rechargement liste OK, UI non bloquée.

