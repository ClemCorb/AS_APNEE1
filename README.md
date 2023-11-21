# Instructions

Pour pouvoir compiler le projet sur votre ordinateur, il vous faut installer les librairies flex et bison.

- Sur linux (ubuntu) cela se fait par la commande :
  
  sudo apt-get install flex bison
  
  (ou équivalent sur d'autres distributions)

- Sur mac, ces librairies sont installées en standard. Si ce n'est pas le cas pour vous, cela peut s'installer via homebrew (cf https://brew.sh/index_fr)

  brew install flex bison

- sur windows, je ne sais pas, à vous de chercher.

# Compte rendu

Je vous demande de rédiger votre compte rendu dans ce fichier que vous rendrez avec vos programmes sur caseine. Je vous suggère les éléments suivants:

## Grammaires utilisée pour les différentes étapes

Donnez ici les grammaires que vous avez utilisées pour résoudre votre problème.
### G0
S -> A ; S | ε
A -> var = A | E   
E -> E + T | E - T | T 
T -> T * F | T / F | F   
F -> F ^ G | G 
G -> sin G | cos G | var | (A) | pi | imediate  
 

### G1 sans récursivité à Gauche (sauf pour A)
S -> A ; S | ε
A -> var = E | E 
E -> T E'  
E'-> + T E' | - T E' | ε  
T -> F T'  
T'-> * F T' | / F T' | ε 
F -> G F'  
F'-> ^ G F' | ε
G -> sin G | cos G | var | (A) | pi | imediate


## Table LL1 calculée

Donnez la table LL1 de votre grammaire.
### Premiers  
|     | var | (   | )   | pi  | immediate | +   | -   | *   | /   | ^   | sin | cos | =   | ;   | ε   |
| --- | --- | --- | --- | --- | --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| S   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     | ♦   |
| A   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     | ♦   |
| E   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| E'  |     |     |     |     |           | ♦   | ♦   |     |     |     |     |     |     |     | ♦   |
| T   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| T'  |     |     |     |     |           |     |     | ♦   | ♦   |     |     |     |     |     | ♦   |
| F   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| F'  |     |     |     |     |           |     |     |     |     | ♦   |     |     |     |     |     |
| G   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |

  
### Suivants
Soit S4 la source tel que : 
S' -> S$
|     | var | (   | )   | pi  | immediate | +   | -   | *   | /   | ^   | sin | cos | =   | ;   | $   |
| --- | --- | --- | --- | --- | --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| S   |     |     |     |     |           |     |     |     |     |     |     |     |     |     | ♦   |
| A   |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| E   |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| E'  |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| T   |     |     | ♦   |     |           | ♦   | ♦   |     |     |     |     |     |     | ♦   |     |
| T'  |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| F   |     |     | ♦   |     |           | ♦   | ♦   | ♦   | ♦   |     |     |     |     | ♦   |     |
| F'  |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| G   |     | ♦   | ♦   |     |           | ♦   | ♦   | ♦   | ♦   | ♦   |     |     |     | ♦   |     |
  
  ### Directeurs
0. S' - > S$
1. S - > A;A 
2. S -> ε
3. A -> var = E
4. A -> E
5. E -> T E'
6. E'-> + T E'
7. E'-> - T E'
8. E'-> ε
9.  T -> F T'
10. T'-> * F T'
11. T'-> / F T'
12. T'-> ε
13. F -> G F'
14. F' -> ^ G F'
15. F' -> ε
16. G -> sin G
17. G -> cos G
18. G -> var
19. G -> (A)
20. G -> pi
21. G -> immediate

Tableau des directeurs
|       |  var  |   (   |   )   |  pi   | immediate |   +   |   -   |   *   |   /   |   ^   |  sin  |  cos  |   =   |   ;   |   $   |
| :---: | :---: | :---: | :---: | :---: | :-------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|   S   |   1   |   1   |       |   1   |     1     |       |       |       |       |       |   1   |   1   |       |       |   2   |
|   A   |  3,4  |   4   |       |   4   |     4     |       |       |       |       |       |   4   |   4   |       |       |       |
|   E   |   5   |   5   |       |   5   |     5     |       |       |       |       |       |   5   |   5   |       |       |       |
|  E'   |       |       |       |       |           |   6   |   7   |       |       |       |       |       |       |   8   |       |
|   T   |   9   |   9   |       |   9   |           |       |       |       |       |       |   9   |   9   |       |       |       |
|  T'   |       |       |       |       |           |       |       |  10   |  11   |       |       |       |       |  12   |       |
|   F   |  13   |  13   |       |  13   |           |       |       |       |       |       |  13   |  13   |       |       |       |
|  F'   |       |       |       |       |           |       |       |       |       |  14   |       |       |       |  15   |       |
|   G   |  18   |  19   |       |  20   |    21     |       |       |       |       |       |  16   |  17   |       |       |       |

Pour A on regarde les cas exeptionnels pour lesquels il faut regarder 2 elements pour savoir quel directeur choisir
 * Pour A si il y a un = apres le var on choisis la règle 3 sinon 4 
 


## Difficultés rencontrées
 

## Remarques éventuelles