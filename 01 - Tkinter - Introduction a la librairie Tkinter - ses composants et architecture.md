# Introduction à la librairie Tkinter

## 1. Qu'est-ce que Tkinter ?

**Tkinter** est la bibliothèque standard de Python pour créer des interfaces graphiques (GUI – Graphical User Interfaces).  
Elle repose sur la bibliothèque **Tk**, qui est multiplateforme, légère et intégrée à la distribution officielle de Python.

- **Langage :** Python
- **Bibliothèque sous-jacente :** Tk
- **Avantages :**
  - Aucune installation supplémentaire
  - Compatible Windows, Linux et macOS
  - Simple pour les interfaces de base


## 2. Objectifs de Tkinter

Tkinter permet de :

- Créer des fenêtres, boîtes de dialogue, menus
- Gérer des entrées utilisateur (texte, clics, choix dans une liste)
- Afficher des éléments dynamiques (étiquettes, images, graphiques)
- Organiser des interfaces en grilles ou avec des conteneurs (Frame)
- Lier des actions aux événements (clavier, souris)



## 3. Principaux composants (widgets)

Voici les **widgets** de base disponibles avec Tkinter :

| Widget         | Rôle                                                                 |
|----------------|----------------------------------------------------------------------|
| `Label`        | Affiche un texte ou une image                                        |
| `Button`       | Déclenche une action (clic)                                          |
| `Entry`        | Champ de saisie à une ligne                                          |
| `Text`         | Champ de texte multiligne                                            |
| `Frame`        | Conteneur pour organiser d’autres widgets                           |
| `Canvas`       | Zone de dessin pour formes, graphiques, images                      |
| `Checkbutton`  | Case à cocher                                                        |
| `Radiobutton`  | Bouton radio (choix exclusif dans un groupe)                        |
| `Listbox`      | Liste d’éléments sélectionnables                                     |
| `Menu`         | Menus déroulants et barres de menu                                   |
| `Scale`        | Curseur pour sélectionner une valeur numérique                      |
| `Scrollbar`    | Barre de défilement verticale ou horizontale                        |
| `ttk`          | Ensemble de widgets modernisés pour un look plus professionnel      |



## 4. Architecture de base d'une application Tkinter

1. Création de la **fenêtre principale** (`tk.Tk()`)
2. Déclaration des **widgets**
3. Positionnement avec `pack()`, `grid()` ou `place()`
4. Définition des **fonctions** associées aux actions
5. Appel de la boucle principale (`mainloop()`)



## 5. Cas d’usage typiques

Tkinter est adapté pour :

- Applications bureautiques simples
- Interfaces de saisie de données
- Outils de configuration
- Petites interfaces d’administration locale
- Outils pédagogiques ou de démonstration (simulateurs, calculatrices, etc.)



## 6. Avantages et limites

### Avantages :
- Simple à apprendre pour les débutants
- Inclus dans Python (aucune installation)
- Suffisant pour des projets personnels ou éducatifs

### Limites :
- Interface vieillissante sans personnalisation poussée
- Moins adapté aux interfaces modernes complexes (préférer PyQt, Kivy ou Electron dans ce cas)
- Pas conçu pour les applications mobiles ou web



## 7. Exemple de projet réalisable avec Tkinter

- Calculatrice graphique
- Gestionnaire de tâches
- Formulaire d’inscription avec base de données SQLite
- Visualiseur de fichiers CSV
- Application de dessin avec Canvas
- Mini-jeux éducatifs (quiz, pendu, etc.)
