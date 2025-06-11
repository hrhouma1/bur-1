<h1 id="examen-api-python">Démo3 – Consommation d’APIs publiques avec Python</h1>

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



# Correction de la démo 3 – Examen  API Python (version `.py`)

```python
import requests
import json

def separateur():
    print("=" * 50)

# Question 1 – Établissements publics du département 53
def question1_etablissements():
    print("Question 1 – Établissements publics dans le département 53")
    url = "https://etablissements-publics.api.gouv.fr/v3/departements/53/cci"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        etablissements = data.get("features", [])
        for etab in etablissements:
            props = etab.get("properties", {})
            print(f"Nom : {props.get('nom')}")
            print(f"Adresse : {props.get('adresse')}")
            print(f"Ville : {props.get('ville')}")
            print(f"Code Postal : {props.get('codePostal')}")
            print("-" * 40)
        print(f"Total d'établissements : {len(etablissements)}")
    except Exception as e:
        print("Erreur lors de l'appel API :", e)
    separateur()

# Question 2 – Pays d'Europe
def question2_pays_europe():
    print("Question 2 – Pays du continent Europe")
    url = "https://restcountries.com/v3.1/all"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        for pays in data:
            if pays.get("region") == "Europe":
                print(f"Nom : {pays.get('name', {}).get('common')}")
                print(f"Capitale : {pays.get('capital', ['N/A'])[0]}")
                print(f"Population : {pays.get('population')}")
                print("-" * 40)
    except Exception as e:
        print("Erreur API :", e)
    separateur()

# Question 3 – Commentaires du post 1
def question3_commentaires():
    print("Question 3 – Commentaires du postId=1")
    url = "https://jsonplaceholder.typicode.com/comments?postId=1"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        for com in data:
            print(f"Auteur : {com['name']}")
            print(f"Email : {com['email']}")
            print(f"Contenu : {com['body']}")
            print("-" * 40)
        print(f"Total de commentaires : {len(data)}")
    except Exception as e:
        print("Erreur API :", e)
    separateur()

# Question 4 – Citations courtes aléatoires
def question4_citations():
    print("Question 4 – Citations aléatoires (maxLength=20)")
    for i in range(3):
        url = "https://api.quotable.io/random?maxLength=20"
        try:
            response = requests.get(url)
            response.raise_for_status()
            data = response.json()
            print(f"{i+1}. \"{data['content']}\" — {data['author']}")
        except Exception as e:
            print("Erreur API :", e)
    separateur()

# Question 6 – Prix du Bitcoin
def question6_bitcoin():
    print("Question 6 – Prix du Bitcoin en CAD")
    url = "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=cad"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        print("Prix du Bitcoin (CAD) :", data['bitcoin']['cad'], "CAD")
    except Exception as e:
        print("Erreur API :", e)
    separateur()

# Exécution de toutes les questions
if __name__ == "__main__":
    question1_etablissements()
    question2_pays_europe()
    question3_commentaires()
    question4_citations()
    question6_bitcoin()
```

---

## 📁 Fichier `README.md` attendu


# Examen – APIs publiques avec Python

## APIs utilisées

1. API Établissements publics (https://etablissements-publics.api.gouv.fr)
2. API RestCountries v3 (https://restcountries.com)
3. API JSONPlaceholder (https://jsonplaceholder.typicode.com)
4. API Quotable (https://api.quotable.io)
5. API CoinGecko (https://api.coingecko.com)

## Résultats attendus

- Établissements publics du 53 correctement listés avec adresse et ville.
- Liste complète des pays d'Europe avec capitale et population.
- Commentaires JSONPlaceholder du postId=1.
- Trois citations courtes et aléatoires (20 caractères max).
- Prix actuel du Bitcoin en dollars canadiens.

## Analyse finale

1. **API la plus simple :** Quotable – une seule réponse propre, pas de tableau.
2. **API la plus complexe :** RestCountries – structure imbriquée et variations de format.
3. **Erreurs rencontrées :**
   - Certaines APIs peuvent échouer sans Internet ou sans clé (`appid`).
   - Structures JSON parfois imprévues.
