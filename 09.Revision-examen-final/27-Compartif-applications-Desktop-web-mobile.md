# 1. Application Desktop

* **Définition** : Logiciel installé localement sur un ordinateur (Windows, macOS, Linux). Exemple : Microsoft Word, AutoCAD.
* **Avantages** :

  * Performances élevées (accès direct au matériel et aux ressources système).
  * Fonctionne même sans connexion Internet.
  * Intégration plus poussée avec les périphériques locaux (imprimantes, cartes graphiques, etc.).
* **Inconvénients** :

  * Déploiement et mises à jour plus lourds (il faut les installer sur chaque machine).
  * Dépend du système d’exploitation.
* **Sécurité** :

  * Risque lié aux malwares/virus (installation locale).
  * Gestion des mises à jour critiques parfois lente.
  * Accès direct aux fichiers sensibles de l’utilisateur.



# 2. Application Web

* **Définition** : Logiciel accessible via un navigateur (Chrome, Firefox, Safari). Exemple : Gmail, Google Docs.
* **Avantages** :

  * Accessible depuis n’importe quel appareil connecté à Internet.
  * Mises à jour centralisées côté serveur.
  * Multi-plateforme par défaut.
* **Inconvénients** :

  * Dépend de la connexion réseau.
  * Performances parfois limitées par rapport au Desktop.
* **Sécurité** :

  * Surface d’attaque plus large (failles XSS, CSRF, injections SQL).
  * Protection dépend du serveur, du chiffrement (HTTPS), et de la gestion des sessions.
  * Plus facile à sécuriser globalement car tout est centralisé.



# 3. Application Mobile

* **Définition** : Logiciel installé sur smartphone ou tablette (Android, iOS). Exemple : WhatsApp, Uber.
* **Avantages** :

  * Portabilité maximale.
  * Accès natif aux capteurs (GPS, caméra, accéléromètre).
  * Notifications push pour interaction en temps réel.
* **Inconvénients** :

  * Développement séparé selon la plateforme (iOS vs Android), sauf si hybride (React Native, Flutter).
  * Dépend fortement de l’écosystème des stores (Google Play, App Store).
* **Sécurité** :

  * Risques liés aux autorisations excessives (caméra, micro, localisation).
  * Besoin de mises à jour fréquentes.
  * Dépendance aux politiques de sécurité des stores (filtrage mais pas infaillible).



# 4. Comparaison Sécurité (Vue d’ensemble)

| Type        | Risques principaux                                      | Contrôle sécurité                                          |
| ----------- | ------------------------------------------------------- | ---------------------------------------------------------- |
| **Desktop** | Malwares, piratage local, exécution de code malveillant | Antivirus, pare-feu, mises à jour manuelles                |
| **Web**     | Attaques réseau (XSS, CSRF, injections)                 | Sécurisation serveur (HTTPS, WAF, durcissement applicatif) |
| **Mobile**  | Fuites de données via permissions, apps frauduleuses    | Stores officiels, sandboxing, mises à jour rapides         |



# 5. Domaine de la Robotique

Dans le contexte **robotique**, les trois types d’applications peuvent coexister :

* **Desktop** : logiciels de simulation ou de programmation de robots (ex. ROS RViz, Gazebo).
* **Web** : interfaces de contrôle à distance ou dashboards cloud pour suivre l’état d’un robot.
* **Mobile** : applications de pilotage ou de monitoring en temps réel (via Wi-Fi, Bluetooth, 4G/5G).

La **sécurité en robotique** est critique car un robot peut agir physiquement sur l’environnement :

* Risque de prise de contrôle à distance.
* Protection nécessaire des communications (chiffrement, authentification forte).
* Cloisonnement entre la partie logicielle (commandes) et matérielle (action sur moteurs, capteurs).






<br/>


# Comparaison entre Application Desktop, Web et Mobile (avec aspects sécurité et usage en robotique)

| Critère                    | **Application Desktop**                                                 | **Application Web**                                                       | **Application Mobile**                                                            |
| -------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **Définition**             | Logiciel installé localement sur un ordinateur (Windows, Linux, macOS). | Logiciel accessible via un navigateur Internet (Chrome, Firefox, Safari). | Logiciel installé sur smartphone/tablette (Android, iOS).                         |
| **Exemples**               | Microsoft Word, AutoCAD, Photoshop.                                     | Gmail, Google Docs, Trello.                                               | WhatsApp, Uber, Spotify.                                                          |
| **Installation**           | Installation manuelle sur chaque machine.                               | Aucune installation (juste un navigateur).                                | Téléchargement via un store (App Store, Google Play).                             |
| **Accessibilité**          | Limitée à l’appareil où l’app est installée.                            | Accessible depuis n’importe quel appareil connecté à Internet.            | Accessible en mobilité (smartphone, tablette).                                    |
| **Performances**           | Très élevées (utilisation directe des ressources matérielles).          | Moyennes, dépend du navigateur et de la connexion réseau.                 | Optimisées pour l’appareil, mais limitées par la puissance du smartphone.         |
| **Mises à jour**           | À faire manuellement ou via installateurs.                              | Automatiques côté serveur (toujours à jour).                              | Via les stores mobiles, nécessite téléchargement.                                 |
| **Sécurité – Risques**     | Malwares, virus, exécution de code malveillant.                         | Attaques réseau : XSS, CSRF, injections SQL.                              | Permissions abusives (caméra, GPS), apps frauduleuses.                            |
| **Sécurité – Protections** | Antivirus, pare-feu, mises à jour régulières.                           | HTTPS, gestion des sessions, sécurisation serveur (WAF).                  | Stores officiels, sandboxing, authentification forte.                             |
| **Connexion Internet**     | Non obligatoire (hors mises à jour).                                    | Obligatoire (application dépend du cloud).                                | Souvent obligatoire (notifications, données en ligne).                            |
| **Avantages**              | Rapidité, intégration matérielle, pas besoin d’Internet.                | Accessibilité universelle, mise à jour centralisée, multi-plateforme.     | Mobilité, notifications push, intégration capteurs (GPS, caméra, accéléromètre).  |
| **Inconvénients**          | Déploiement lourd, dépend du système d’exploitation.                    | Performances limitées, dépend du réseau.                                  | Développement coûteux (iOS vs Android), dépend des stores.                        |
| **Usage en robotique**     | Logiciels de simulation et de programmation (ROS, Gazebo, Matlab).      | Interfaces Web pour contrôler ou surveiller des robots à distance.        | Applications mobiles pour piloter un robot ou recevoir des alertes en temps réel. |
| **Sécurité en robotique**  | Protéger l’ordinateur de contrôle contre malwares.                      | Sécuriser les communications robot–serveur (chiffrement, VPN).            | Chiffrement des communications mobile–robot, limitation des autorisations.        |



<br/>

# Annexe 1 - Exemples




# 1. Application Desktop

* **Exemple général** :

  * *Photoshop* (édition d’images), *Matlab* (calcul scientifique), *AutoCAD* (CAO).
* **Exemple en robotique** :

  * *ROS RViz* pour visualiser les capteurs d’un robot,
  * *Gazebo* pour simuler les mouvements d’un bras robotisé.
* **Sécurité** :

  * Si l’ordinateur est infecté par un malware, l’attaquant pourrait manipuler directement le logiciel de contrôle du robot.
  * Protection par antivirus, pare-feu, mises à jour système.



# 2. Application Web

* **Exemple général** :

  * *Gmail*, *Google Docs*, *Trello*, accessibles dans un navigateur.
* **Exemple en robotique** :

  * Un *tableau de bord web* qui affiche en temps réel la position d’un robot mobile dans une usine.
  * Un *cloud robotics dashboard* permettant de superviser plusieurs robots connectés à Internet.
* **Sécurité** :

  * Risque de piratage via des attaques web (XSS, CSRF).
  * Protection par HTTPS, authentification forte, pare-feu applicatif (WAF).


# 3. Application Mobile

* **Exemple général** :

  * *WhatsApp* (messagerie), *Uber* (transport), *Spotify* (musique).
* **Exemple en robotique** :

  * Une application mobile qui permet de piloter un drone via Wi-Fi ou 4G.
  * Une application qui envoie une alerte si un robot de surveillance détecte un mouvement suspect.
* **Sécurité** :

  * Risque si l’application demande trop de permissions (caméra, micro, GPS).
  * Protection via sandboxing (isolement des applications), chiffrement des données, et mise à jour régulière depuis les stores officiels.



# Annexe 2 - Quiz – Applications Desktop, Web et Mobile

## Étude de cas 1 – Conception et simulation

Une entreprise de robotique doit simuler les trajectoires d’un bras robotisé en 3D avec une précision millimétrique.
**Question :** Quel type d’application est le plus adapté ?

* A. Application Web
* B. Application Mobile
* C. Application Desktop

**Réponse attendue :** C – Application Desktop (performance locale, intégration matérielle).

---

## Étude de cas 2 – Supervision à distance

Un ingénieur doit surveiller l’état de plusieurs robots déployés dans une usine depuis n’importe quel ordinateur, sans installer de logiciel.
**Question :** Quel type d’application est le plus adapté ?

* A. Application Web
* B. Application Mobile
* C. Application Desktop

**Réponse attendue :** A – Application Web (accessible partout via navigateur).

---

## Étude de cas 3 – Pilotage sur le terrain

Un technicien doit contrôler un drone d’inspection en extérieur et recevoir des alertes instantanées si la batterie est faible.
**Question :** Quel type d’application est le plus adapté ?

* A. Application Web
* B. Application Mobile
* C. Application Desktop

**Réponse attendue :** B – Application Mobile (portabilité, notifications, accès aux capteurs).

---

## Étude de cas 4 – Sécurité des données

Un logiciel de contrôle de robot installé sur PC a été infecté par un virus, entraînant une perte de contrôle du bras robotisé.
**Question :** Quel type d’application est concerné ?

* A. Application Desktop
* B. Application Web
* C. Application Mobile

**Réponse attendue :** A – Application Desktop (exposition aux malwares locaux).

---

## Étude de cas 5 – Collaboration en temps réel

Une équipe internationale doit rédiger en même temps un rapport technique sur les performances d’un robot. Chacun doit pouvoir éditer le document à distance.
**Question :** Quel type d’application est le plus adapté ?

* A. Application Desktop
* B. Application Web
* C. Application Mobile

**Réponse attendue :** B – Application Web (collaboration en ligne en temps réel).

---

## Étude de cas 6 – Maintenance rapide

Un technicien reçoit une alerte et doit rapidement consulter le manuel interactif du robot sur le terrain, directement depuis son téléphone.
**Question :** Quel type d’application est le plus adapté ?

* A. Application Web
* B. Application Mobile
* C. Application Desktop

**Réponse attendue :** B – Application Mobile (portabilité, accès immédiat).


