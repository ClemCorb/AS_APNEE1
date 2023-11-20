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
A -> X = E ;  A | E | ε  
E -> E + T | E - T | T 
T -> T * F | T / F | F   
F -> G ^ F | G 
G -> sin G | cos G | X 

X -> var | (A)
Y -> immediate | pi  
 

### G1 sans récursivité à Gauche (sauf pour A)
A -> X = E ; A | E | ε  
E -> T E'  
E'-> + T E' | - T E' | ε  
T -> F T'  
T'-> * F T' | / F T' | ε 
F -> G ^ F | G 
G -> sin G | cos G | X   

X -> var | (A) 
Y -> immediate | pi  


## Table LL1 calculée

Donnez la table LL1 de votre grammaire.
### Premiers  
|     | var | (   | )   | pi  | immediate | +   | -   | *   | /   | ^   | sin | cos | =   | ;   | ε   |
| --- | --- | --- | --- | --- | --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A   | ♦   | ♦   |     |     |           |     |     |     |     |     | ♦   | ♦   |     |     | ♦   |
| E   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| E'  |     |     |     |     |           | ♦   | ♦   |     |     |     |     |     |     |     | ♦   |
| T   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| T'  |     |     |     |     |           |     |     | ♦   | ♦   |     |     |     |     |     | ♦   |
| F   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| G   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| X   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     |     |     |     |     |     |
| Y   |     |     |     | ♦   | ♦         |     |     |     |     |     |     |     |     |     |     |
  
### Suivants
Soit S la source tel que : 
S -> A$
|     | var | (   | )   | pi  | immediate | +   | -   | *   | /   | ^   | sin | cos | =   | ;   | $   |
| --- | --- | --- | --- | --- | --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A   |     |     |     |     |           |     |     |     |     |     |     |     |     | ♦   | ♦   |
| E   |     |     | ♦   |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| E'  |     |     |     |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| T   |     |     |     |     |           | ♦   | ♦   |     |     |     |     |     |     |     |     |
| T'  |     |     |     |     |           |     |     |     |     |     |     |     |     | ♦   |     |
| F   |     |     | ♦   |     |           | ♦   | ♦   | ♦   | ♦   | ♦   |     |     |     |     |     |
| G   |     | ♦   |     |     |           | ♦   | ♦   | ♦   | ♦   | ♦   | ♦   | ♦   |     |     |     |
| X   |     |     | ♦   |     |           | ♦   | ♦   | ♦   | ♦   | ♦   |     |     | ♦   |     |     |
| Y   |     |     | ♦   |     |           | ♦   | ♦   | ♦   | ♦   | ♦   |     |     |     |     |     |
  
  ### Directeurs
1. A -> X = E ; A
2. A -> E
3. A -> ε
4. E -> T E'
5. E'-> + T E'
6. E'-> - T E'
7. E'-> ε
8. T -> F T'
9. T'-> * F T'
10. T'-> / F T'
11. T'-> ε
12. F -> G ^ F
13. F -> G
14. G -> sin G
15. G -> cos G
16. G -> X 
17. G -> Y
18. X -> var
19. X -> (E)
20. Y -> immediate
21. Y -> pi

Tableau des directeurs
|       |  var  |   (   |   )   |  pi   | immediate |   +   |   -   |   *   |   /   |   ^   |  sin  |  cos  |   =   |   ;   |   $   |
| :---: | :---: | :---: | :---: | :---: | :-------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|   A   |  1,2  |  1,2  |       |   2   |     2     |       |       |       |       |       |  1,2  |  1,2  |       |   3   |   3   |
|   E   |   4   |   4   |       |   4   |     4     |       |       |       |       |       |   4   |   4   |       |       |       |
|  E'   |       |       |       |       |           |   5   |   6   |       |       |       |       |       |       |   7   |       |
|   T   |   8   |   8   |       |   8   |           |       |       |       |       |       |   8   |   8   |       |       |       |
|  T'   |       |       |       |       |           |       |       |   9   |  10   |       |       |       |       |  11   |       |
|   F   | 12,13 | 12,13 |       | 12,13 |   12,13   |       |       |       |       |       |       |       |       |       |       |
|   G   |  16   |  16   |       |  16   |    17     |       |       |       |       |       |  14   |  15   |       |       |       |
|   X   |  18   |  19   |       |  21   |    20     |       |       |       |       |       |       |       |       |       |       |

Pour A & G, on regarde les cas exeptionnels pour lesquels il faut regarder 2 elements pour savoir quel directeur choisir
 * Pour A si il y a un = apres le X on choisis la règle 1 sinon 2 
 * Pour F si il y a un ^ apres le G on choisis la règle 12 sinon 13  


## Difficultés rencontrées

Implementer les ambiguïtées avec A et F 

## Remarques éventuelles