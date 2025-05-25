
<h1 id="plan-complet">Plan complet – Projet PySide6 avec interfaces liées</h1>

### Contexte :

Le projet final sera une **application de gestion d’utilisateurs et de modules**, avec :

* un **écran de connexion sécurisé**,
* un **tableau de bord dynamique** (affichage de widgets conditionnels),
* plusieurs **interfaces interconnectées** via `QStackedWidget`,
* un **système de dialogues (création, modification, suppression)**,
* une **base de données SQLite** intégrée,
* une structure de projet claire et modulaire (`/ui`, `/logic`, `/data`, etc.),
* et un installeur final `.exe`.

---

### Partie 1. Présentation du projet final et architecture visée

* Présentation de l’idée : gestion d’utilisateurs avec accès à différents modules
* Vue d’ensemble de l’arborescence finale
* Vue générale des interfaces à construire (connexion, tableau de bord, etc.)

### Partie 2. Création du projet et environnement virtuel

* `mkdir`, `cd`, `python -m venv`, activation
* Installation de `pyside6`, `pyside6-tools`

### Partie 3. Lancement de Qt Designer et création de `login.ui`

* Interface simple de connexion (username, password, bouton, message d’erreur)
* Sauvegarde dans `/ui/login.ui`

### Partie 4. Création de `dashboard.ui` dans Qt Designer

* Interface principale après connexion : barre latérale, boutons, zone centrale
* Création d’un layout avec `QStackedWidget`

### Partie 5. Ajout de 2 sous-vues : `page_utilisateurs.ui`, `page_modules.ui`

* Interfaces imbriquées accessibles depuis le menu latéral
* Utilisation du `QStackedWidget` dans `dashboard.ui`

### Partie 6. Conversion des `.ui` avec `pyside6-uic` et structuration `/ui`

* Génération des `.py` à partir des `.ui`
* Organisation du dossier `/ui`

### Partie 7. Script `main.py` et logique de bascule entre vues

* Lancement initial, bascule entre `login.ui` et `dashboard.ui`
* Code de navigation avec `self.stackedWidget.setCurrentIndex(...)`

### Partie 8. Système de connexion simulé

* Champs utilisateur/mot de passe
* Gestion des erreurs (QMessageBox)
* Redirection vers `dashboard` après validation

### Partie 9. Navigation dans le tableau de bord (`btn_utilisateurs`, `btn_modules`)

* Connexion des boutons de la barre latérale à `QStackedWidget`

### Partie 10. Interaction : charger une liste d'utilisateurs dynamiquement

* Affichage dans `QTableWidget`
* Ajout/suppression d’éléments avec boutons

### Partie 11. Création de dialogues : `AjoutUtilisateurDialog.ui`

* Création d’une fenêtre modale pour saisir de nouveaux utilisateurs
* Lancement via `QDialog.exec()`

### Partie 12. Ajout d’une base SQLite (`sqlite3`) pour stocker les données

* Création de `database.db`, tables `users`, `modules`
* Insertion/lecture/suppression avec `sqlite3`

### Partie 13. Liaison base de données ↔ interface

* Chargement des données dans le tableau
* Insertion depuis le dialogue
* Suppression par sélection dans le tableau

### Partie 14. Validation de champs et messages d’erreur

* Champs obligatoires
* Contrôles (longueur, unicité…)
* Affichage d’erreurs avec `QMessageBox.critical`

### Partie 15. Ajout d’un menu principal et d’un système de déconnexion

* Menu `Fichier → Se déconnecter`
* Retour à `login.ui`

### Partie 16. Ajout de transitions visuelles (fondu, animation de slide)

* Utilisation de `QPropertyAnimation`
* Effet de slide entre vues

### Partie 17. Gestion des ressources (icônes, images, `.qrc`)

* Création d’un fichier `.qrc`, conversion avec `pyside6-rcc`
* Utilisation dans les boutons, menus

### Partie 18. Structuration avancée du projet

* Dossiers `/ui`, `/logic`, `/data`, `/assets`
* Séparation des fichiers : `login_logic.py`, `dashboard_logic.py`, etc.

### Partie 19. Création d’un fichier de configuration (JSON, YAML)

* Stockage des paramètres (chemin DB, thèmes, langue…)
* Chargement dynamique à l’ouverture

### Partie 20. Génération d’un installeur Windows (.exe) avec `pyinstaller`

* Script de build
* Inclure tous les fichiers nécessaires
* Générer un `.exe` exécutable sur n’importe quel poste Windows

