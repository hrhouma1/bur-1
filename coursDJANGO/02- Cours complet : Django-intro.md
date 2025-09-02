# 📘 Cours introductif pour Django 

---

## 1. C’est quoi Django ?

👉 Imagine encore un **restaurant** :

* Les **clients** = navigateurs ou applis qui envoient des requêtes.
* Le **serveur** = Django (beaucoup plus grand que Flask).
* La **cuisine** = ton code Python, mais très organisé.
* Le **menu** = les **URLs**.
* Les **chefs spécialisés** = les **views** (fonctions ou classes).
* Les **recettes** = les **templates** (HTML, affichage).
* La **comptabilité** = l’ORM intégré avec SQL (SQLite, PostgreSQL…).

⚡ Django est :

* Un **framework complet** (pas micro comme Flask).
* Livré avec ORM, admin, authentification, templates.
* Idéal pour sites **structurés**, **CRUD**, **apps web complètes**.

---

## 2. Installation de Django

### Étape 1 : créer dossier de projet

```bash
mkdir projet_django
cd projet_django
```

### Étape 2 : créer environnement virtuel

Linux/macOS :

```bash
python3 -m venv venv
source venv/bin/activate
```

Windows :

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

### Étape 3 : installer Django

```bash
pip install django
```

### Vérifier :

```bash
django-admin --version
```

---

## 3. Créer un projet Django

Commande magique :

```bash
django-admin startproject monsite .
```

👉 Arborescence générée :

```
monsite/
    __init__.py
    settings.py   # configuration
    urls.py       # les "menus" (routes)
    asgi.py
    wsgi.py
manage.py        # outil principal (ligne de commande)
```

---

## 4. Lancer le serveur

```bash
python manage.py runserver
```

👉 Ouvre [http://127.0.0.1:8000](http://127.0.0.1:8000) → Django t’affiche une page “It worked!” 🎉

---

## 5. Créer une application

Django sépare en **apps** (modules).

Commande :

```bash
python manage.py startapp blog
```

👉 Arborescence `blog/` :

```
blog/
    admin.py
    apps.py
    models.py   # BDD
    views.py    # fonctions qui répondent aux requêtes
    urls.py     # urls propres à l'app
    templates/  # fichiers HTML
```

---

## 6. Ajouter une route simple

Dans `blog/views.py` :

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Bienvenue dans Django !")
```

Dans `blog/urls.py` :

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.home, name="home"),
]
```

Dans `monsite/urls.py` :

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("blog.urls")),
]
```

👉 Lance `python manage.py runserver` → [http://127.0.0.1:8000](http://127.0.0.1:8000) → “Bienvenue dans Django !”

---

## 7. Django et les bases de données

### Définir un modèle

Dans `blog/models.py` :

```python
from django.db import models

class Article(models.Model):
    titre = models.CharField(max_length=200)
    contenu = models.TextField()
    date_pub = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titre
```

### Migrer (créer la table dans SQLite)

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 8. L’admin Django

Créer un superuser :

```bash
python manage.py createsuperuser
```

Lancer le serveur, puis aller sur [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)
👉 Tu peux gérer tes `Article` via une belle interface graphique.

---

## 9. CRUD avec Django

### Lire (liste des articles)

Dans `blog/views.py` :

```python
from django.shortcuts import render
from .models import Article

def liste_articles(request):
    articles = Article.objects.all()
    return render(request, "blog/liste.html", {"articles": articles})
```

### Template `blog/templates/blog/liste.html`

```html
<h1>Liste des articles</h1>
<ul>
  {% for article in articles %}
    <li>{{ article.titre }} - {{ article.date_pub }}</li>
  {% endfor %}
</ul>
```

### Ajouter URL

Dans `blog/urls.py` :

```python
urlpatterns = [
    path("", views.home),
    path("articles/", views.liste_articles, name="liste_articles"),
]
```

---

## 10. Répétition (pour que ça rentre)

* `manage.py runserver` → lance le serveur.
* `views.py` → cuisine (fonctions qui préparent les plats).
* `urls.py` → menu (chemin → view).
* `models.py` → recettes en base de données (tables).
* `templates/` → assiettes (HTML affiché).
* `admin/` → interface de gestion automatique.

Django = tout intégré (ORM + admin + templates).

---

## 11. Limite de Django “pur”

* Pas de Swagger automatique.
* Mais Django peut faire des APIs via **Django REST Framework (DRF)**.
* Avec DRF : `/api` + docs automatiques (très puissant).

---

# 🎯 Petit exercice pour toi

1. Crée une app `blog` avec un modèle `Article(titre, contenu, date_pub)`.
2. Ajoute la route `/articles/` qui affiche la liste en HTML.
3. Ajoute un article via l’admin et vérifie qu’il s’affiche.

