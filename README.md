# Bataille navale en MIPS32

Simulateur de bataille navale écrit en **assembleur MIPS32**, réalisé seule dans le cadre de ma première année de BUT Informatique (Université de Strasbourg).

> *Cinq navires cachés sur une grille de 10 × 10. Aucun joueur : le programme place la flotte, tire au hasard, puis traque chaque navire touché jusqu'à le couler.*

## ⛴︎ Le principe

Le programme joue seul une partie complète :

1. **Mise en place** : la grille est remplie de cases vides, puis les 5 navires sont placés aléatoirement, horizontalement ou verticalement, sans chevauchement et sans sortir de la grille.
2. **Phase de chasse** : le programme tire sur une case au hasard.
3. **Phase de traque** : dès qu'un navire est touché, la fonction `traque` explore **récursivement** les 4 cases voisines (nord, sud, ouest, est) jusqu'à ne plus rien trouver.
4. **Fin de partie** : la partie se termine quand les **17 cases** de navires sont touchées. Le programme affiche alors le nombre total de coups.

La grille est affichée après chaque tir.

| Navire | Taille | Symbole |
|---|---|---|
| Porte-avions (*Aircraft Carrier*) | 5 | `A` |
| Cuirassé (*Battleship*) | 4 | `B` |
| Croiseur (*Cruiser*) | 3 | `C` |
| Sous-marin (*Submarine*) | 3 | `S` |
| Contre-torpilleur (*Destroyer*) | 2 | `D` |

Sur la grille, `.` représente une case vide, `~` un tir manqué et `X` un navire touché.

## ⛴︎ Aperçu

Exemple d'une partie lancée avec le programme. Au début, la grille initiale montre les 5 navires placés aléatoirement :

```
   A B C D E F G H I J
 1 . . . . . . . . . .
 2 . . . . . . . . B .
 3 C . . A . . . . B .
 4 C D . A . . . . B .
 5 C D . A . . . . B .
 6 . . . A . . . . . .
 7 . . . A . . . . . .
 8 . . S S S . . . . .
 9 . . . . . . . . . .
10 . . . . . . . . . .
```

À la fin de la partie, les 17 cases de navires sont touchées (`X`) et les tirs manqués sont marqués d'un `~` :

```
   A B C D E F G H I J
 1 . . ~ . . . . . ~ ~
 2 ~ . ~ ~ . . . ~ X ~
 3 X ~ ~ X ~ ~ ~ ~ X ~
 4 X X ~ X ~ . . ~ X ~
 5 X X ~ X ~ . . ~ X ~
 6 ~ ~ ~ X ~ . . . ~ .
 7 . . ~ X ~ . ~ . . .
 8 . ~ X X X ~ . . . .
 9 . . ~ ~ ~ ~ ~ . . ~
10 . . . . . . . . . .
```

Le programme affiche ensuite : `Le nombre total de coups dans cette partie est de : 56`

## ⛴︎ Points techniques

- **Grille** stockée dans une chaîne de 100 octets (`.space 100`) : la case `(ligne, colonne)` se trouve à l'index `ligne * 10 + colonne`.
- **Récursivité en assembleur** : à chaque appel, `traque` sauvegarde sur la pile son adresse de retour (`$ra`) et l'index de la case, puis les restaure après chaque exploration.
- **Gestion des bords** : avant chaque appel récursif, le programme vérifie que la case voisine ne sort pas de la grille (première ou dernière ligne, première ou dernière colonne grâce au modulo).
- **Code commenté** : chaque instruction est commentée et chaque fonction décrit son objectif, ses paramètres et les registres utilisés.

## ⛴︎ Lancer le programme

**Prérequis** : Java et le simulateur [MARS](https://dpetersanderson.github.io/) (fichier `Mars4_5.jar`, à télécharger depuis son site).

```bash
git clone https://github.com/Thalioukill/battleship-mips32.git
cd battleship-mips32
```

Placez `Mars4_5.jar` dans ce dossier, puis :

**Avec l'interface de MARS** :

1. Lancer MARS : `java -jar Mars4_5.jar`
2. Ouvrir `bataille_navale.asm` (*File > Open*).
3. Assembler le programme (*Run > Assemble* ou `F3`).
4. Lancer l'exécution (*Run > Go* ou `F5`).

Les grilles successives s'affichent dans la console *Run I/O*, en bas de la fenêtre.

**En ligne de commande** :

```bash
java -jar Mars4_5.jar nc bataille_navale.asm
```

## ⛴︎ Structure du code

| Fonction | Rôle |
|---|---|
| `initPartie` | Prépare une nouvelle partie : grille vide et placement des 5 navires |
| `initGrille` | Remplit la grille de cases vides (`.`) |
| `setShip` | Choisit aléatoirement l'axe de placement d'un navire |
| `placeHorizontale` / `placeVerticale` | Tirent une position valide et posent le navire |
| `verifPlaceHorizontale` / `verifPlaceVerticale` | Vérifient que les cases sont libres |
| `displayGrille` | Affiche la grille avec ses coordonnées |
| `displayLettres` / `addLineCount` | Affichent les lettres des colonnes et les numéros de lignes |
| `simulationJeu` | Boucle de chasse, jusqu'à 17 coups au but |
| `traque` | Tir sur une case et exploration récursive des cases voisines |
| `getAleatoire` / `getModulo` / `addNewLine` | Fonctions utilitaires |

## ⛴︎ À propos de l'historique

Ce projet a été réalisé avant que j'utilise Git : il est publié ici en un seul commit, sans historique de développement.

## ⛴︎ Crédits

Le sujet, ainsi que les fonctions de départ du fichier `start.s` (`initPartie`, `initGrille`, `setShip`, `getAleatoire`, `getModulo` et `addNewLine`), ont été fournis par l'équipe pédagogique de l'IUT Robert Schuman.

Le reste du code est distribué sous licence MIT (voir le fichier [LICENSE](LICENSE)).

© 2025 Thalia Herzog