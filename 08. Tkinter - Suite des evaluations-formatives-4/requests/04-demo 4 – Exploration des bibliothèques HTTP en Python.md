## <h1 id="exercice-api-libs">Demo 4 – Exploration des bibliothèques HTTP en Python</h1>

### <h2 id="objectif">Objectif pédagogique</h2>

Identifier, comparer et expérimenter les principales bibliothèques Python utilisées pour consommer des services web de type API REST, en distinguant les appels **synchrones** des appels **asynchrones**.

---

### <h2 id="consignes">Consignes</h2>

1. Complétez le tableau des bibliothèques.
2. Faites une recherche sur leur mode de fonctionnement.
3. Implémentez au moins une requête `GET` **synchronisée** et une autre **asynchrone**.
4. Comparez le comportement et la syntaxe.

---

### <h2 id="liste-libs">Liste (non exhaustive) des bibliothèques HTTP en Python</h2>

| # | Nom de la bibliothèque | Type d'appel               | URL/documentation officielle                                                                                        |
| - | ---------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| 1 | `requests`             | **Synchrone**              | [https://docs.python-requests.org/](https://docs.python-requests.org/)                                              |
| 2 | `http.client`          | **Synchrone**              | [https://docs.python.org/3/library/http.client.html](https://docs.python.org/3/library/http.client.html)            |
| 3 | `urllib.request`       | **Synchrone**              | [https://docs.python.org/3/library/urllib.request.html](https://docs.python.org/3/library/urllib.request.html)      |
| 4 | `httpx`                | **Synchrone / Asynchrone** | [https://www.python-httpx.org/](https://www.python-httpx.org/)                                                      |
| 5 | `aiohttp`              | **Asynchrone**             | [https://docs.aiohttp.org/en/stable/](https://docs.aiohttp.org/en/stable/)                                          |
| 6 | `treq`                 | **Asynchrone** (Twisted)   | [https://github.com/twisted/treq](https://github.com/twisted/treq)                                                  |
| 7 | `pycurl`               | **Synchrone**              | [https://pycurl.io/docs/latest/](https://pycurl.io/docs/latest/)                                                    |
| 8 | `requests-async`       | **Asynchrone**             | [https://github.com/encode/requests-async](https://github.com/encode/requests-async) (déprécié, remplacé par httpx) |

---

### <h2 id="travail-demande">Travail demandé</h2>

#### Partie 1 – Requête `GET` avec `requests` (synchrone)

Écrivez un script qui utilise `requests` pour appeler :

```txt
https://jsonplaceholder.typicode.com/users
```

Affichez le nom et le courriel des utilisateurs.

---

#### Partie 2 – Requête `GET` avec `aiohttp` (asynchrone)

Écrivez un script équivalent à celui de la partie 1, mais avec `aiohttp` (attention à l’usage de `async/await`).

---

#### Partie 3 – Comparaison

1. Quelle bibliothèque est la plus facile à utiliser ?
2. Pourquoi choisir une bibliothèque asynchrone dans certains cas ?
3. Quelles bibliothèques permettent les deux modes (sync/async) ?
4. Quel est le comportement de chaque script si on exécute plusieurs appels en parallèle (bonus) ?

---

### <h2 id="bonus">Bonus</h2>

Essayez de faire 10 appels à l’API `https://api.quotable.io/random` avec `requests` et avec `aiohttp`, puis mesurez le **temps d’exécution total** avec `time.time()` pour comparer les performances.



<br/>

<br/>





## <h1 id="correction-api-libs">Correction – Consommation d’API avec bibliothèques synchrones et asynchrones en Python</h1>



### <h2 id="partie1-requests">Partie 1 – Requête GET avec `requests` (synchrone)</h2>

```python
# fichier : get_users_requests.py

import requests

def fetch_users():
    url = "https://jsonplaceholder.typicode.com/users"
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        for user in data:
            print(f"Nom : {user['name']} | Email : {user['email']}")
    else:
        print("Erreur lors de l'appel API")

if __name__ == "__main__":
    fetch_users()
```



### <h2 id="partie2-aiohttp">Partie 2 – Requête GET avec `aiohttp` (asynchrone)</h2>

```python
# fichier : get_users_aiohttp.py

import aiohttp
import asyncio

async def fetch_users():
    url = "https://jsonplaceholder.typicode.com/users"
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            if response.status == 200:
                data = await response.json()
                for user in data:
                    print(f"Nom : {user['name']} | Email : {user['email']}")
            else:
                print("Erreur lors de l'appel API")

if __name__ == "__main__":
    asyncio.run(fetch_users())
```



### <h2 id="partie3-comparaison">Partie 3 – Comparaison</h2>

| Critère                         | `requests`                     | `aiohttp`                              |
| ------------------------------- | ------------------------------ | -------------------------------------- |
| Simplicité d’usage              | Très simple (1 ligne pour GET) | Moins simple (nécessite async/await)   |
| Contexte                        | Idéal pour appels simples      | Idéal pour appels multiples/parallèles |
| Support async                   | Non                            | Oui (natif)                            |
| Performance (beaucoup d'appels) | Plus lent                      | Plus rapide                            |

---

### <h2 id="bonus-performance">Bonus – Performance : 10 appels `GET`</h2>

#### 🟩 Version `requests` (synchronisée, séquentielle)

```python
# fichier : multi_requests.py
import requests
import time

def fetch_multiple():
    start = time.time()
    for i in range(10):
        response = requests.get("https://api.quotable.io/random")
        if response.status_code == 200:
            data = response.json()
            print(f"{i+1}. {data['content']} — {data['author']}")
    end = time.time()
    print(f"Durée totale (requests) : {end - start:.2f} sec")

if __name__ == "__main__":
    fetch_multiple()
```

---

#### 🟦 Version `aiohttp` (asynchrone, concurrente)

```python
# fichier : multi_aiohttp.py
import aiohttp
import asyncio
import time

async def fetch_quote(session, i):
    url = "https://api.quotable.io/random"
    async with session.get(url) as response:
        if response.status == 200:
            data = await response.json()
            print(f"{i+1}. {data['content']} — {data['author']}")

async def main():
    start = time.time()
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_quote(session, i) for i in range(10)]
        await asyncio.gather(*tasks)
    end = time.time()
    print(f"Durée totale (aiohttp) : {end - start:.2f} sec")

if __name__ == "__main__":
    asyncio.run(main())
```

---

### <h2 id="resultats-performance">Résultats typiques de comparaison</h2>

| Méthode    | Durée pour 10 requêtes |
| ---------- | ---------------------- |
| `requests` | \~8 à 10 secondes      |
| `aiohttp`  | \~1 à 2 secondes       |




