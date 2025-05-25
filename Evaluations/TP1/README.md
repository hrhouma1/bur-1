<h1 id="table-des-matieres">Table des matières – Projet PySide6 complet</h1>

```markdown
# Table des matières

1. [Présentation du projet et architecture visée](#partie1)
2. [Création du projet et environnement virtuel](#partie2)
3. [Création de l'interface de connexion (`login.ui`)](#partie3)
4. [Création de l'interface principale (`dashboard.ui`)](#partie4)
5. [Création des pages internes utilisateurs et modules](#partie5)
6. [Conversion des fichiers `.ui` en `.py`](#partie6)
7. [Création du fichier principal `main.py`](#partie7)
8. [Connexion des vues avec `QStackedWidget`](#partie8)
9. [Affichage dynamique des utilisateurs](#partie9)
10. [Affichage dynamique des modules](#partie10)
11. [Dialogue d’ajout d’un utilisateur](#partie11)
12. [Dialogue d’ajout d’un module](#partie12)
13. [Intégration de la base de données SQLite](#partie13)
14. [Liaison entre base de données et interface](#partie14)
15. [Validation des champs et gestion des erreurs](#partie15)
16. [Déconnexion et retour à l’écran de connexion](#partie16)
17. [Ajout des ressources graphiques avec `.qrc`](#partie17)
18. [Organisation du projet en modules](#partie18)
19. [Fichier de configuration externe (`config.json`)](#partie19)
20. [Création d’un exécutable Windows avec PyInstaller](#partie20)
```

---

## Liens vers les fichiers du dépôt GitHub

### Partie 3 – Interface de connexion

[`ui/login.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/login.ui)

### Partie 4 – Interface principale

[`ui/dashboard.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/dashboard.ui)

### Partie 5 – Pages internes

[`ui/page_utilisateurs.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/page_utilisateurs.ui)
[`ui/page_modules.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/page_modules.ui)

### Partie 6 – Fichiers `.py` générés

[`ui_gen/login.py`](https://github.com/ton-compte/projet_pyside6/blob/main/ui_gen/login.py)
[`ui_gen/dashboard.py`](https://github.com/ton-compte/projet_pyside6/blob/main/ui_gen/dashboard.py)
[`ui_gen/page_utilisateurs.py`](https://github.com/ton-compte/projet_pyside6/blob/main/ui_gen/page_utilisateurs.py)
[`ui_gen/page_modules.py`](https://github.com/ton-compte/projet_pyside6/blob/main/ui_gen/page_modules.py)

### Partie 7 – Fichier principal

[`main.py`](https://github.com/ton-compte/projet_pyside6/blob/main/main.py)

### Partie 11 et 12 – Dialogues modaux

[`ui/AjoutUtilisateurDialog.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/AjoutUtilisateurDialog.ui)
[`ui/AjoutModuleDialog.ui`](https://github.com/ton-compte/projet_pyside6/blob/main/ui/AjoutModuleDialog.ui)

### Partie 13 – Base de données SQLite

[`data/database.py`](https://github.com/ton-compte/projet_pyside6/blob/main/data/database.py)
`data/database.db` → créé automatiquement au premier lancement

### Partie 17 – Ressources graphiques

[`assets/ressources.qrc`](https://github.com/ton-compte/projet_pyside6/blob/main/assets/ressources.qrc)
[`ui_gen/ressources_rc.py`](https://github.com/ton-compte/projet_pyside6/blob/main/ui_gen/ressources_rc.py)

### Partie 18 – Structure logique

[`logic/dialogs.py`](https://github.com/ton-compte/projet_pyside6/blob/main/logic/dialogs.py)
[`logic/dashboard_logic.py`](https://github.com/ton-compte/projet_pyside6/blob/main/logic/dashboard_logic.py)
[`logic/login_logic.py`](https://github.com/ton-compte/projet_pyside6/blob/main/logic/login_logic.py)

### Partie 19 – Configuration externe

[`config/config.json`](https://github.com/ton-compte/projet_pyside6/blob/main/config/config.json)

### Partie 20 – Fichier `.spec` et exécutable

`gestionnaire.spec` (à créer manuellement)
`dist/gestionnaire.exe` (généré par PyInstaller)

