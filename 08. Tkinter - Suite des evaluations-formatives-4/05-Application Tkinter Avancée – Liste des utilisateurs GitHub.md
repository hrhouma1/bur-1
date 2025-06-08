<h1 id="app-tkinter-avancee">Application Tkinter Avancée – Liste des utilisateurs GitHub</h1>


## Objectif

Créer une application graphique qui interroge l’API publique `https://api.github.com/users`, et affiche dans une interface Tkinter :

* Le **nom d'utilisateur GitHub**
* L’**avatar** de l'utilisateur
* Le **lien vers le profil**
* Le tout dans une interface **scrollable**



## Étape 1 – Création de l’environnement virtuel

```bash
mkdir app_utilisateurs_avance
cd app_utilisateurs_avance

python -m venv venv
venv\Scripts\activate
```



## Étape 2 – Fichier `requirements.txt`

```txt
requests
pillow
```

Installe les dépendances avec :

```bash
pip install -r requirements.txt
```



## Étape 3 – Code complet `app.py`

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
from io import BytesIO
from PIL import Image, ImageTk

class AppUtilisateursAvance:
    def __init__(self, root):
        self.root = root
        self.root.title("Utilisateurs GitHub")
        self.root.geometry("600x500")

        self.frame = ttk.Frame(root, padding=10)
        self.frame.pack(fill=tk.BOTH, expand=True)

        self.btn_charger = ttk.Button(self.frame, text="Charger les utilisateurs", command=self.fetch_users)
        self.btn_charger.pack(pady=5)

        self.canvas = tk.Canvas(self.frame)
        self.scrollbar = ttk.Scrollbar(self.frame, orient="vertical", command=self.canvas.yview)
        self.scrollable_frame = ttk.Frame(self.canvas)

        self.scrollable_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )

        self.canvas.create_window((0, 0), window=self.scrollable_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.scrollbar.set)

        self.canvas.pack(side="left", fill="both", expand=True)
        self.scrollbar.pack(side="right", fill="y")

        self.label_status = ttk.Label(self.frame, text="")
        self.label_status.pack(pady=5)

        self.photos = []  # pour stocker les images

    def fetch_users(self):
        url = "https://api.github.com/users"
        self.label_status.config(text="Chargement...")
        for widget in self.scrollable_frame.winfo_children():
            widget.destroy()
        self.photos.clear()

        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            users = response.json()

            for user in users:
                user_frame = ttk.Frame(self.scrollable_frame, padding=5)
                user_frame.pack(fill=tk.X, pady=2)

                # Avatar
                avatar_url = user['avatar_url']
                avatar_img = Image.open(BytesIO(requests.get(avatar_url).content)).resize((50, 50))
                photo = ImageTk.PhotoImage(avatar_img)
                self.photos.append(photo)
                avatar_label = ttk.Label(user_frame, image=photo)
                avatar_label.pack(side=tk.LEFT)

                # Infos
                info = f"{user['login']} - {user['html_url']}"
                label = ttk.Label(user_frame, text=info, wraplength=400, justify="left")
                label.pack(side=tk.LEFT, padx=10)

            self.label_status.config(text=f"{len(users)} utilisateurs chargés.")

        except requests.exceptions.RequestException as e:
            self.label_status.config(text="Erreur lors du chargement.")
            messagebox.showerror("Erreur", str(e))


if __name__ == "__main__":
    root = tk.Tk()
    app = AppUtilisateursAvance(root)
    root.mainloop()
```



## Résultat attendu

* Liste scrollable d’utilisateurs GitHub
* Avatar + nom d’utilisateur + lien profil
* Bouton pour lancer la requête
* Gestion des erreurs via `messagebox`


# Étape 4 -❓ Question conceptuelle avec expérimentation

> En exécutant l'application suivante, vous observez que l'interface Tkinter devient temporairement non réactive (les boutons ne répondent plus, l'affichage est figé) pendant le chargement des utilisateurs GitHub.
>
> **Question :**
>
> **Pourquoi l’interface se bloque-t-elle ?**
>
> a) Parce que `Tkinter` ne permet pas de gérer des images à distance
> b) Parce que `requests.get()` est trop rapide pour être visible
> c) Parce que la requête réseau est exécutée dans le même thread que l’interface
> d) Parce que la méthode `fetch_users()` est vide

> Justifiez votre réponse en expliquant comment Tkinter gère les événements et les appels longs. Proposez une solution technique pour éviter ce blocage.




## Réponse : 

- Dans une application Tkinter, toutes les actions (affichage, clics, redimensionnement, etc.) sont gérées par une **boucle principale appelée "mainloop"**, qui tourne sur **un seul thread**. Lorsque vous effectuez une opération longue ou bloquante dans ce même thread – comme une **requête réseau avec `requests.get()`** – Tkinter **interrompt temporairement l’affichage**, car il attend la fin de cette opération avant de reprendre le contrôle de l’interface. Cela signifie que l'utilisateur ne peut ni interagir avec la fenêtre, ni voir les changements (comme une animation de chargement) tant que la requête n’est pas terminée. C’est ce qu’on appelle un **blocage de l’interface utilisateur (UI freeze)**. Pour éviter cela, il est nécessaire d’exécuter les appels réseau dans un **thread séparé** ou via des techniques asynchrones afin que l'interface reste réactive.



# Étape 5 - démonstration de l'appel bloquant



- Ce code montre **que l'interface Tkinter se fige** pendant l’appel à l’API et au chargement des images.
- Ce code contient un **bouton "Compter"** pour démontrer que l’interface est bloquée pendant le chargement.



##  Code complet `app.py` (appel bloquant)

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
from io import BytesIO
from PIL import Image, ImageTk

class AppUtilisateursAvance:
    def __init__(self, root):
        self.root = root
        self.root.title("Utilisateurs GitHub – Appel bloquant")
        self.root.geometry("600x550")

        self.frame = ttk.Frame(root, padding=10)
        self.frame.pack(fill=tk.BOTH, expand=True)

        # Bouton qui fige l’UI
        self.btn_charger = ttk.Button(self.frame, text="Charger les utilisateurs (bloquant)", command=self.fetch_users)
        self.btn_charger.pack(pady=5)

        # Bouton interactif pour démonstration
        self.compteur = 0
        self.btn_test = ttk.Button(self.frame, text="Compter", command=self.incrementer)
        self.btn_test.pack(pady=5)
        self.label_test = ttk.Label(self.frame, text="Compteur : 0")
        self.label_test.pack(pady=5)

        # Zone scrollable
        self.canvas = tk.Canvas(self.frame)
        self.scrollbar = ttk.Scrollbar(self.frame, orient="vertical", command=self.canvas.yview)
        self.scrollable_frame = ttk.Frame(self.canvas)

        self.scrollable_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )

        self.canvas.create_window((0, 0), window=self.scrollable_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.scrollbar.set)

        self.canvas.pack(side="left", fill="both", expand=True)
        self.scrollbar.pack(side="right", fill="y")

        # Indicateur de statut
        self.label_status = ttk.Label(self.frame, text="")
        self.label_status.pack(pady=5)

        self.photos = []

    def incrementer(self):
        self.compteur += 1
        self.label_test.config(text=f"Compteur : {self.compteur}")

    def fetch_users(self):
        url = "https://api.github.com/users"
        self.label_status.config(text="Chargement...")
        for widget in self.scrollable_frame.winfo_children():
            widget.destroy()
        self.photos.clear()

        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            users = response.json()

            for user in users:
                user_frame = ttk.Frame(self.scrollable_frame, padding=5)
                user_frame.pack(fill=tk.X, pady=2)

                # Avatar
                avatar_url = user['avatar_url']
                avatar_img = Image.open(BytesIO(requests.get(avatar_url).content)).resize((50, 50))
                photo = ImageTk.PhotoImage(avatar_img)
                self.photos.append(photo)
                avatar_label = ttk.Label(user_frame, image=photo)
                avatar_label.pack(side=tk.LEFT)

                # Infos
                info = f"{user['login']} - {user['html_url']}"
                label = ttk.Label(user_frame, text=info, wraplength=400, justify="left")
                label.pack(side=tk.LEFT, padx=10)

            self.label_status.config(text=f"{len(users)} utilisateurs chargés.")

        except requests.exceptions.RequestException as e:
            self.label_status.config(text="Erreur lors du chargement.")
            messagebox.showerror("Erreur", str(e))


if __name__ == "__main__":
    root = tk.Tk()
    app = AppUtilisateursAvance(root)
    root.mainloop()
```



##  À tester

1. Lance l’application.
2. Clique plusieurs fois sur **"Compter"** → ça fonctionne.
3. Clique ensuite sur **"Charger les utilisateurs (bloquant)"**.
4. Essaie de recliquer sur **"Compter"** → **l’interface est figée** jusqu’à la fin du chargement des avatars.





<br/>





## ✅ Code avec compteur interactif (et blocage partiel démontrable)

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
from io import BytesIO
from PIL import Image, ImageTk
import threading

class AppUtilisateursInteractif:
    def __init__(self, root):
        self.root = root
        self.root.title("Utilisateurs GitHub – Démo Blocage Partiel")
        self.root.geometry("600x600")

        self.frame = ttk.Frame(root, padding=10)
        self.frame.pack(fill=tk.BOTH, expand=True)

        # Bouton pour charger
        self.btn_charger = ttk.Button(self.frame, text="Charger les utilisateurs", command=self.lancer_requete)
        self.btn_charger.pack(pady=5)

        # Compteur interactif
        self.compteur = 0
        self.btn_compter = ttk.Button(self.frame, text="Compter", command=self.incremente_compteur)
        self.btn_compter.pack(pady=5)

        self.label_compteur = ttk.Label(self.frame, text="Compteur : 0")
        self.label_compteur.pack(pady=5)

        # Zone scrollable
        self.canvas = tk.Canvas(self.frame)
        self.scrollbar = ttk.Scrollbar(self.frame, orient="vertical", command=self.canvas.yview)
        self.scrollable_frame = ttk.Frame(self.canvas)

        self.scrollable_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )

        self.canvas.create_window((0, 0), window=self.scrollable_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.scrollbar.set)

        self.canvas.pack(side="left", fill="both", expand=True)
        self.scrollbar.pack(side="right", fill="y")

        self.label_status = ttk.Label(self.frame, text="")
        self.label_status.pack(pady=5)

        self.photos = []

    def incremente_compteur(self):
        self.compteur += 1
        self.label_compteur.config(text=f"Compteur : {self.compteur}")

    def lancer_requete(self):
        thread = threading.Thread(target=self.fetch_and_prepare_data)
        thread.start()

    def fetch_and_prepare_data(self):
        self.root.after(0, lambda: self.label_status.config(text="Chargement..."))
        self.root.after(0, lambda: self.clear_users())
        url = "https://api.github.com/users"

        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            users = response.json()

            prepared = []

            for user in users:
                login = user['login']
                html_url = user['html_url']
                avatar_url = user['avatar_url']

                avatar_response = requests.get(avatar_url, timeout=10)
                avatar_image = Image.open(BytesIO(avatar_response.content)).resize((50, 50))
                avatar_photo = ImageTk.PhotoImage(avatar_image)

                prepared.append({
                    'login': login,
                    'html_url': html_url,
                    'photo': avatar_photo
                })

            self.root.after(0, lambda: self.update_ui(prepared))

        except requests.exceptions.RequestException as e:
            self.root.after(0, lambda: self.label_status.config(text="Erreur lors du chargement."))
            self.root.after(0, lambda: messagebox.showerror("Erreur", str(e)))

    def update_ui(self, users):
        for user in users:
            frame = ttk.Frame(self.scrollable_frame, padding=5)
            frame.pack(fill=tk.X, pady=2)

            self.photos.append(user['photo'])
            avatar_label = ttk.Label(frame, image=user['photo'])
            avatar_label.pack(side=tk.LEFT)

            info = f"{user['login']} - {user['html_url']}"
            label = ttk.Label(frame, text=info, wraplength=400, justify="left")
            label.pack(side=tk.LEFT, padx=10)

        self.label_status.config(text=f"{len(users)} utilisateurs chargés.")

    def clear_users(self):
        for widget in self.scrollable_frame.winfo_children():
            widget.destroy()
        self.photos.clear()

if __name__ == "__main__":
    root = tk.Tk()
    app = AppUtilisateursInteractif(root)
    root.mainloop()
```



##  À tester

1. Clique sur **"Charger les utilisateurs"**
2. Pendant le chargement, clique rapidement sur **"Compter"**
3. Tu verras :

   * Le compteur **fonctionne** au début (chargement JSON)
   * Il **ralentit ou fige temporairement** pendant les **téléchargements et affichage d’avatars**













<br/>

# Étape 6 - Ajout d'un thread

### Rappel :

* Requête API GitHub ([https://api.github.com/users](https://api.github.com/users))
* Affichage des avatars, noms d’utilisateur, liens
* Compteur interactif pour prouver que l’UI n’est pas bloquée
* Téléchargement des images dans un thread secondaire



##  Code complet – `app.py`

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
from io import BytesIO
from PIL import Image, ImageTk
import threading

class AppUtilisateursInteractif:
    def __init__(self, root):
        self.root = root
        self.root.title("Utilisateurs GitHub (Thread Non Bloquant)")
        self.root.geometry("600x550")

        self.frame = ttk.Frame(root, padding=10)
        self.frame.pack(fill=tk.BOTH, expand=True)

        # Bouton pour charger les utilisateurs
        self.btn_charger = ttk.Button(self.frame, text="Charger les utilisateurs", command=self.lancer_requete)
        self.btn_charger.pack(pady=5)

        # Compteur interactif
        self.compteur = 0
        self.btn_compter = ttk.Button(self.frame, text="Compter", command=self.incremente_compteur)
        self.btn_compter.pack(pady=5)

        self.label_compteur = ttk.Label(self.frame, text="Compteur : 0")
        self.label_compteur.pack(pady=5)

        # Zone scrollable
        self.canvas = tk.Canvas(self.frame)
        self.scrollbar = ttk.Scrollbar(self.frame, orient="vertical", command=self.canvas.yview)
        self.scrollable_frame = ttk.Frame(self.canvas)

        self.scrollable_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )

        self.canvas.create_window((0, 0), window=self.scrollable_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.scrollbar.set)

        self.canvas.pack(side="left", fill="both", expand=True)
        self.scrollbar.pack(side="right", fill="y")

        # Statut de chargement
        self.label_status = ttk.Label(self.frame, text="")
        self.label_status.pack(pady=5)

        # Stockage des images pour éviter le garbage collection
        self.photos = []

    def incremente_compteur(self):
        self.compteur += 1
        self.label_compteur.config(text=f"Compteur : {self.compteur}")

    def lancer_requete(self):
        thread = threading.Thread(target=self.fetch_and_prepare_data)
        thread.start()

    def fetch_and_prepare_data(self):
        self.root.after(0, lambda: self.label_status.config(text="Chargement..."))
        self.root.after(0, lambda: self.clear_users())
        url = "https://api.github.com/users"

        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            users = response.json()

            prepared = []

            for user in users:
                login = user['login']
                html_url = user['html_url']
                avatar_url = user['avatar_url']

                avatar_response = requests.get(avatar_url, timeout=10)
                avatar_image = Image.open(BytesIO(avatar_response.content)).resize((50, 50))
                avatar_photo = ImageTk.PhotoImage(avatar_image)

                prepared.append({
                    'login': login,
                    'html_url': html_url,
                    'photo': avatar_photo
                })

            self.root.after(0, lambda: self.update_ui(prepared))

        except requests.exceptions.RequestException as e:
            self.root.after(0, lambda: self.label_status.config(text="Erreur lors du chargement."))
            self.root.after(0, lambda: messagebox.showerror("Erreur", str(e)))

    def update_ui(self, users):
        for user in users:
            frame = ttk.Frame(self.scrollable_frame, padding=5)
            frame.pack(fill=tk.X, pady=2)

            self.photos.append(user['photo'])
            avatar_label = ttk.Label(frame, image=user['photo'])
            avatar_label.pack(side=tk.LEFT)

            info = f"{user['login']} - {user['html_url']}"
            label = ttk.Label(frame, text=info, wraplength=400, justify="left")
            label.pack(side=tk.LEFT, padx=10)

        self.label_status.config(text=f"{len(users)} utilisateurs chargés.")

    def clear_users(self):
        for widget in self.scrollable_frame.winfo_children():
            widget.destroy()
        self.photos.clear()

if __name__ == "__main__":
    root = tk.Tk()
    app = AppUtilisateursInteractif(root)
    root.mainloop()
```



## À tester

* Lance le script (`python app.py`)
* Clique sur **"Charger les utilisateurs"**
* Pendant le chargement, clique plusieurs fois sur **"Compter"**
* Le compteur doit continuer d’augmenter pendant le chargement → **preuve que l’interface n’est pas bloquée**


<br/>

# Étape 7 -❓ Question conceptuelle avec expérimentation

> En exécutant l'application, vous pensez que le problème est résolu ?




**Réponse courte :**
Oui, **ce code est *partiellement bloquant*** — il utilise un `thread` pour éviter de bloquer **la requête API principale**, **mais il reste une zone critique bloquante** : le **téléchargement des avatars** est exécuté dans **le thread principal**, via `root.after(...)`.



##  Détails techniques :

###  Ce qui est bien :

* Tu exécutes `fetch_and_prepare_data()` dans un `thread`, ce qui évite de bloquer l’UI pendant l’appel à `https://api.github.com/users`.

### ❌ Ce qui bloque encore :

* Tu appelles **dans le thread principal** (via `root.after`) une fonction qui :

  ```python
  requests.get(avatar_url)
  Image.open(...)
  ImageTk.PhotoImage(...)
  ```

  Donc, **le téléchargement et le traitement d’images se fait dans le thread principal**, ce qui **bloque l’interface quelques millisecondes par avatar** (et c’est visible si tu en charges plusieurs).



## Diagnostic simple :

> Oui, la requête JSON (la liste des utilisateurs) est bien **non bloquante**
> Mais les appels `requests.get(avatar_url)` (pour charger les images) sont **encore bloquants** car **effectués dans le thread principal** via `root.after`.



##  Test à faire :

1. Lance l’app
2. Clique sur **"Charger les utilisateurs"**
3. Clique rapidement sur **"Compter"**
4. Tu vas constater que :

   * le compteur fonctionne pendant l’appel initial à l’API
   * puis **ralentit ou se fige pendant quelques instants**, au fur et à mesure du chargement des avatars.


##  Pour le corriger complètement :

Déplace **toute la logique de téléchargement des avatars et de `Image.open(...)`** dans le thread secondaire, **puis seulement les `ttk.Label(...)` dans `root.after()`**.



<br/>

# Étape 8 - Ajout d'un thread




Voici maintenant la **version corrigée à 100 %**, où :

* **Toute** la logique lente (requêtes JSON **et** avatars, traitement d’images) est exécutée dans un **thread secondaire**
* Seul l’appel à `update_ui(...)` est exécuté dans le thread principal via `root.after(...)`
* L’interface Tkinter reste **parfaitement fluide du début à la fin**

---

## ✅ Code final non bloquant – `app.py`

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
from io import BytesIO
from PIL import Image, ImageTk
import threading

class AppUtilisateursThread:
    def __init__(self, root):
        self.root = root
        self.root.title("Utilisateurs GitHub – Non bloquant 100 %")
        self.root.geometry("600x600")

        self.frame = ttk.Frame(root, padding=10)
        self.frame.pack(fill=tk.BOTH, expand=True)

        self.btn_charger = ttk.Button(self.frame, text="Charger les utilisateurs", command=self.lancer_thread)
        self.btn_charger.pack(pady=5)

        self.compteur = 0
        self.btn_compter = ttk.Button(self.frame, text="Compter", command=self.incrementer)
        self.btn_compter.pack(pady=5)
        self.label_compteur = ttk.Label(self.frame, text="Compteur : 0")
        self.label_compteur.pack(pady=5)

        self.canvas = tk.Canvas(self.frame)
        self.scrollbar = ttk.Scrollbar(self.frame, orient="vertical", command=self.canvas.yview)
        self.scrollable_frame = ttk.Frame(self.canvas)

        self.scrollable_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )

        self.canvas.create_window((0, 0), window=self.scrollable_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.scrollbar.set)

        self.canvas.pack(side="left", fill="both", expand=True)
        self.scrollbar.pack(side="right", fill="y")

        self.label_status = ttk.Label(self.frame, text="")
        self.label_status.pack(pady=5)

        self.photos = []

    def incrementer(self):
        self.compteur += 1
        self.label_compteur.config(text=f"Compteur : {self.compteur}")

    def lancer_thread(self):
        thread = threading.Thread(target=self.fetch_all_in_thread)
        thread.start()

    def fetch_all_in_thread(self):
        self.root.after(0, lambda: self.label_status.config(text="Chargement..."))
        self.root.after(0, self.clear_users)

        url = "https://api.github.com/users"

        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            users = response.json()

            prepared = []

            for user in users:
                login = user['login']
                html_url = user['html_url']
                avatar_url = user['avatar_url']

                avatar_response = requests.get(avatar_url, timeout=10)
                avatar_image = Image.open(BytesIO(avatar_response.content)).resize((50, 50))
                avatar_photo = ImageTk.PhotoImage(avatar_image)

                prepared.append({
                    'login': login,
                    'html_url': html_url,
                    'photo': avatar_photo
                })

            self.root.after(0, lambda: self.update_ui(prepared))

        except requests.exceptions.RequestException as e:
            self.root.after(0, lambda: self.label_status.config(text="Erreur réseau."))
            self.root.after(0, lambda: messagebox.showerror("Erreur", str(e)))

    def update_ui(self, users):
        self.clear_users()
        for user in users:
            frame = ttk.Frame(self.scrollable_frame, padding=5)
            frame.pack(fill=tk.X, pady=2)

            self.photos.append(user['photo'])  # garder référence
            avatar_label = ttk.Label(frame, image=user['photo'])
            avatar_label.pack(side=tk.LEFT)

            info = f"{user['login']} - {user['html_url']}"
            label = ttk.Label(frame, text=info, wraplength=400, justify="left")
            label.pack(side=tk.LEFT, padx=10)

        self.label_status.config(text=f"{len(users)} utilisateurs chargés.")

    def clear_users(self):
        for widget in self.scrollable_frame.winfo_children():
            widget.destroy()
        self.photos.clear()

if __name__ == "__main__":
    root = tk.Tk()
    app = AppUtilisateursThread(root)
    root.mainloop()
```

---

## ✅ Résultat attendu

* Interface **fluide et réactive**
* Le bouton **"Compter"** fonctionne parfaitement **même pendant tout le chargement**
* Aucun gel visible : tout est dans un thread sauf l'affichage graphique final




