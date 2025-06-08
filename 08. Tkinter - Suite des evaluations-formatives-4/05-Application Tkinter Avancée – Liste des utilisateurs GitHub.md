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

# Étape 6 - Ajout d'un thread






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




