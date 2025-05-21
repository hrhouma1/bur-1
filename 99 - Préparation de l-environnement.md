

# ⚙ **Préparation de l’environnement sous Windows**

Avant de commencer, crée un environnement virtuel pour isoler le projet :

```bash
# Ouvre l'invite de commandes Windows (cmd) ou PowerShell
# 1. Crée un dossier pour ton projet
mkdir tkinter_projets
cd tkinter_projets

# 2. Crée un environnement virtuel
python -m venv venv

# 3. Active l’environnement virtuel
venv\Scripts\activate

# 4. Tu peux maintenant créer tes fichiers .py dans ce dossier
# (Tkinter est déjà inclus avec Python, pas besoin d'installation)
```

# ⚙ **Utile**


```bash
## Pour trouver les dépendances
pip install pipreqs
pipreqs .
## Ouvrir le fihichier requirements.txt
## Pour installer (l'inverse)
pip freeze > requirements.txt
pip install -r requirements.txt
```

- https://stackoverflow.com/questions/31684375/automatically-create-file-requirements-txt
