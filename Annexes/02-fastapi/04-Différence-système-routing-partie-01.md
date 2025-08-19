# Différence entre Next.js (App Router) et FastAPI

La différence technique majeure entre **Next.js (App Router)** et **FastAPI** réside dans la manière dont on déclare les routes d’un service web. Avec **Next.js**, le système repose sur l’**arborescence de fichiers et de dossiers** : chaque fichier `route.ts` placé dans un chemin `app/api/...` devient automatiquement un endpoint. L’URL est donc directement liée à la structure du projet. À l’inverse, **FastAPI** fonctionne avec des **annotations (décorateurs Python)** : une fonction devient un endpoint lorsque l’on ajoute `@app.get("/...")`, `@app.post("/...")`, etc. L’URL n’est pas déterminée par un emplacement dans un répertoire, mais par l’annotation écrite dans le code. Autrement dit, Next.js applique une logique de **convention par fichiers**, tandis que FastAPI applique une logique **déclarative par annotations**.


