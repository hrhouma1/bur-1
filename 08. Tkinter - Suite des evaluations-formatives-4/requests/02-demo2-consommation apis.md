<h1 id="examen-api-python">Démo 2 – Consommation d’APIs publiques avec Python</h1>

<h2 id="contexte">Contexte</h2>

Vous êtes chargé de créer un petit outil d’exploration d’APIs ouvertes. Toutes les APIs sont publiques et accessibles en lecture seule. Vous utiliserez exclusivement la méthode HTTP `GET`. L’examen vise à valider les compétences suivantes :

* Savoir faire une requête GET avec `requests`
* Analyser une réponse JSON
* Afficher les données utiles avec une mise en forme propre
* Gérer les erreurs de réponse
* Utiliser une API externe sans authentification (ou avec clé publique si fournie)

---

<h2 id="instructions-generales">Instructions générales</h2>

* Utilisez le module `requests` pour faire vos appels API.
* Affichez uniquement les données demandées (pas la réponse complète).
* Gérez les cas d’erreur (codes autres que 200, absence de clé, etc.).
* Utilisez `json.dumps(..., indent=2)` si nécessaire pour l’affichage.


---

<h2 id="partie-1-etude-api">Partie 1 – Étude d'une API de données publiques</h2>

### Question 1 – Affichage des établissements publics du département 53

Utilisez l’API suivante :
👉 `https://etablissements-publics.api.gouv.fr/v3/departements/53/cci`

**Travail demandé :**

* Faites une requête `GET`
* Affichez pour chaque établissement :

  * Le nom (`nom`)
  * L'adresse (`adresse`)
  * La ville (`ville`)
  * Le code postal (`codePostal`)

**Bonus :** Affichez le nombre total d’établissements.


---

<h2 id="partie-2-analyse-et-filtrage">Partie 2 – Analyse et filtrage de données</h2>

### Question 2 – Liste des pays par continent (Europe uniquement)

Utilisez l’API RestCountries v3 :
👉 `https://restcountries.com/v3.1/all`

**Travail demandé :**

* Filtrer les pays du continent Europe
* Pour chacun, afficher :

  * Nom du pays
  * Capitale
  * Population


---

<h2 id="partie-3-integration-avec-une-api-fictive">Partie 3 – Simulation de forum</h2>

### Question 3 – Liste des commentaires du post numéro 1

👉 `https://jsonplaceholder.typicode.com/comments?postId=1`

**Travail demandé :**

* Affichez :

  * L’auteur du commentaire (`name`)
  * Son courriel (`email`)
  * Le contenu (`body`)
* Affichez le nombre total de commentaires reçus

---

<h2 id="partie-4-api-citation">Partie 4 – API de citation</h2>

### Question 4 – Affichage d’une citation courte aléatoire

👉 `https://api.quotable.io/random?maxLength=20`

**Travail demandé :**

* Affichez la citation et son auteur
* Relancer cette requête 3 fois

---

<h2 id="partie-5-api-meteo">Partie 5 – API météo (simulation)</h2>

### Question 5 – Obtenir la météo actuelle à Montréal

👉 `http://api.openweathermap.org/data/2.5/weather?q=Montreal&appid=YOUR_API_KEY&units=metric`

**Travail demandé :**

* Affichez :

  * Température (`main.temp`)
  * Description météo (`weather[0].description`)
  * Vitesse du vent (`wind.speed`)

**Remarque :** remplacez `YOUR_API_KEY` par une clé valide si vous en avez une. Sinon, commentez la partie.

---

<h2 id="partie-6-api-crypto">Partie 6 – Prix du Bitcoin</h2>

### Question 6 – Affichage du prix du Bitcoin en dollars canadiens

👉 `https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=cad`

**Travail demandé :**

* Affichez le prix actuel du Bitcoin en CAD



---

<h2 id="partie-7-analyse">Partie 7 – Mini-analyse</h2>

### Question 7 – Comparaison de deux APIs

Choisissez **deux** des APIs utilisées plus haut et répondez :

* Quelle API est la plus facile à utiliser et pourquoi ?
* Laquelle a la structure de données la plus complexe ?
* Y avait-il des erreurs ou des pièges particuliers ?

---

<h2 id="livrables">Livrables</h2>

* Un fichier `.py` contenant votre code bien commenté
* Un fichier `README.md` avec :

  * Le nom des APIs utilisées
  * Une capture d’écran ou exemple de sortie
  * Vos réponses à la question 7 (analyse)



<br/>

<br/>


# Correction des exercices


## <h2 id="corr-exo1">Correction – Exercice 1 : Analyse d’un prénom (API agify, genderize, nationalize)</h2>

### Objectif

Faire appel à trois APIs publiques pour obtenir des informations liées à un prénom fourni par l’utilisateur.



### 🔧 Script `exo1.py`

```python
import requests

def get_age(name):
    url = f"https://api.agify.io/?name={name}"
    response = requests.get(url)
    return response.json()

def get_gender(name):
    url = f"https://api.genderize.io/?name={name}"
    response = requests.get(url)
    return response.json()

def get_nationalities(name):
    url = f"https://api.nationalize.io/?name={name}"
    response = requests.get(url)
    return response.json()

def afficher_resultats(name):
    try:
        age_data = get_age(name)
        gender_data = get_gender(name)
        nat_data = get_nationalities(name)

        print(f"\nPrénom : {name.capitalize()}")
        print(f"Âge estimé : {age_data.get('age', 'inconnu')} ans")
        print(f"Genre : {gender_data.get('gender', 'inconnu')} (probabilité : {gender_data.get('probability', 0):.2f})")

        print("Nationalités probables :")
        for country in nat_data.get('country', []):
            print(f" - {country['country_id']} ({country['probability']:.2f})")

    except Exception as e:
        print("Erreur lors de la récupération des données :", e)

if __name__ == "__main__":
    nom = input("Entrez un prénom : ").strip().lower()
    afficher_resultats(nom)
```



###  Explications 

* **Trois fonctions dédiées** à chaque API → bonne séparation des responsabilités.
* Utilisation de `.get()` pour éviter les erreurs si une clé est absente.
* Affichage structuré des données : propre et lisible.
* Traitement des exceptions global pour éviter les crashs.



---



## <h2 id="corr-exo2">Correction – Exercice 2 : Universités dans un pays donné</h2>

### Objectif

Consommer l’API `https://universities.hipolabs.com/search?country=XXX` pour lister les universités d’un pays.

---

### 🔧 Script `exo2.py`

```python
import requests

def get_universites(pays):
    url = f"https://universities.hipolabs.com/search?country={pays}"
    response = requests.get(url)
    response.raise_for_status()  # Génère une erreur si le code HTTP est 4xx ou 5xx
    return response.json()

def afficher_universites(liste_univ):
    total = len(liste_univ)
    print(f"\nNombre total d'universités trouvées : {total}\n")
    
    for i, univ in enumerate(liste_univ[:10], 1):  # Afficher seulement les 10 premières
        print(f"{i}. {univ.get('name')}")

if __name__ == "__main__":
    pays = input("Entrez le nom d’un pays (en anglais, ex: Canada) : ").strip()
    
    try:
        universites = get_universites(pays)
        if universites:
            afficher_universites(universites)
        else:
            print("Aucune université trouvée pour ce pays.")
    except requests.exceptions.HTTPError as errh:
        print("Erreur HTTP :", errh)
    except requests.exceptions.RequestException as err:
        print("Erreur de connexion :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```



### Explications 

* `raise_for_status()` est utilisée pour intercepter les erreurs HTTP proprement.
* Affichage limité à 10 universités pour garder la sortie lisible.
* Le script est robuste, gère les erreurs réseau et les entrées incorrectes.
* Utilisation de `.get()` pour éviter les `KeyError`.



----




## <h2 id="corr-exo3">Correction – Exercice 3 : Blague et image d’animal</h2>

### Objectif

1. Afficher une blague de Chuck Norris (API `chucknorris.io`)
2. Télécharger et afficher une image aléatoire de chien (API `dog.ceo`)

---

### 🔧 Script `exo3.py`

```python
import requests
from PIL import Image
from io import BytesIO

def get_blague():
    url = "https://api.chucknorris.io/jokes/random"
    response = requests.get(url)
    return response.json().get("value")

def get_image_url():
    url = "https://dog.ceo/api/breeds/image/random"
    response = requests.get(url)
    data = response.json()
    return data.get("message")  # URL de l'image

def afficher_image_depuis_url(image_url):
    try:
        reponse_image = requests.get(image_url)
        image = Image.open(BytesIO(reponse_image.content))
        image.show()
    except Exception as e:
        print("Erreur lors de l'affichage de l'image :", e)

if __name__ == "__main__":
    try:
        print("\nBlague du jour :")
        print(get_blague())

        print("\nTéléchargement de l’image de chien...")
        url_image = get_image_url()
        print("Image URL :", url_image)

        afficher_image_depuis_url(url_image)

    except requests.exceptions.RequestException as err:
        print("Erreur de connexion :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```



### Explications 

* `PIL` et `BytesIO` sont utilisés pour **afficher l’image depuis une URL sans sauvegarde locale**.
* L’image est ouverte directement en mémoire grâce à `BytesIO`.
* La fonction `image.show()` lance la visionneuse d’images du système (si installée).
* Chaque étape est bien séparée (blague, récupération d’image, affichage).

---

### Remarque importante

Ce script nécessite la **bibliothèque PIL** (ou `Pillow`) :
À installer avec :

```bash
pip install pillow
```



---




## <h2 id="corr-exo4">Correction – Exercice 4 : Faits divers sur les chats</h2>

### Objectif

* Appeler 5 fois l’API `https://catfact.ninja/fact`
* Afficher les faits reçus
* Bonus : les enregistrer dans un fichier texte



### 🔧 Script `exo4.py`

```python
import requests

def get_cat_fact():
    url = "https://catfact.ninja/fact"
    response = requests.get(url)
    data = response.json()
    return data.get("fact")

def enregistrer_faits(faits, nom_fichier="faits_chats.txt"):
    try:
        with open(nom_fichier, "w", encoding="utf-8") as f:
            for i, fait in enumerate(faits, 1):
                f.write(f"{i}. {fait}\n")
        print(f"\nFaits enregistrés dans le fichier : {nom_fichier}")
    except Exception as e:
        print("Erreur lors de l'enregistrement du fichier :", e)

if __name__ == "__main__":
    faits = []
    try:
        print("Faits amusants sur les chats :\n")
        for i in range(5):
            fait = get_cat_fact()
            print(f"{i+1}. {fait}")
            faits.append(fait)

        enregistrer_faits(faits)

    except requests.exceptions.RequestException as err:
        print("Erreur de connexion :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```



###  Explications

* Une boucle `for` permet de faire 5 appels successifs à l’API.
* Utilisation d’une **liste `faits`** pour stocker les réponses et les sauvegarder.
* Le fichier texte est écrit avec encodage `utf-8` pour supporter les caractères spéciaux.
* Le script est robuste aux erreurs (réseau, fichier, etc.).


---




## <h2 id="corr-exo5">Correction – Exercice 5 : Recherche dans les APIs publiques (catégorie Animals)</h2>

### Objectif

* Appeler l’API `https://api.publicapis.org/entries`
* Filtrer les résultats par catégorie (ex. : `"Animals"`)
* Afficher nom, description et lien des APIs trouvées

---

### 🔧 Script `exo5.py`

```python
import requests

def get_apis():
    url = "https://api.publicapis.org/entries"
    response = requests.get(url)
    response.raise_for_status()
    return response.json().get("entries", [])

def filtrer_par_categorie(entries, categorie="Animals"):
    return [api for api in entries if api.get("Category") == categorie]

def afficher_apis(apis):
    if not apis:
        print("Aucune API trouvée dans cette catégorie.")
    for i, api in enumerate(apis, 1):
        print(f"\n{i}. Nom       : {api.get('API')}")
        print(f"   Description : {api.get('Description')}")
        print(f"   Lien        : {api.get('Link')}")

if __name__ == "__main__":
    try:
        print("Récupération des APIs publiques...")
        toutes_les_apis = get_apis()
        apis_animaux = filtrer_par_categorie(toutes_les_apis, "Animals")
        afficher_apis(apis_animaux)

    except requests.exceptions.RequestException as err:
        print("Erreur de connexion :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```



### Explications

* L’ensemble des résultats est obtenu via la clé `"entries"` dans la réponse JSON.
* Une **liste par compréhension** filtre directement les APIs de la catégorie `"Animals"`.
* Les champs `API`, `Description` et `Link` sont extraits un par un avec `.get()` pour éviter les erreurs.
* Le script affiche proprement les résultats.



---



## <h2 id="corr-exo6">Correction – Exercice 6 : Analyse d’un pays (API restcountries)</h2>

### Objectif

* Utiliser `https://restcountries.com/v3.1/all`
* Demander un nom de pays à l’utilisateur
* Afficher : nom, capitale, population, langues, devise, drapeau (URL)

---

### 🔧 Script `exo6.py`

```python
import requests

def get_all_countries():
    url = "https://restcountries.com/v3.1/all"
    response = requests.get(url)
    response.raise_for_status()
    return response.json()

def chercher_pays(nom_pays, liste_pays):
    for pays in liste_pays:
        noms = pays.get("name", {}).get("common", "").lower()
        if nom_pays.lower() == noms:
            return pays
    return None

def afficher_infos(pays):
    nom = pays.get("name", {}).get("common", "N/A")
    capitale = pays.get("capital", ["N/A"])[0]
    population = pays.get("population", 0)
    langues = ", ".join(pays.get("languages", {}).values())
    devises = ", ".join(pays.get("currencies", {}).keys())
    drapeau = pays.get("flags", {}).get("png", "N/A")

    print(f"\nNom : {nom}")
    print(f"Capitale : {capitale}")
    print(f"Population : {population:,}")
    print(f"Langues officielles : {langues}")
    print(f"Devise(s) : {devises}")
    print(f"Drapeau (image) : {drapeau}")

if __name__ == "__main__":
    try:
        nom_recherche = input("Entrez le nom du pays (en anglais, ex: France) : ").strip()
        liste = get_all_countries()
        pays = chercher_pays(nom_recherche, liste)

        if pays:
            afficher_infos(pays)
        else:
            print("Pays non trouvé dans la base.")

    except requests.exceptions.RequestException as err:
        print("Erreur réseau :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```



###  Explications pédagogiques

* `restcountries.com` retourne une **liste de dictionnaires**, chaque dictionnaire représentant un pays.
* Le champ `"languages"` est un dictionnaire → il faut `values()` pour les noms.
* Le champ `"currencies"` est aussi un dictionnaire → on récupère les clés (EUR, USD…).
* Les drapeaux sont accessibles via `"flags"` → on peut utiliser `"png"` pour afficher dans un navigateur si besoin.



----


## <h2 id="corr-exo7">Correction – Exercice 7 : Établissements publics d’un département</h2>

### Objectif

* Utiliser l’API officielle française :
  `https://etablissements-publics.api.gouv.fr/v3/departements/{dep}/communes`
* Afficher les communes et les services publics qui y sont présents.

---

### 🔧 Script `exo7.py`

```python
import requests

def get_communes_et_services(departement):
    url = f"https://etablissements-publics.api.gouv.fr/v3/departements/{departement}/communes"
    response = requests.get(url)
    response.raise_for_status()
    return response.json()  # Liste de communes

def afficher_communes(data):
    if not data:
        print("Aucune commune trouvée.")
        return

    for commune in data:
        nom_commune = commune.get("nom", "Inconnu")
        print(f"\nCommune : {nom_commune}")

        services = commune.get("services", [])
        if not services:
            print("  Aucun service public trouvé.")
        else:
            for service in services:
                print(f"  - {service.get('nom', 'Service inconnu')}")

if __name__ == "__main__":
    departement = input("Entrez un numéro de département (ex: 75 pour Paris) : ").strip()

    try:
        communes = get_communes_et_services(departement)
        afficher_communes(communes)

    except requests.exceptions.HTTPError as errh:
        print("Erreur HTTP :", errh)
    except requests.exceptions.RequestException as err:
        print("Erreur réseau :", err)
    except Exception as e:
        print("Erreur inattendue :", e)
```

---

###  Explications 

* La réponse est une **liste de communes**, chaque commune contient une **liste de services**.
* L’affichage est bien structuré : commune + services associés.
* Le script est entièrement protégé contre les erreurs (réseau, HTTP, structure manquante).
* On utilise `.get()` systématiquement pour éviter tout plantage si une clé est absente.




