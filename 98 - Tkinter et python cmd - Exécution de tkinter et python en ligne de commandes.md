# Exécution de tkinter et python en ligne de commandes

- C'est optionnel mais il s'agit  **bloc unique contenant 50 commandes `python -c` Tkinter**, à exécuter directement dans un terminal Linux/Mac/WSL/Windows. 
- Elles utilisent **uniquement la ligne de commande**, sans ouvrir d'éditeur.

---

```bash
python -c "import tkinter as tk; root = tk.Tk(); root.title('Fenêtre simple'); root.geometry('300x200'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Bonjour !').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); entry = tk.Entry(root); entry.pack(); tk.Button(root, text='Valider', command=lambda: print(entry.get())).pack(); root.mainloop()"
python -c "import tkinter as tk; from tkinter import messagebox; root = tk.Tk(); root.withdraw(); messagebox.showinfo('Info', 'Hello depuis le terminal')"
python -c "import tkinter as tk; root = tk.Tk(); root.configure(bg='lightblue'); tk.Label(root, text='Fenêtre bleue', bg='lightblue').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Button(root, text='Fermer', command=root.destroy).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.geometry('200x100+400+300'); tk.Label(root, text='Positionnée !').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); canvas = tk.Canvas(root, width=200, height=100); canvas.pack(); canvas.create_rectangle(50, 20, 150, 80, fill='red'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); canvas = tk.Canvas(root, width=200, height=200); canvas.pack(); canvas.create_oval(50, 50, 150, 150, fill='green'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.attributes('-topmost', True); tk.Label(root, text='Toujours au-dessus').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.attributes('-fullscreen', True); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Scale(root, from_=0, to=100, orient='horizontal').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Checkbutton(root, text='Option A').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); var = tk.StringVar(value='1'); tk.Radiobutton(root, text='Oui', variable=var, value='1').pack(); tk.Radiobutton(root, text='Non', variable=var, value='0').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Message(root, text='Un message multi-ligne automatique', width=200).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Listbox(root).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Listbox(root); l.pack(); [l.insert('end', f'Item {i}') for i in range(10)]; root.mainloop()"
python -c "import tkinter as tk; from tkinter import filedialog; root = tk.Tk(); root.withdraw(); print(filedialog.askopenfilename())"
python -c "import tkinter as tk; root = tk.Tk(); text = tk.Text(root); text.pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Spinbox(root, from_=1, to=10).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Entry(root, show='*').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.iconify(); root.after(2000, root.deiconify); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='Clignote'); l.pack(); import itertools; def blink(): l.config(text=next(cycle)); root.after(500, blink); cycle = itertools.cycle(['🔴','⚪']); blink(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.title('Redimension interdite'); root.resizable(False, False); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Button(root, text='Alerte', command=lambda:print('Clique détecté')).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='Initial'); l.pack(); def update(): l.config(text='Mis à jour'); tk.Button(root, text='Mettre à jour', command=update).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='⌛ Chargement...').pack(); root.after(2000, root.destroy); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); for i in range(5): tk.Label(root, text=f'Label {i}').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Multiligne').pack(); tk.Text(root, height=5, width=30).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); b = tk.Button(root, text='Désactiver'); b.config(state='disabled'); b.pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); c = tk.Canvas(root, width=300, height=150); c.pack(); c.create_line(0, 0, 300, 150); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); c = tk.Canvas(root, width=200, height=200); c.pack(); c.create_arc(50, 50, 150, 150, start=0, extent=150, fill='yellow'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Saisie').grid(row=0, column=0); tk.Entry(root).grid(row=0, column=1); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='Hover moi'); l.pack(); l.bind('<Enter>', lambda e: l.config(text='Survol !')); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.bind('<Escape>', lambda e: root.destroy()); tk.Label(root, text='Appuie sur ESC pour fermer').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); def hello(): print('Hello'); root.bind('<Return>', lambda e: hello()); tk.Entry(root).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='0'); l.pack(); def plus(): l.config(text=str(int(l['text'])+1)); tk.Button(root, text='+1', command=plus).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.title('Fenêtre centrée'); w,h = 300,150; sw,sh = root.winfo_screenwidth(), root.winfo_screenheight(); x,y = (sw-w)//2, (sh-h)//2; root.geometry(f'{w}x{h}+{x}+{y}'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); f = tk.Frame(root, bg='red', width=100, height=50); f.pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='⏳ Fermeture automatique').pack(); root.after(3000, root.destroy); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); def resize(): root.geometry('600x400'); tk.Button(root, text='Agrandir', command=resize).pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.title('Focus'); e = tk.Entry(root); e.pack(); root.after(100, lambda: e.focus()); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Ctrl+C pour quitter').pack(); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); root.configure(cursor='watch'); tk.Label(root, text='Souris spéciale').pack(); root.mainloop()"
python -c "import tkinter as tk; from tkinter import simpledialog; root = tk.Tk(); root.withdraw(); r = simpledialog.askstring('Nom', 'Quel est ton nom ?'); print(r)"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='Scroll'); l.pack(); s = tk.Scrollbar(root); s.pack(side='right', fill='y'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Bouton large').pack(fill='x'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); tk.Label(root, text='Alignement gauche').pack(anchor='w'); root.mainloop()"
python -c "import tkinter as tk; root = tk.Tk(); l = tk.Label(root, text='Invisible'); l.pack(); l.pack_forget(); root.mainloop()"
```
