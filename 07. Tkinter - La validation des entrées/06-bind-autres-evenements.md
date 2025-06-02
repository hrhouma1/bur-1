# Événements les plus utilisés dans Tkinter, présentée dans un tableau pour une meilleure lisibilité.


##  Tableau des principaux événements Tkinter

| **Événement**                      | **Déclenchement**                                                |
| ---------------------------------- | ---------------------------------------------------------------- |
| `<FocusIn>`                        | Quand le widget reçoit le focus clavier                          |
| `<FocusOut>`                       | Quand le widget perd le focus clavier                            |
| `<Enter>`                          | Quand la souris entre dans la zone du widget                     |
| `<Leave>`                          | Quand la souris sort de la zone du widget                        |
| `<Button-1>`                       | Clic gauche de la souris (bouton 1)                              |
| `<Button-2>`                       | Clic molette (bouton 2)                                          |
| `<Button-3>`                       | Clic droit de la souris (bouton 3)                               |
| `<Double-Button-1>`                | Double-clic gauche                                               |
| `<Triple-Button-1>`                | Triple-clic gauche                                               |
| `<ButtonPress>`                    | Appui sur **n’importe quel** bouton de souris                    |
| `<ButtonRelease>`                  | Relâchement d’un bouton de souris                                |
| `<Key>`                            | Appui sur **n’importe quelle** touche du clavier                 |
| `<KeyPress>` ou `<Key>`            | Identique : touche pressée                                       |
| `<KeyRelease>`                     | Touche relâchée                                                  |
| `<Return>`                         | Touche Entrée pressée                                            |
| `<Tab>`                            | Touche Tabulation                                                |
| `<BackSpace>`                      | Touche Retour arrière (effacer)                                  |
| `<Escape>`                         | Touche Échap                                                     |
| `<Up>` `<Down>` `<Left>` `<Right>` | Flèches directionnelles                                          |
| `<Control-KeyPress-X>`             | Ctrl + X pressé                                                  |
| `<Shift-KeyPress-A>`               | Maj + A pressé                                                   |
| `<Configure>`                      | Quand la taille du widget change                                 |
| `<Expose>`                         | Quand le widget est redessiné                                    |
| `<Destroy>`                        | Quand le widget est détruit                                      |
| `<Motion>`                         | Quand la souris se déplace dans le widget                        |
| `<MouseWheel>`                     | Quand l’utilisateur fait défiler avec la molette (Windows/macOS) |

---

##  Syntaxe générale d’un bind dans Tkinter

```python
widget.bind("<NomEvenement>", fonction)
```

La fonction appelée doit prendre au moins **un argument** (`event`), qui donne accès à :

* `event.keysym` : nom de la touche pressée
* `event.char` : caractère saisi (si applicable)
* `event.x`, `event.y` : position de la souris (pixels)
* `event.widget` : widget concerné
* etc.

---

## Exemple simple d’utilisation

```python
champ = tk.Entry(root)
champ.bind("<FocusIn>", lambda e: print("Champ activé"))
champ.bind("<KeyRelease>", lambda e: print(f"Touche : {e.keysym}"))
champ.bind("<Return>", lambda e: print("Entrée validée"))
```

