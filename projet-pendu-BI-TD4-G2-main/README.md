import tkinter as tk
 
def afficher_regles():
    """la fonction commence lorsque on actionne le bouton aide"""
    regles = "Les règles du pendu sont..."
  
    tk.messagebox.showinfo("Règles du pendu", regles)
 
fenetre = tk.Tk()
 
bouton_aide = tk.Button(fenetre, text="Aide", command=afficher_regles)
bouton_aide.pack()
 
fenetre.mainloop()