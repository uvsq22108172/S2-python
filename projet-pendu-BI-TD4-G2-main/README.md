import tkinter
import tkinter as tk                                                
from tkinter import *
import random
import pygame

graphique = tk.Tk()                                   
graphique.geometry("1920x1080")                                 
graphique.title("Pendu BI-TD4_Groupe 2")
mainmenu = tkinter.Menu(graphique)

label = Label(text= "Jeu du Pendu")                                     
label.pack(pady=100) 

aide1 = None       #variable globale 

def aide_pendu() :
    global aide1  #référence variable globale
    aide1 = tk.Tk()
    label_aide = Label(aide1, text= "Le but du jeu est simple : deviner toute les lettres qui doivent composer un mot en un nombre limité de tentatives. A chaque fois que le joueur devine une lettre, celle-ci est affichée. Dans le cas contraire, le dessin d'un pendu se met à apparaître…")
    label_aide.pack(pady= 250)
    aide1.geometry("1920x1080")
    bouton_leave = Button(aide1, text="Revenir au menu principale", command= (exit_aide))
    bouton_leave.pack(pady=100)

def exit_aide() :
    global aide1 #référence variable globale
    aide1.destroy()

def jeu_du_pendu() :
    # Fonction pour choisir un mot au hasard

    

    #images = ["C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur1", #images du pendu
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur2", 
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur3", 
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur4", 
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur5", 
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur6", 
    #"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Dessin pendu\erreur7"]

    def choix_du_mot() :
        mots = ['docteur', 'bois', 'touche', 'biologie', 'informatique']
        return random.choice(mots)

    def vérification(lettre, mot, mot_caché) :           #vérifie si la lettre est dans le mot
        for i in range(len(mot)) :
            if mot[i] == lettre :
                mot_caché = mot_caché[:i] + lettre + mot_caché[i+1:]
        return mot_caché

    def montrer_mot_caché(mot_caché) :             #affiche le mot caché
        return ' '.join(mot_caché)

    
    global tentatives_restantes
    
    jeu = tk.Tk()
    jeu.geometry('1920x1080')
    
    label_mot = tk.Label(jeu, text='')
    label_mot.pack(pady=100)

    label_tentatives_restantes = tk.Label(jeu, text='Nombre de tentatives restantes: 7')
    label_tentatives_restantes.pack(pady=50)

    entry_lettre = tk.Entry(jeu)
    entry_lettre.pack()

    bouton_deviner = tk.Button(jeu, text='Deviner')
    bouton_deviner.pack(pady=80)


    global tentatives_restantes, mot_caché
    
    mot = choix_du_mot()
    mot_caché = '*' * len(mot)
    tentatives_restantes = 7

    
    def deviner_une_lettre() :                           #fonction qui gère une tentative "gagnante" ou "perdante" 
        global tentatives_restantes, mot_caché
        lettre = entry_lettre.get().lower()
        entry_lettre.delete(0, tk.END)
        if lettre in mot :
            mot_caché = vérification(lettre, mot, mot_caché)
            label_mot.config(text=montrer_mot_caché(mot_caché))
            if '*' not in mot_caché :
                label_tentatives_restantes.config(text='Vous avez gagné !')
                bouton_deviner.config(state=tk.DISABLED)
        else:
            tentatives_restantes -= 1
            label_tentatives_restantes.config(text=f'Nombre de tentatives restantes: {tentatives_restantes}')
            if tentatives_restantes == 0:
                label_mot.config(text=mot)
                label_tentatives_restantes.config(text='Vous avez perdu !')
                bouton_deviner.config(state=tk.DISABLED)

    #def rejouer() :
        #global mot, mot_caché, tentatives_restantes
        #mot = choix_du_mot()
        #mot_caché = '*' * len(mot)
        #tentatives_restantes = 7
        #label_mot.config(text=montrer_mot_caché(mot_caché))
        #label_tentatives_restantes.config(text=f'Nombre de tentatives restantes: {tentatives_restantes}')
        #bouton_deviner.config(state=tk.NORMAL)
    def rejouer() :
        jeu.destroy()
        jeu_du_pendu()
    bouton_rejouer = tk.Button(jeu, text='Rejouer', command=rejouer)
    bouton_rejouer.pack()      
    

# Configuration du bouton pour appeler la fonction de gestion des tentatives
    bouton_deviner.config(command=deviner_une_lettre)
    
# Affichage du mot caché
    label_mot.config(text=montrer_mot_caché(mot_caché))

    def exit_jeu() :
        jeu.destroy()
    
    bouton_quitter_jeu = Button(jeu, text="Revenir au menu principale", command=(exit_jeu))
    bouton_quitter_jeu.pack(pady= 100)

    
    
    
#---------------------------------------------------------------------------------------------------------------------
# source : https://learntutorials.net/fr/pygame/topic/7419/ajout-de-musique-de-fond-et-d-effets-sonores
def paramètres_button() :
    son = Label(command=change_volume)
    son.pack()

def open_volume_window() :
    volume_window = tk.Toplevel(graphique)
    volume_window.title("Réglage du volume")

def change_volume(volume) :
    pygame.mixer.music.set_volume(volume)
    

def change_vol() :
    volume_var = tk.DoubleVar() #créer une variable qui peut stocker et gérer des valeurs de type flottante
    volume_var.set(0.5) 
    volume_menu = tk.OptionMenu(graphique, volume_var, *[(label, value) for label, value in VOLUME_OPTIONS], command=lambda v: change_volume(v[1]))
    volume_menu.pack(side=LEFT)


# Initialisation de pygame pour jouer de la musique
pygame.mixer.init()
music_file = r"C:\Fac\L1 Semestre 2\IN200N\projet-pendu-BI-TD4-G2-main\Background sonore Pendu\No copyright Song.mp3"
pygame.mixer.music.load(music_file)
pygame.mixer.music.play(-1)      #loop

VOLUME_OPTIONS = [("Mute", 0.0), ("Faible", 0.2), ("Moyen", 0.5), ("Elevé", 1.0)] 


#--------------------------------------------------------------------------------------------------------------------

bouton_play = Button(text="Jouer", command=(jeu_du_pendu))    
bouton_play.pack()                                              

bouton_leave = Button(text="Quitter", command=(quit))
bouton_leave.pack(pady=200)

paramètres = tkinter.Menu(mainmenu)                        
paramètres.add_radiobutton(label= 'Volume Sonore', command=(change_vol))
mainmenu.add_cascade(label= 'Paramètres', menu= paramètres)  

aide = tkinter.Menu(mainmenu)
aide.add_radiobutton(label= 'Aide', command=(aide_pendu))
mainmenu.add_cascade(label= 'Aide', menu= aide)

   

graphique.config(menu=mainmenu)


graphique.mainloop()
