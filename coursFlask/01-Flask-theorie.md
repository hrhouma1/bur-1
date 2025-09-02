Flask est un **framework web minimaliste en Python** utilisé pour développer des applications web et des APIs. Contrairement à FastAPI, il est plus ancien (créé en 2010) et a servi de point de départ à une grande partie de l’écosystème Python côté web. Voici la partie théorique essentielle :



### 1. Nature et philosophie

* Flask est un **micro-framework** : il fournit uniquement l’essentiel (gestion des requêtes HTTP, routes, et réponses).
* Sa philosophie est la **simplicité et la flexibilité** : on ajoute seulement ce dont on a besoin à travers des extensions (ORM, authentification, formulaires, etc.).
* Il ne force pas une architecture stricte : le développeur choisit comment organiser son code.

---

### 2. Fondements techniques

* Basé sur **WSGI (Web Server Gateway Interface)**, le standard historique pour connecter Python aux serveurs web.
* Utilise **Werkzeug** pour la gestion des requêtes/réponses et **Jinja2** comme moteur de templates (pour générer du HTML dynamique).
* Contrairement à FastAPI, Flask n’exploite pas nativement les types Python ni la programmation asynchrone.

---

### 3. Avantages

* **Simplicité d’apprentissage** : facile à comprendre pour un débutant, car le cœur est léger.
* **Grande flexibilité** : on choisit librement les bibliothèques à intégrer (base de données, sécurité, validation, etc.).
* **Communauté très large** : énormément de tutoriels, extensions et support disponibles.
* **Adapté aux projets légers à moyens** ou aux prototypes rapides.

---

### 4. Limites

* Pas de **validation automatique** des données (il faut utiliser des bibliothèques externes comme Marshmallow).
* Pas de **documentation interactive générée automatiquement** (contrairement à FastAPI).
* Moins performant que les frameworks modernes asynchrones, car il reste basé sur WSGI.

---

### 5. Cas d’utilisation

* Création de **sites web dynamiques** (via templates HTML avec Jinja2).
* Développement d’**APIs REST** de petite à moyenne envergure.
* **Prototypage rapide** d’applications web.
* Projets éducatifs ou de formation, car la logique est simple et compréhensible.

---

### 6. Comparaison avec FastAPI

* Flask est **plus ancien et plus simple**, mais demande plus de configuration manuelle.
* FastAPI est **plus moderne et automatisé** (validation, documentation, async).
* Flask reste populaire car il est extrêmement flexible et adapté aux projets où l’on veut garder le contrôle total.

---

👉 En résumé, **Flask est un micro-framework Python minimaliste qui fournit la base pour développer des applications web et APIs, en laissant au développeur le soin de choisir et assembler les composants supplémentaires nécessaires.**


<br/>

<br/>

# Annexe 1 - Flask vs FastAPI vs Django

| Critère                      | **Flask**                                      | **FastAPI**                                                         | **Django**                                                               |
| ---------------------------- | ---------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Année de création**        | 2010                                           | 2018                                                                | 2005                                                                     |
| **Type de framework**        | Micro-framework minimaliste                    | Framework moderne pour APIs (ASGI)                                  | Framework complet ("batteries included")                                 |
| **Philosophie**              | Flexibilité maximale, on ajoute ce qu’on veut  | Automatisation, performance et validation intégrée                  | Tout-en-un : fournit ORM, auth, templates, admin                         |
| **Gestion des données**      | Validation manuelle (via extensions)           | Validation automatique avec **Pydantic**                            | Validation via **Django Forms** et **Models**                            |
| **Documentation API**        | Pas intégrée (nécessite outils externes)       | Automatique avec Swagger / Redoc                                    | Pas native, nécessite **Django REST Framework (DRF)**                    |
| **Performance**              | Correcte mais limitée (WSGI)                   | Très rapide, optimisé async (ASGI)                                  | Lourde mais robuste (WSGI/ASGI depuis Django 3)                          |
| **Extensions / Écosystème**  | Riche écosystème d’extensions                  | Moins d’extensions mais compatible avec l’écosystème Python moderne | Énorme écosystème officiel et communautaire (DRF, CMS, e-commerce, etc.) |
| **Moteur de templates**      | **Jinja2** inclus                              | Pas de moteur intégré (API-first)                                   | **Django Template Language** intégré                                     |
| **Courbe d’apprentissage**   | Très simple, accessible aux débutants          | Moyenne (types, async, Pydantic)                                    | Plus raide (beaucoup de concepts : ORM, migrations, admin, etc.)         |
| **Cas d’usage typiques**     | Prototypes rapides, petits sites, petites APIs | APIs modernes, microservices, ML/IA                                 | Applications web complètes, e-commerce, ERP, gros projets                |
| **Organisation du projet**   | Libre, non imposée                             | Encourage une organisation claire (routes, modèles, dépendances)    | Fortement structurée (apps, modèles, vues, templates)                    |
| **Communauté et popularité** | Très grande                                    | En forte croissance                                                 | Immense, très mature et soutenue depuis 20 ans                           |

---

👉 **En résumé :**

* **Flask** = léger, simple, parfait pour prototypes ou petits projets.
* **FastAPI** = moderne, rapide, idéal pour APIs et microservices.
* **Django** = framework complet, adapté aux grandes applications web nécessitant un socle robuste (ORM, sécurité, back-office intégré).




