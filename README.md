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
E -> X = E | E + T | E - T | T 
T -> T * F | T / F | F   
F -> G ^ F | G 
G -> sin G | cos G | X 

X -> var | (E) | immediate | pi  
 

### G1 sans récursivité à Gauche (sauf pour A)
A -> X = E ; A | E | ε  
E -> X = E | T E'  
E'-> + T E' | - T E' | ε  
T -> F T'  
T'-> * F T' | / F T' | ε 
F -> G ^ F | G 
G -> sin G | cos G | X   

X -> var | (E) | immediate | pi  


## Table LL1 calculée

Donnez la table LL1 de votre grammaire.
### Premiers  
|     | var | (   | )   | pi  | immediate | +   | -   | *   | /   | ^   | sin | cos | =   | ;   | ε   |
| --- | --- | --- | --- | --- | --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     | ♦   |
| E   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| E'  |     |     |     |     |           | ♦   | ♦   |     |     |     |     |     |     |     | ♦   |
| T   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| T'  |     |     |     |     |           |     |     | ♦   | ♦   |     |     |     |     |     | ♦   |
| F   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| G   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     | ♦   | ♦   |     |     |     |
| X   | ♦   | ♦   |     | ♦   | ♦         |     |     |     |     |     |     |     |     |     |     |
  
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
| F   |     |     |     |     |           |     |     | ♦   | ♦   |     |     |     |     |     |     |
| G   |     | ♦   |     |     |           |     |     |     |     | ♦   | ♦   | ♦   |     |     |     |
| X   |     |     |     |     |           |     |     |     |     |     |     |     | ♦   |     |     |
  
  ### Directeurs
1. A -> X = E ; A
2. A -> E
3. A -> ε
4. E -> X = E
5. E -> T E'
6. E'-> + T E'
7. E'-> - T E'
8. E'-> ε
9. T -> F T'
10. T'-> * F T'
11. T'-> / F T'
12. T'-> ε
13. F -> G ^ F
14. F -> G
15. G -> sin G
16. G -> cos G
17. G -> X
18. X -> var
19. X -> (E)
20. X -> immediate
21. X -> pi


|       |  var  |   (   |   )   |  pi   | immediate |   +   |   -   |   *   |   /   |   ^   |  sin  |  cos  |   =   |   ;   |   $   |
| :---: | :---: | :---: | :---: | :---: | :-------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|   A   |   1,2 |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|   E   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|  E'   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|   T   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|  T'   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|   F   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|   G   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |
|   X   |       |       |       |       |           |       |       |       |       |       |       |       |       |       |       |


## Difficultés rencontrées

Si vous en avez eu...

## Remarques éventuelles



