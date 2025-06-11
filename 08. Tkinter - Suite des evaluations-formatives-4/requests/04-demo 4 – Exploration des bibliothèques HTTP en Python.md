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

---

### <h2 id="correction-souhaitee">Souhaitez-vous la correction ?</h2>

Je peux vous fournir :

* Un script complet `requests` (partie 1)
* Un script complet `aiohttp` (partie 2)
* Un exemple de comparaison (partie 3)
* Une mesure du temps pour la partie bonus


