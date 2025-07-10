
# **Examen Applications Bureau – Partie 1** (40%)

- https://forms.office.com/r/V2nqiM5WRc


# **Examen Applications Bureau – Partie 2**

<br/>

# **PARTIE 1 – Interface graphique avec Slot** (30%)

### **Objectifs :**

Vous devez concevoir une application graphique minimaliste en utilisant **PySide6**. L’interface doit contenir les éléments suivants :

* Un bouton cliquable visible dès le lancement.
* Un composant visuel de type `QLabel` permettant d'afficher dynamiquement un message à l’écran.
* Un mécanisme de gestion d’événement basé sur les **slots** de PySide6.
* Lorsque l’utilisateur clique sur le bouton, un message doit être affiché dans la zone prévue.



### **Contraintes fonctionnelles personnalisées:**

1. **Titre de la fenêtre** :
   Le titre de la fenêtre doit être **"Examen PySide6 – \[Votre prénom]"**
   → Exemples :
   - `"Examen PySide6 – Haythem"`
   - `"Examen PySide6 – François"`

2. **Message affiché dans le QLabel** :
   Le message affiché au clic doit être :
   - **"Bonjour \[Votre prénom], votre message a bien été enregistré."**

3. **Nom du fichier principal** :
   Le fichier principal contenant le code doit être nommé :
   - **`main_[prenom]_[nom].py`**
   - → Exemple : `main_haythem_rehouma.py`


<br/>

# **PARTIE 2 – Insertion du message en base de données** (20%)

### **Objectifs :**

À chaque clic sur le bouton, vous devez insérer le message affiché dans une base de données relationnelle.



### **Spécifications techniques :**

1. Utilisez **une base de données relationnelle au choix** parmi :

   * SQLite (fichier `.db`)
   * PostgreSQL
   * MySQL
   * **Toute autre base relationnelle avec justificatif**

2. Créez une **table nommée obligatoirement** :
   - **`messages_[nom_de_famille]`**
   → Exemple : `messages_rehouma`

3. La table doit contenir les colonnes suivantes :

    * `id` : identifiant unique, entier, auto-incrémenté
    * `contenu` : texte du message
    * `horodatage` : date et heure d'insertion

4. À chaque clic, insérez dans cette table :

    * Le message affiché
    * La date et l’heure d’exécution

5. La base de données peut être initialisée à l'exécution si elle n’existe pas encore.





<br/>


# **PARTIE 3 – Élément graphique libre avec interaction (slot requis)**(10%)

### **Objectifs :**

Ajoutez un **deuxième composant graphique interactif** à votre interface (autre qu’un bouton), tel que :

* `QComboBox` (menu déroulant)
* `QLineEdit` (champ de texte)
* `QCheckBox`
* `QSlider`
* Ou tout autre **widget de votre choix**.


### **Contraintes fonctionnelles :**

* Ce widget doit être **interactif** et relié à un **slot personnalisé**.
* Ce slot doit produire un **effet visible ou logique** dans l'application (ex. : changer un texte depuis la base de données, afficher une valeur, activer une option, etc.)
* Le comportement doit être clairement expliqué dans le rapport.

**Vous êtes libre de choisir l’usage de ce deuxième élément**, à condition que :

* Il soit relié à un événement utilisateur.
* Il soit traité par une fonction (slot) dans votre code.

### **Résumé et exemple des contraintes fonctionnelles :**

* Ce widget doit être **interactif** et **relié à un slot** personnalisé.
* Le slot doit produire un **effet visible ou logique** dans l'application.
  - Exemples possibles :

  * Afficher ou modifier un texte
  * Activer/désactiver un élément
  * Afficher une valeur dynamique
  * Rechercher une valeur dans la base
* Le comportement doit être **clairement décrit dans le rapport fourni**.




#### **Table à remplir dans le rapport**

| Élément                         | Votre réponse                                      |
| ------------------------------- | -------------------------------------------------- |
| Type de widget utilisé          | *(ex : QComboBox, QLineEdit, etc.)*                |
| Signal déclencheur              | *(ex : currentIndexChanged, textChanged, etc.)*    |
| Nom de la fonction slot appelée | *(ex : afficher\_resultat, mettre\_a\_jour, etc.)* |
| Effet produit                   | *(ex : affiche un message, modifie une valeur)*    |
| Composant affecté               | *(ex : QLabel, QLineEdit, etc.)*                   |
| Base de données utilisée ?      | Oui / Non                                          |
| Si oui : type d'opération       | *(ex : lecture, insertion, requête, etc.)*         |


### **Rappel :**

* Cette partie est **libre dans le choix du widget** mais **obligatoire**.
* Toute absence d’interaction réelle (aucune action liée au widget) ou absence de description dans le rapport = **note nulle pour cette partie**.



<br/>
<br/>




## **Livrables à rendre**

Rendez l’ensemble compressé dans un fichier `.zip` nommé `examen_[prenom]_[nom].zip`.

Le `.zip` doit contenir **obligatoirement** :

1. **Les fichiers Python principaux** (`main_[prenom]_[nom].py`) contenant :

   * L’interface graphique complète
   * Le mécanisme de slot
   * L’insertion en base de données

2. *La base de données utilisée, le script ou un screenshot dans le rapport de la base de données* :

   * Fichier `.db` (pour SQLite)
   * OU export `.sql` (pour MySQL/PostgreSQL)

3. **Un rapport Word** nommé `rapport_[prenom]_[nom].docx` contenant :

   * Votre prénom et nom
   * Le **titre exact de la fenêtre**
   * Le **message affiché** exactement comme dans l'application
   * Le **nom de la table**
   * Capture d’écran de l’application en cours d'exécution (avec message affiché)
   * Capture d’écran de la table de base de données remplie
   * Choix de la base de données
   * Explication du fonctionnement global de votre application

