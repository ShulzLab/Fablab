## Paramètres généraux :

Nom : Fraiseuse 500BL CNC PROXXON 

<u>Course axes en mm ( ne pas dépasser ) :</u>

X : 0 : 280

Y : 0 : 103

Z : 0 : -194

<u>Post-Processeur :</u> 

[Explication sur le site du fabricant du contrôleur machine](http://www.max-computer.de/x5f/adaptation-du-postprocesseur-.htm)

Basé sur Code DIN/ISO 66025 - Nom du standard : KosyCNC



Le logiciel Nccad9 est capable de charger :

- fichiers KosyCNC **(.knc)** : code ISO pour exécuter programme (écrit à la main ou nécessite post-processeur ISO 66025 dans logiciel CAO/FAO comme SolidworksCAM par exemple. Dans les faits, un post-processeur Marlin tout simple suffit puisqu'aucun des deux standards n'inclut de code spéciaux (utilisation quasi exclusivement de G0 G1 G2 G3 et F).

- fichiers .DXF pour utilitaire FAO
- fichiers .HPG ou .PLT pour utilitaire FAO

Contient un petit éditeur avec simulation et visualisation pour le code ISO ainsi qu'une version en "conversationnel" (appelé dialogue par le logiciel)

## Outils :

<u>Important :</u>

- Ne jamais utiliser une vitesse supérieure à 800 t/min avec une pinnule de centrage au risque de la casser.

- En usinage métal, lubrifier outil.

- En utilisation avec métal, [calculer](https://www.cncfraises.fr/content/16-fraisage-definir-les-parametres-de-coupe) les paramètres suivants avant de lancer un programme : 

	1. <u>Vitesse de Coupe</u> **Vc** (en m/min) : obtenir dans un tableau en fonction du matériau usiné, du matériau de l'outil (carbure / acier rapide) et de l'opération (ébauche/finition)

		La vitesse de coupe est la distance parcourue par une dent en une minute/ Elle correspond à la longueur du copeau enlevé exprimée en mètre par un outil de coupe en une minute. Elle dépend de la matière usinée, du type d'opération, de l'outil, de l'[état de surface](https://fr.wikipedia.org/wiki/État_de_surface) (ébauche/finition) souhaité.

	2. <u>Avance par dent</u> **fz** (en mm/dent) : obtenir dans un tableau en fonction du matériau usiné, du matériau de l'outil (carbure / acier rapide) et de l'opération (ébauche/finition)

		Elle correspond à la quantité de matière qu'une dent va mordre dans la matière avant d'être vidée, ou la largeur du copeau (en tournage). Une avance par dent trop faible peut résulter en une usure de l'outil prématurée (chauffe sur la tranche et émoussage). Une avance trop élevée peut résulter en la casse de l'outil (et par conséquent son éjection en vol : **dangereux**)

	3. <u>Fréquence de rotation</u> **S** (speed) (en tr/min) :
		$$
		S = \frac{1000 \times Vc}{\pi \times DiametreFraise(mm) }
		$$

	4. <u>Vitesse Avance</u> **F** (feed) (en mm/min) : obtenir dans un tableau en fonction du matériau et de l'opération (ébauche/finition)
		$$
		F = S \times NombreDentFraise \times fz
		$$
		**Attention :** Le paramètre **S** ( réglage fréquence de rotation / vitesse broche en t/min) n'est pas prise en charge par le logiciel (bien qu'usuellement pris en charge en code ISO standard), le réglage de la broche se fait exclusivement via le potentiomètre de contrôle manuel, après avoir activé le relai 220V de la broche, interne au contrôleur via le panneau de commande du logiciel. Le paramètre **F** est en revanche à entrer dans le Gcode. Enfin, les paramètres **Vc** et **fz** ne sont utiles que pour l'obtention des deux premiers.

	<u>Document récapitulatif pour paramètres **Vc** et **Fz** :</u>

	<embed src="C:\Users\Timothe\NasgoyaveOC\Professionnel\TheseUNIC\MKdocs\docs\Machines\Parametres_coupe.pdf" type="application/pdf" height="800px" width="1200px"/>

## Gcode :

[Code ISO basé sur 66025](https://www.isel.com/fr/mwdownloads/download/link/id/4292/), (comme fraiseuse [Isel](https://www.isel.com/fr) de patrick) notes de retours d'expériences :

G2 et G3 (arcs de cercles) fonctionnent dans le code .knc mais pas dans l'éxecuteur de commande live.

G90 et G91 (relatif, absolu) ne sont pas supportés

G41 et G42 ne sont pas disponibles (correction de contour avec prise en charge des correcteurs d'usure/correcteurs dynamiques)

M2 (stop programme) n'engendre pas un arret et la suite est exécutée directement.

La commande S (fréquence rotation broche) n'est pas prise en charge, le réglage de la broche se fait exclusivement via le potentiomètre de contrôle manuel.

## Origines pièces :

<u>Réglage :</u> 

La fonction "choisir origine pièce" ouvre une fenêtre dans laquelle sont affichés 20 mémoires origines pièces.

En sélectionner une (double clic) fait apparaître une fenêtre dans laquelle il est possible de rentrer les valeurs des décalages pour la nouvelle origine.

Seuls les champs X,Y et Z sont utiles pour cette machine ( les autres champs sont utiles pour les axes secondaires : UVW et les axes de rotation ABC. Aucun de ceux-ci n'est présent sur la machine) 

La "remarque" permet d'afficher le nom de l'origine pièce (OP) séléctionnée dans le panneau de commande par la suite.

Ces multiples origines pièces peuvent être utiles si quelqun effecture un montage sur une partie de l'axe X long (à une extremité) nécessitant plusieurs opérations, par plusieurs outils (fraises diamètres différents, formes différentes) référencées à la même origine, et qu'il n'a pas le temps de les finir toutes, avant de laisser la possibilité à lui ou quelqu'un d'autre d'usiner une pièce entre temps. L'utilisation d'une autre origine pièce pour le second utilisateur montant son brut à l'autre extrémité de l'axe X peut se révéler utile.

Un autre cas de figure est l'utilisation d'un outil de montage préréglé, fixe (comme un étau, n'ayant pas vocation à être déplacé, sur lequel on aurait préréglé le 0 à un coin, rendant ainsi le calcul de l'origine pièce pour l'utilisateur s'en servant plus aisé)

#### Cas origine n°1 : 

Lors de l'entrée des valeurs, celles-ci seront soustraites (pour les axes X et Y) de l'origine actuelle afin de générer la nouvelle origine. Pour l'axe Z, la valeur sera ajoutée.

Il est ainsi possible de re-régler une origine pièce à partir d'une origine actuelle déjà modifiée en **renseignant 0** pour les axes que l'on ne souhaite pas changer, et en rentrant la valeur positive **que l'on souhaite soustraire** pour X et Y ou la valeur positive **que l'on souhaite ajouter** en Z.

Afin de supprimer tout offset par rapport à l'origine machine pour une mémoire, il suffit lorsque l'on entre dans un d'entre elles, de rajouter des signes négatifs sur les valeurs proposées dans les champs X et Y et de ne pas changer la valeur du champ Z. Presser sauvegarder et OK.

Attention : il est à noter que chaque origine pièce à partir de la seconde est définie relativement à l'origine pièce n1 ( origine 1 relative à OM, origine 2 à 20 relatives à origine 1 )

#### Cas origine n°2 à 20 : 

Lors de l'entrée des valeurs, celles-ci seront soustraites (pour les axes X et Y) de l'origine n°1 afin de générer la nouvelle origine. Pour l'axe Z, la valeur sera ajoutée.

Lors de l'édition de ces lignes cependant, la valeur indiquée dans les champs n'est pas à nouveau ajoutée à l'origine actuelle si l'utilisateur presse le bouton sauvegarder ou accepter, contrairement à l'origine n°1.

<u>Attention :</u> Toutes ces origines sont réglées par rapport à l'origine réglée en 1.

Ainsi, si un utilisateur change la valeur de l'origine 1 alors que vous utilisiez une origine 2-20, la position absolue de votre origine sur la table croisée sera altérée.

<u>Retour origine pièce :</u>

Lors du retour à une origine pièce, avec le bouton '"aller à origine pièce"', si les axes XY ne sont pas déjà à 0 par rapport à l'origine pièce, le déplacement remonte et passe par le Z0 machine, puis translate sur le plan XY de la valeur nécessaire et redescends en Z à la valeur 0 pièce. 

Ceci n'est pas le cas lorsque seul l'axe Z n'est pas à l'origine, et il est alors simplement ajusté.

Il peut ainsi être plus rapide de tapper dans le champ rapide : G0 X0 Y0 afin de faire se déplacer la machine à l'origine pièce sur le plan XY, après toutefois avoir vérifié qu'aucun obstacle ne se trouve à la hauteur de la tête ni de l'outil. Dans le doute , il est toujours préférable d'utiliser la commande "aller à origine pièce" ou de remonter manuellement sur l'axe Z au préalable.

## Divers liens:

[Modif de fraiseuse Kosy](http://www.metabricoleur.com/t13857-modifications-d-une-fraiseuse-cnc-kosy)

