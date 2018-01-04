using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace IPROG_Alice
{
    class Program
    {





        static void DessinerPlateau(char[,] tab)
        {
            //procédure qui trace le plateau de jeu, i.e. un tableau de case 10x10
            Console.Write("   *  A  *  B  *  C  *  D  *  E  *  F  *  G  *  H  *  I  *  J  *\n");
            for (int i = 0; i < 10; i++)
            {
                Console.WriteLine("****************************************************************");
                Console.Write("   *     *     *     *     *     *     *     *     *     *     *\n");
                //nom des lignes
                if (i < 9)
                {
                    Console.Write((i + 1) + "  ");
                }
                else
                {
                    Console.Write((i + 1) + " ");
                }//fin nom des lignes
                for (int j = 0; j < 10; j++)
                {
                    Console.Write("*  " + tab[i, j] + "  ");
                    if (j == 9)
                    {
                        Console.Write("*");
                    }
                }
                Console.Write("\n");
                Console.Write("   *     *     *     *     *     *     *     *     *     *     *\n");
                if (i == 9)
                {
                    Console.Write("****************************************************************\n");
                }
            }
        }




        static void InitialiserPlateau(char[,] tab, char[] tabBateau)
        {
            Random r = new Random();
            int choix;
            bool test;
            do
            {
                choix = r.Next(2);
                test = true;
                if (choix == 0)
                {

                    int i = r.Next(10);
                    int j = r.Next(10 - tabBateau.Length);
                    //teste si le tableau est vide là où on veut écrire
                    int k; //compteur de la taille du tableau du bateau
                    for (k = 0; k < tabBateau.Length; k++)
                    {
                        if (tab[i, j + k] == '#')
                        {
                            test = false;
                        }
                    }

                    if (test == true)
                    {
                        for (k = 0; k < tabBateau.Length; k++)
                        {
                            tab[i, j + k] = tabBateau[k];
                        }
                    }
                }


                else
                {

                    int j = r.Next(10);
                    int i = r.Next(10 - tabBateau.Length);
                    //teste si le tableau est vide là où on veut écrire
                    int k; //compteur de la taille du tableau du bateau
                    for (k = 0; k < tabBateau.Length; k++)
                    {
                        if (tab[i + k, j] == '#') //(tab[i + k, j] != 0)
                        {
                            test = false;
                        }
                    }

                    if (test == true)
                    {
                        for (k = 0; k < tabBateau.Length; k++)
                        {
                            tab[i + k, j] = tabBateau[k];
                        }
                    }
                }
            } while (test == false);
        }




        static void InitialiserPlateauOrdi(char[,] tab, char[] tabBateau, int nb, ref int[] data)
        {
            Random r = new Random();
            int choix;
            bool test;
            int indiceI; //à récupérer, premier case du bateau
            int indiceJ;
            do
            {
                choix = r.Next(2);
                test = true;

                if (choix == 0)
                {

                    int i = r.Next(10);
                    int j = r.Next(10 - tabBateau.Length);
                    indiceI = i;
                    indiceJ = j;
                    //teste si le tableau est vide là où on veut écrire
                    int k; //compteur de la taille du tableau du bateau
                    for (k = 0; k < tabBateau.Length; k++)
                    {
                        if (tab[i, j + k] != 0)
                        {
                            test = false;
                        }
                    }

                    if (test == true)
                    {
                        for (k = 0; k < tabBateau.Length; k++)
                        {
                            tab[i, j + k] = tabBateau[k];
                        }
                    }

                }
                else
                {


                    int j = r.Next(10);
                    int i = r.Next(10 - tabBateau.Length);
                    indiceI = i;
                    indiceJ = j;
                    //teste si le tableau est vide là où on veut écrire
                    int k; //compteur de la taille du tableau du bateau
                    for (k = 0; k < tabBateau.Length; k++)
                    {
                        if (tab[i + k, j] != 0)
                        {
                            test = false;
                        }
                    }

                    if (test == true)
                    {
                        for (k = 0; k < tabBateau.Length; k++)
                        {
                            tab[i + k, j] = tabBateau[k];
                        }
                    }

                }
            } while (test == false);
            //data = { tabBateau.Length, choix, indiceI; indiceJ};
            data[0 + nb * 4] = tabBateau.Length;
            data[1 + nb * 4] = choix;
            data[2 + nb * 4] = indiceI;
            data[3 + nb * 4] = indiceJ;

            //return data;
        }


        static int LirePlateau(char[,] tab)
        {
            int nbDiese = 0;
            for (int i = 0; i < 10; i++)
            {
                for (int j = 0; j < 10; j++)
                {

                    if (tab[i, j] == '#')
                    {
                        nbDiese++;
                        /*donnees[k, 0] = i;
                        donnees[k, 1] = j;*/
                    }
                }
            }
            return nbDiese;
        }




        static void PresenterJeu(ref bool bool1, ref bool bool2)
        {
            Console.WriteLine("######################## BATAILLE NAVALE ########################");
            Console.WriteLine("\nBienvenue ! Ce jeu vous permet de jouer à la bataille navale contre votre ordinateur.");
            string reponse = "a";
            while ((reponse != "o") && (reponse != "n"))
            {

                Console.WriteLine("Voulez -vous lire les règles ? (o/n)");
                reponse = Console.ReadLine();
            }
            if (reponse == "o")
            {
                Console.WriteLine("##### REGLES #####\n\nLe but de la bataille navale est de couler tous les bateaux de son adversaire.\nDans un premier temps, chaque joueur place ses bateaux sur le plateau sans voir ceux de son adversaire.\nEnsuite, chacun à leur tour, les joueurs essaient de trouver et de couler les bateaux de l'adversaire.\nChaque joueur dispose de 5 bateaux longueurs différentes: \n- Un porte-avion d'une longueur de 5 cases\n- Un cuirassé d'une longueur de 4 cases \n- Un sous-marin et un croiseur d'une longueur de 3 cases \n- Un contre-torpilleur d'une longueur de 2 cases \nCe jeu dispose de deux modes de jeux suivant la façon de tirer :\n- le mode classique : A chaque tir annoncé par un joueur (par exemple, 'case C5'), le second joueur regarde si l’un de ses navires occupe la case visée. \nIl doit indiquer à l'autre joueur s'il a touché ('Touché') ou coulé ('Touché coulé') un navire ou bien tiré dans l'eau ('Raté').\n- mode salvo : chaque joueur annonce plusieurs tirs à la fois. Le nombre de tirs simultanés est de 5 initialement. Ce nombre diminue en fonction du nombre de bateaux déjà coulés.\nQuand un joueur détruit un navire de son adversaire, il ne tire plus que 4 coups au lieu de 5. Quand 2 bateaux sont détruits, le joueur ennemi tire uniquement 3 coups. \nQuand 3 navires sont détruits, le joueur opposé ne tire plus que 2 coups, etc.");
            }
            string reponse2 = "a";
            while ((reponse2 != "s") && (reponse2 != "n"))
            {
                Console.WriteLine("Avec quel mode voulez-vous jouer ? Tapez 's' pour le mode Salvo ou 'n' pour le mode normal");
                reponse2 = Console.ReadLine();
            }
            if (reponse2 == "n")
            {
                bool1 = false;
            }
            string reponse3 = "a";
            while ((reponse3 != "f") && (reponse3 != "i"))
            {
                Console.WriteLine("Quel niveau souhaitez-vous pour l'ordinateur ? Tapez 'f' pour facile ou 'i' pour intelligent");
                reponse3 = Console.ReadLine();
            }
            if (reponse3 == "i")
            {
                bool2 = true;
            }
        }



        static void TesterToucheCoule(char[] tabBateau, ref int nbBateauCoulé, ref bool coulé)
        {
            bool testBateau = true;
            for (int k = 0; k < tabBateau.Length; k++)
            {
                if (tabBateau[k] != 'X')
                {
                    testBateau = false;
                }
            }
            if (testBateau == true)
            {
                Console.WriteLine("Touché coulé");
                nbBateauCoulé++;
                coulé = true;
            }
        }





        static void FinirJeu()
        {
            //score ?
            Console.WriteLine("Merci d'avoir joué ! Nous espérons vous revoir bientôt !");
            Console.WriteLine("Ce programme a été développé par Antoine BOUQUET et Alice CANTAGREL dans le cadre du projet informatique IPROG de première année à l'ENSC.");
        }


        /*static void DefinirTableau(char[] tabBateau, int[] donnees, char[,] tab)
        {
            for (int a = 0; a < 20; a = a + 4)//va de 4 en 4 pour parcourir le tableau donnees
            {
                //if ((donnees[a])==5)
                if (donnees[1 + a] == 0)
                {
                    for (int k = 0; k < tabBateau.Length; k++)
                    {
                        tabBateau[k] = tab[donnees[2 + a], (donnees[3 + a] + k)];
                    }
                }
                else
                {
                    for (int k = 0; k < tabBateau.Length; k++)
                    {
                        Console.WriteLine("Donnees 2 : " + donnees[2]);
                        Console.WriteLine("Donnees 3 : " + (donnees[3] + k));
                        tabBateau[k] = tab[(donnees[2 + a] + k), donnees[3 + a]];
                        //Console.WriteLine("PA : " + PA[k]);
                    }
                }
            }
        }*/


        static void Main(string[] args)
        {
            //interface et choix de jeu
            bool modeJeu = true; //par défaut le mode salvo est lancé
            bool niveauOrdi = false; //par défaut le niveau facile est lancé
            PresenterJeu(ref modeJeu, ref niveauOrdi); //faut placer les arguments en références pour qu'ils soient effectivement modifiés
            //Console.WriteLine("Mode Jeu : "+ modeJeu); tu peux effacer si tu veux 
            //Console.WriteLine("nivordi :" +niveauOrdi);

            char[,] tabj1 = new char[10, 10];
            char[,] tabOrdi = new char[10, 10];
            int nbTours = 0;

            //Création des bateaux
            char[] PA = { '#', '#', '#', '#', '#' }; //PorteAvion
            char[] Cuir = { '#', '#', '#', '#' }; //Cuirassé
            char[] SM = { '#', '#', '#' }; //SousMarin
            char[] Crois = { '#', '#', '#' }; //Croiseur
            char[] CT = { '#', '#' };//ContreTorpilleur

            //Début de partie : choix du placement des bateaux
            Random r = new Random();
            int choix = r.Next(2); //si choix vaut 0, il place de manière horizontale, sinon de manière verticale
            string reponse = "a";
            Console.WriteLine("\nVOTRE PLATEAU");
            InitialiserPlateau(tabj1, PA);
            InitialiserPlateau(tabj1, Cuir);
            InitialiserPlateau(tabj1, SM);
            InitialiserPlateau(tabj1, Crois);
            InitialiserPlateau(tabj1, CT);
            DessinerPlateau(tabj1);
            while ((reponse != "o") && (reponse != "n"))
            {
                Console.WriteLine("NbDiese :" + LirePlateau(tabj1));
                Console.WriteLine("Le placement des bateaux vous convient-il ? (o/n)");
                reponse = Console.ReadLine();

                while (reponse == "n")
                {
                    tabj1 = new char[10, 10]; //réinitialiser tabj1 
                    InitialiserPlateau(tabj1, PA);
                    InitialiserPlateau(tabj1, Cuir);
                    InitialiserPlateau(tabj1, SM);
                    InitialiserPlateau(tabj1, Crois);
                    InitialiserPlateau(tabj1, CT);
                    DessinerPlateau(tabj1);
                    Console.WriteLine("NbDiese :" + LirePlateau(tabj1));
                    Console.WriteLine("Le placement des bateaux vous convient-il ? (o/n)");
                    reponse = Console.ReadLine();
                }
            }
            Console.WriteLine("\n \n");


            //Initialisation du plateau de l'IA
            Console.WriteLine("PLATEAU DE L'IA");
            int[] donnees = new int[20];
            int compteur = 0;
            int bateau = 0;
            for (compteur = 0; compteur < 5; compteur++)
            {
                if (bateau == 0)
                {
                    InitialiserPlateauOrdi(tabOrdi, PA, compteur, ref donnees);
                }
                if (bateau == 1)
                {
                    InitialiserPlateauOrdi(tabOrdi, Cuir, compteur, ref donnees);
                }
                if (bateau == 2)
                {
                    InitialiserPlateauOrdi(tabOrdi, SM, compteur, ref donnees);
                }
                if (bateau == 3)
                {
                    InitialiserPlateauOrdi(tabOrdi, Crois, compteur, ref donnees);
                }
                if (bateau == 4)
                {
                    InitialiserPlateauOrdi(tabOrdi, CT, compteur, ref donnees);
                }
                bateau++;
            }

            /*TEST POUR PLACEMENT D 1 BATEAU 
             * for (int k = 0; k < 5; k++)
             {
                 tabOrdi[3 + k, 0] = PA[k];
             }*/


            DessinerPlateau(tabOrdi);
            Console.WriteLine("\n");


            //Déroulement de la partie
            int nbBateauCoulé = 0;
            int nbTirDispo = 5 - nbBateauCoulé;
            
            bool couléPA = false;
            bool couléCuir = false;
            bool couléSM = false;
            bool couléCrois = false;
            bool couléCT = false;

            while ((couléPA == false) || (couléCuir == false) || (couléSM == false) || (couléCrois == false) || (couléCT == false))
            {
                nbTirDispo = 5 - nbBateauCoulé;
                nbTours++;
                Console.WriteLine("###### TOUR {0} ######", nbTours);
                Console.WriteLine("C'est votre tour. Sur quelle case voulez-vous tirer ? \nVous avez le droit à {0} tir(s).", nbTirDispo);
                for (int nbTir = 0; nbTir < nbTirDispo; nbTir++)
                {
                    Console.WriteLine("C'est votre tir n°{0}.", (nbTir + 1));
                    string saisieColonne = "K";
                    while ((saisieColonne != "A") && (saisieColonne != "B") && (saisieColonne != "C") && (saisieColonne != "D") && (saisieColonne != "E") && (saisieColonne != "F") && (saisieColonne != "G") && (saisieColonne != "H") && (saisieColonne != "I") && (saisieColonne != "J"))
                    {
                        Console.Write("Colonne ? ");
                        saisieColonne = Console.ReadLine();
                    }
                    int colonne = -3;
                    if (saisieColonne == "A")
                    { colonne = 0; }
                    else if (saisieColonne == "B")
                    { colonne = 1; }
                    else if (saisieColonne == "C")
                    { colonne = 2; }
                    else if (saisieColonne == "D")
                    { colonne = 3; }
                    else if (saisieColonne == "E")
                    { colonne = 4; }
                    else if (saisieColonne == "F")
                    { colonne = 5; }
                    else if (saisieColonne == "G")
                    { colonne = 6; }
                    else if (saisieColonne == "H")
                    { colonne = 7; }
                    else if (saisieColonne == "I")
                    { colonne = 8; }
                    else if (saisieColonne == "J")
                    { colonne = 9; }


                    int ligne = 11;
                    while ((ligne < 0) || (ligne > 9))
                    {
                        Console.Write("Ligne ? ");
                        string saisieLigne = Console.ReadLine();
                        ligne = Convert.ToInt16(saisieLigne);
                    }
                    ligne--;//pour que le numéro de ligne entré corresponde à la bonne ligne du plateau


                    //Actions sur le plateau de l'IA
                    if (tabOrdi[ligne, colonne] == 0)
                    {
                        tabOrdi[ligne, colonne] = 'O'; //raté, tir dans l'eau
                        Console.WriteLine("Raté !");
                    }
                    else
                        if (tabOrdi[ligne, colonne] == '#')//cad sur un bateau
                    {
                        tabOrdi[ligne, colonne] = 'X'; //touché
                        Console.WriteLine("Touché !");
                    }
                    else if ((tabOrdi[ligne, colonne] == 'O') || (tabOrdi[ligne, colonne] == 'X'))
                    { Console.WriteLine("Attention ! Vous aviez déjà tiré à cet endroit."); } // ATTENTION ! DEPEND DU MODE DE JEU


                    //TOUCHE COULE
                    //Les 4 premières cases du donnees[] sont consacrées au PA
                    if (donnees[1] == 0)
                    {
                        for (int k = 0; k < 5; k++)
                        {
                            PA[k] = tabOrdi[donnees[2], (donnees[3] + k)];
                        }
                    }
                    else
                    {
                        for (int k = 0; k < 5; k++)
                        {
                            PA[k] = tabOrdi[(donnees[2] + k), donnees[3]];
                        }
                    }
                    //Les cases 4 à 7 sont consacrées au Cuir
                    if (donnees[5] == 0)
                    {
                        for (int k = 0; k < 4; k++)
                        {
                            Cuir[k] = tabOrdi[donnees[6], (donnees[7] + k)];
                        }
                    }
                    else
                    {
                        for (int k = 0; k < 4; k++)
                        { Cuir[k] = tabOrdi[(donnees[6] + k), donnees[7]]; }
                    }
                    //Les cases 8 à 11 sont consacrées au SM
                    if (donnees[9] == 0)
                    {
                        for (int k = 0; k < 3; k++)
                        {
                            SM[k] = tabOrdi[donnees[10], (donnees[11] + k)];
                        }
                    }
                    else
                    {
                        for (int k = 0; k < 3; k++)
                        { SM[k] = tabOrdi[(donnees[10] + k), donnees[11]]; }
                    }
                    //Les cases 12 à 15 sont consacrées au Crois
                    if (donnees[13] == 0)
                    {
                        for (int k = 0; k < 3; k++)
                        {
                            Crois[k] = tabOrdi[donnees[14], (donnees[15] + k)];
                        }
                    }
                    else
                    {
                        for (int k = 0; k < 3; k++)
                        { Crois[k] = tabOrdi[(donnees[14] + k), donnees[15]]; }
                    }
                    //Les cases 16 à 19 sont consacrées au CT
                    if (donnees[17] == 0)
                    {
                        for (int k = 0; k < 2; k++)
                        {
                            CT[k] = tabOrdi[donnees[18], (donnees[19] + k)];
                        }
                    }
                    else
                    {
                        for (int k = 0; k < 2; k++)
                        {
                            CT[k] = tabOrdi[(donnees[18] + k), donnees[19]];
                        }
                    }
                    /*
                    DefinirTableau(PA, donnees, tabOrdi);
                    DefinirTableau(PA, donnees, tabOrdi);
                    DefinirTableau(PA, donnees, tabOrdi);
                    DefinirTableau(PA, donnees, tabOrdi);*/


                    int c1 = 0;
                    int c2 = 0;
                    int c3 = 0;
                    int c4 = 0;
                    int c5 = 0;
                    while ((couléPA == false) && (c1 == 0))
                    {
                        c1++;
                        TesterToucheCoule(PA, ref nbBateauCoulé, ref couléPA);
                    }
                    while ((couléCuir == false) && (c2 == 0))
                    {

                        TesterToucheCoule(Cuir, ref nbBateauCoulé, ref couléCuir);
                        c2++;
                    }
                    while ((couléSM == false) && (c3 == 0))
                    {
                        c3++;
                        TesterToucheCoule(SM, ref nbBateauCoulé, ref couléSM);
                    }
                    while ((couléCrois == false) && (c4 == 0))
                    {
                        c4++;
                        TesterToucheCoule(Crois, ref nbBateauCoulé, ref couléCrois);
                    }
                    while ((couléCT == false) && (c5 == 0))
                    {
                        c5++;
                        TesterToucheCoule(CT, ref nbBateauCoulé, ref couléCT);
                    }

                }//for (nbTir<5)
                Console.WriteLine("Vous avez coulé {0} bateau(x).", nbBateauCoulé);

            }

            Console.WriteLine("Bravo ! Vous avez gagné !\n Il vous a fallu {0} tours pour gagner.", nbTours);
            


            //Vérifications 
            DessinerPlateau(tabOrdi);


            //scores

            //Sauvegarde

            //Fin de partie
            FinirJeu();


            Console.ReadKey();
        }


    }
}
