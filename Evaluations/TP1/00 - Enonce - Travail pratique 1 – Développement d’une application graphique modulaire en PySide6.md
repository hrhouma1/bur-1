<h1 id="tp1-enonce">Travail pratique 1 – Développement d’une application graphique modulaire en PySide6</h1>

## Contexte pédagogique

Vous êtes mandaté en tant que développeur junior dans une entreprise spécialisée dans la conception d’outils de gestion multiplateformes. Votre rôle est de concevoir une **application graphique complète** permettant de gérer des **utilisateurs** et des **modules**.

Cette application servira d'outil administratif interne pour une organisation fictive appelée **NeoSavoir**.

---

## Objectif général

Développer une **application graphique complète et fonctionnelle** en Python avec **PySide6** et **Qt Designer**, incluant plusieurs interfaces liées, une **base de données SQLite persistante**, un système de navigation dynamique, et des **dialogues modaux de gestion de données**.

---

## Objectifs spécifiques

1. Concevoir des interfaces avec **Qt Designer**
2. Convertir les interfaces `.ui` en modules Python exploitables
3. Implémenter une logique de navigation multi-vues (`QStackedWidget`)
4. Intégrer une base de données relationnelle SQLite
5. Permettre l’ajout dynamique de données via des dialogues modaux
6. Valider les champs et afficher des messages d’erreur clairs
7. Organiser le projet de façon professionnelle et modulaire
8. Générer un exécutable Windows autonome

---

## Scénario à réaliser

Vous devez livrer une **application de gestion** contenant les fonctionnalités suivantes :

### 1. Écran de connexion

* Champs : nom d’utilisateur, mot de passe
* Validation d’un compte administrateur fixe (`admin` / `admin`)
* Message d’erreur en cas d’identifiants incorrects

### 2. Tableau de bord principal

* Menu latéral avec 3 boutons : **Utilisateurs**, **Modules**, **Déconnexion**
* Zone centrale utilisant `QStackedWidget` pour afficher les pages

### 3. Page “Utilisateurs”

* Tableau listant les utilisateurs (ID, nom, courriel)
* Bouton `Ajouter un utilisateur`
* Dialogue modal pour saisir nom + courriel
* Validation obligatoire + format du courriel
* Insertion réelle dans la base de données SQLite

### 4. Page “Modules”

* Tableau listant les modules (ID, titre, responsable)
* Bouton `Ajouter un module`
* Dialogue modal pour saisir le titre + responsable
* Insertion réelle dans la base de données SQLite

### 5. Déconnexion

* Ferme le tableau de bord
* Rouvre l’écran de connexion avec champs réinitialisés

---

## Contraintes techniques

* Utiliser uniquement **PySide6** (aucun autre framework GUI n’est accepté)
* Les interfaces doivent être créées dans **Qt Designer** (`.ui`)
* Toutes les interfaces doivent être converties avec `pyside6-uic`
* L’application doit être **fonctionnelle avec une base SQLite**
* Le code doit être organisé en **modules** : `/ui`, `/logic`, `/data`, `/assets`, `/config`
* Une version `.exe` doit être générée avec `PyInstaller`

---

## Organisation du projet attendue

```
projet_pyside6/
├── assets/
├── config/
├── data/
├── logic/
├── ui/
├── ui_gen/
├── main.py
└── README.md
```

---

## Livrables obligatoires

1. Le projet complet compressé au format `.zip`
2. Un fichier `README.md` décrivant :

   * Le but du projet
   * Les étapes d’exécution
   * Les modules fonctionnels
3. Le fichier exécutable `.exe` généré (dans un dossier `dist/`)
4. Le fichier de configuration externe (`config/config.json`)
5. Le fichier `.db` SQLite pré-rempli (optionnel)

---

## Évaluation

| Critère                                   | Pondération |
| ----------------------------------------- | ----------- |
| Fonctionnalité complète (connexion, CRUD) | 40 %        |
| Respect des consignes techniques          | 20 %        |
| Structure et modularité du projet         | 20 %        |
| Qualité du code (lisibilité, validation)  | 10 %        |
| Fichier exécutable généré correctement    | 10 %        |

---

## Consignes importantes

* Aucune bibliothèque graphique autre que **PySide6** n’est autorisée.
* Le plagiat total ou partiel entraîne **l’annulation du TP**.
* Les interfaces `.ui` doivent être **réutilisées via import**, et non recodées manuellement.
* Le projet doit être fonctionnel **localement**, sans connexion Internet.

---

## Remise

* Format attendu : archive `.zip` nommée `TP1_NOM_PRENOM.zip`
* Dépôt sur la plateforme officielle avant **\[date à déterminer]**
* Tout retard entraîne une pénalité selon les règles institutionnelles.

