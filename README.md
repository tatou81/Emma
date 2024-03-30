# Créé par Emma, le 09/01/2023 en Python 3.7
import random
import tkinter

def jouer():
    global fin,score,victoire,taille,texte,fond,fond2,fond3,case_mines,case_standard,case_standard_2,case_tresor

    fin=False

    score=0
    victoire=75

    taille=12
    grille=[[0 for i in range(taille)] for j in range(taille)]

    #variable des couleurs
    texte="white"
    fond="#5A7302"
    fond2="#83A603"
    fond3="#384001"
    case_standard="#733B0A"
    case_standard_2="#592411"
    case_tresor="#F2B705"
    case_mines="#F20505"

    #fonction qui place les mines et ajoute 1 a toutes les cases voisines
    def mines(grille) :

        for k in range (25):
            i=random.randint(1,taille-2)
            j=random.randint(1,taille-2)


            if grille[i][j]<10 :
                grille[i][j]=10

                #place les +1
                grille[i][j-1]=grille[i][j-1]+1
                grille[i][j+1]=grille[i][j+1]+1
                grille[i+1][j]=grille[i+1][j]+1
                grille[i+1][j+1]=grille[i+1][j+1]+1
                grille[i+1][j-1]=grille[i+1][j-1]+1
                grille[i-1][j]=grille[i-1][j]+1
                grille[i-1][j+1]=grille[i-1][j+1]+1
                grille[i-1][j-1]=grille[i-1][j-1]+1

    #fonction qui permet d'afficher la grille dans le terminal
    def afficherGrille(grille):


        for i in range (taille):
            for j in range (taille):

                if grille[i][j]>9:
                    grille[i][j]= 9

        for ligne in grille:
            print(ligne)

    #fonction qui reagit au click
    def pointeur(event):
        global fin,abscisse,ordonnee,score,victoire
        if fin==False:

            if event :
                tresor=random.randint(0,20)
                abscisse=event.x
                ordonnee=event.y
                #On vérifie que le clic se fait dans une case
                #dessin de la grille
                for x in range(50,300,25):
                    for y in range(50,300,25):

                        if abscisse >x and abscisse < x+25 and ordonnee > y  and ordonnee < y+25 :
                            #Détermination de l'indice de position de la colonne+1 et de la ligne+1
                            #correspondant au clic
                            colonne=(x-50)//25
                            ligne=(y-50)//25

                            #on choisi aleatoirement la couleur des cases cliqués
                            if tresor<20:
                                color=case_standard

                            if tresor<9:
                                color=case_standard_2

                            if tresor==20 :
                                color=case_tresor
                                victoire=victoire+10
                                score=score+10

                            #on cree les case cliqués
                            if fin==False and grille[ligne+1][colonne+1]<7 :
                                espace_dessin.create_rectangle(x,y,x+25,y+25,width=0, fill=color )

                                #on créé un label qui ecrit la valeur de la case
                                label1=tkinter.Label(fenetre,text=grille[ligne+1][colonne+1],font=("Helvetica",10))
                                #Couleur de fond et de texte du label
                                label1.configure(bg=color,width=2,fg=texte)
                                #il faut impérativement indiquer sa position
                                label1.place(x=x+162.5,y=y+112.5, anchor='center')

                                score=score+1
                                result()

                            if fin==False and grille[ligne+1][colonne+1]==9:
                                espace_dessin.create_rectangle(x,y,x+25,y+25,width=0, fill=case_mines)
                                espace_dessin.create_oval(x+5,y+5,x+20,y+20,width=0,fill="#8C0303")

                                fin=True
                                result()

    #fonction qui réagit au click droit
    def drapeau(event) :
        global fin,abscisse,ordonnee,score

        if fin==False:
            if event :
                abscisse=event.x
                ordonnee=event.y
                #Parcours du dessin de la grille
                #On vérifie que le clic se fait dans une case

                for x in range(50,300,25):
                    for y in range(50,300,25):

                        if abscisse >x and abscisse < x+25 and ordonnee > y  and ordonnee < y+25 :
                            colonne=(x-50)//25
                            ligne=(y-50)//25

                            espace_dessin.create_rectangle(x+6,y+5,x+8,y+20,outline=case_mines, fill=case_mines)
                            espace_dessin.create_rectangle(x+4,y+20,x+10,y+22,outline=case_mines, fill=case_mines)
                            espace_dessin.create_polygon(x+6,y+2, x+6,y+14, x+20,y+8, fill=case_mines)

    #fonction qui dessine la grille non découverte
    def dessiner_grille():

        for x in range(50,300,25):
            for y in range(50,300,25):

                i=random.randint(0,4)

                if i>3 :
                    color=fond3

                elif i>1 :
                    color=fond

                else :
                    color=fond2

                espace_dessin.create_rectangle(x,y,x+25,y+25,outline=texte,width=0,fill=color)
                espace_dessin.place(x=150, y=100)

        espace_dessin.create_rectangle(49,49,301,301,outline=texte,width=2)

    #fonction relié au bouton rejouer
    def rejouer():
        global fin,score

        if fin==True:
            fenetre.destroy()
            jouer()

    #fonction qui gere le label de score
    def result():

        if fin==False :
            #Création d'un label pour afficher le résultat
            label1=tkinter.Label(fenetre,text="score : "+ str(score),font=("Helvetica",16),fg=texte,bg=fond2)
            label1.place(x=325, y=429, anchor='center')

        if fin==True :

            if score<victoire:
                #Création d'un label pour afficher le résultat
                label1=tkinter.Label(fenetre,text="perdu, votre score était : "+ str(score),font=("Impact",16),fg=case_mines,bg=fond2)
                label1.place(x=325, y=429, anchor='center')

        if score>=victoire:
            #Création d'un label pour afficher gagné
            label1=tkinter.Label(fenetre,text="   gagné !  ",font=("Impact",20),fg=texte,bg=fond2)
            label1.place(x=325, y=429, anchor='center')

    #Création de la fenetre
    fenetre=tkinter.Tk()
    fenetre.title("demineur")
    fenetre.geometry("654x554")
    #non redimensinnable
    fenetre.resizable(width=False, height=False)
    #Couleur de fond
    fenetre.configure(bg=fond)

    #Création d'un espace de dessin
    espace_dessin=tkinter.Canvas(bg=fond2,width=350, height=350,cursor="tcross",highlightthickness=0)

    #appel des fonctions
    mines(grille)
    afficherGrille(grille)
    dessiner_grille()

    #Création d'un bouton rejouer
    bouton=tkinter.Button(text="rejouer",command=rejouer,font=("Impact",16),relief="flat")
    #on Centre le bouton
    bouton.place(x=325, y=504, anchor='center')
    #on défini les couleur du bouton
    bouton.configure(bg=fond2,activebackground=fond3,fg=texte,activeforeground=texte,width=34,height=1)

    #Création d'un label pour afficher le titre
    label2=tkinter.Label(fenetre,text="DEMINEUR",font=("Impact",30),fg=texte,bg=fond)
    label2.place(x=325, y=50, anchor='center')

    #relie les fonction au clicks
    fenetre.bind("<Button-1>", pointeur)
    fenetre.bind("<Button-3>", drapeau)

    fenetre.mainloop()

jouer()







