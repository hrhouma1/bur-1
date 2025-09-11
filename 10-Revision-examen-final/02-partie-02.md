# Quiz – Threads dans PySide6

### Partie A – Compréhension générale

1. (Vrai/Faux) `QThread` est une classe qui représente un fil d’exécution indépendant.
2. Quelle classe est **plus légère** et ne crée pas un thread complet, mais exécute une tâche ?

   * A. `QThread`
   * B. `QRunnable`
   * C. `QMainWindow`
3. (Réponse courte) Pourquoi dit-on que `QThreadPool` est plus efficace que de créer plusieurs `QThread` séparés ?

<br/>

### Partie B – Cas pratiques

4. (QCM) Si tu dois lancer **des dizaines de petites tâches courtes**, quel choix est le meilleur ?

   * A. Créer un `QThread` pour chaque tâche
   * B. Utiliser `QThreadPool` avec des `QRunnable`
   * C. Tout exécuter dans le thread principal
5. (Réponse courte) Donne un exemple d’utilisation typique d’un `QThread` (longue tâche continue).
6. (Réponse courte) Donne un exemple d’utilisation typique d’un `QRunnable` (petite tâche ponctuelle).

<br/>

### Partie C – Développement

7. (Développement) Explique en quelques phrases comment tu mettrais en place un système qui télécharge plusieurs fichiers en parallèle sans bloquer l’UI.
8. (Développement) Quelles précautions faut-il prendre quand un thread doit mettre à jour l’interface graphique PySide6 ?

