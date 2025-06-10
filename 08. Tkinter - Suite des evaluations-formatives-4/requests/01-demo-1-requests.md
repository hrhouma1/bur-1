## <h1 id="demo-api">Démonstration : appel API en Python avec `requests`</h1>

### Objectif

Faire un appel HTTP GET vers l’API GitHub, récupérer une liste d’utilisateurs et afficher leurs identifiants (login) et leurs URLs.

---

## <h2 id="1-code-minimal">1. Code minimal (ligne de commande, sans interface graphique)</h2>

```python
import requests

def get_github_users():
    url = "https://api.github.com/users"

    try:
        response = requests.get(url, timeout=10)  # Appel HTTP
        response.raise_for_status()  # Vérifie que la réponse est valide (code 200)

        users = response.json()  # Convertit le JSON en liste Python

        for user in users:
            print(f"Login : {user['login']} | URL : {user['html_url']}")

    except requests.exceptions.RequestException as e:
        print("Erreur lors de l’appel API :", e)

# Exécution
get_github_users()
```

---

## <h2 id="2-explication">2. Explication ligne par ligne</h2>

| Ligne                               | Explication                                                         |
| ----------------------------------- | ------------------------------------------------------------------- |
| `import requests`                   | Importe la bibliothèque utilisée pour les requêtes HTTP             |
| `url = ...`                         | URL de l’API GitHub qui retourne une liste d’utilisateurs           |
| `requests.get(url, timeout=10)`     | Envoie une requête HTTP GET avec un délai d’attente                 |
| `response.raise_for_status()`       | Déclenche une exception en cas de code HTTP d’erreur                |
| `response.json()`                   | Décode la réponse JSON en structure Python (liste de dictionnaires) |
| `for user in users:`                | Boucle sur les utilisateurs renvoyés                                |
| `user['login']`, `user['html_url']` | Accès aux champs du dictionnaire de chaque utilisateur              |

---

## <h2 id="3-output">3. Exemple de sortie</h2>

```text
Login : mojombo | URL : https://github.com/mojombo
Login : defunkt | URL : https://github.com/defunkt
Login : pjhyett | URL : https://github.com/pjhyett
Login : wycats | URL : https://github.com/wycats
...
```

---

## <h2 id="4-variantes">4. Variantes possibles</h2>

### Appel avec pagination :

```python
url = "https://api.github.com/users?since=100"
```

### Enregistrer les résultats dans un fichier texte :

```python
with open("users.txt", "w", encoding="utf-8") as f:
    for user in users:
        f.write(f"{user['login']} - {user['html_url']}\n")
```

### Extraire uniquement les logins :

```python
logins = [user['login'] for user in users]
print(logins)
```


