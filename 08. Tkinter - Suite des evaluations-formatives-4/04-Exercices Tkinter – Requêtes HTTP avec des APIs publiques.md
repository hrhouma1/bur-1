<h1 id="exercices-tkinter-api">Exercices Tkinter – Requêtes HTTP avec des APIs publiques</h1>

> Objectif général : Apprendre à manipuler les requêtes HTTP en Python avec `requests`, et afficher dynamiquement les résultats dans une interface graphique Tkinter.



## Prérequis

Avant de commencer les exercices, installez la bibliothèque `requests` :

```bash
pip install requests
```



## Exercice 1 – Afficher une liste d’utilisateurs GitHub

### API :

```
https://api.github.com/users
```

### Consignes :

* Créez une fenêtre principale avec Tkinter.
* Utilisez `requests.get()` pour récupérer la liste des utilisateurs GitHub.
* Affichez dans un `Listbox` ou dans des `Label` les informations suivantes :

  * Nom d’utilisateur (`login`)
  * Lien vers le profil (`html_url`)
* Ajoutez un bouton "Charger" pour lancer la requête.



## Exercice 2 – Afficher des photos (JSONPlaceholder)

### API :

```
https://jsonplaceholder.typicode.com/photos
```

### Consignes :

* Affichez les 20 premières photos dans une fenêtre Tkinter.
* Pour chaque photo, montrez :

  * Le titre (`title`)
  * La miniature (`thumbnailUrl`) via `PIL.Image` et `requests`
* Utilisez `Canvas` ou `Label` pour afficher les images.



## Exercice 3 – Afficher des articles de blog

### API :

```
https://jsonplaceholder.typicode.com/posts
```

### Consignes :

* Créez une interface avec une `Text` ou `Listbox` pour afficher :

  * Le titre (`title`)
  * Le corps de l’article (`body`)
* Affichez les 10 premiers articles.
* Ajoutez un bouton "Recharger".



## Exercice 4 – Liste de pays avec leur drapeau

### API :

```
https://restcountries.com/v3.1/all
```

### Consignes :

* Récupérez la liste des pays.
* Affichez :

  * Le nom (`name.common`)
  * Le drapeau (`flags.png`) dans un `Label` image
* Affichez un pays à la fois avec bouton "Suivant".



## Exercice 5 – Afficher des faits sur les chats

### API :

```
https://catfact.ninja/facts
```

### Consignes :

* Affichez 10 faits dans une interface Tkinter.
* Pour chaque fait, montrez :

  * Le texte (`fact`)
  * La longueur (`length`)
* Ajoutez un bouton "Nouvelle requête".


