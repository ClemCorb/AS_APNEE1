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
A -> X = E ;  A | ε  
E -> X = E | E + T | E - T | T 
T -> T * F | T / F | F   
F -> G ^ F | G 
G -> sin G | cos G | X 

X -> var | (E) | Y | pi  
 

### G1 sans récursivité à Gauche
A -> X = E ; A | ε  
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
|   | var | ( | ) | pi | immediate | + | - | * | / | ^ | sin | cos | = | ; |  ε   
| A |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  ♦  
| E |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   | 
| E'|     |   |   |    |           | ♦ | ♦ |   |   |   |     |     |   |   |  ♦  
| T |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |    
| T'|     |   |   |    |           |   |   | ♦ | ♦ |   |     |     |   |   |  ♦  
| F |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |  ♦  |  ♦  |   |   |  
| G |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |  ♦  |  ♦  |   |   |   
| X |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  
  
### Suivants
Soit S la source tel que : 
S -> A$
|   | var | ( | ) | pi | immediate | + | - | * | / | ^ | sin | cos | = | ; | $ 
| A |     |   |   |    |           |   |   |   |   |   |     |     |   | ♦ | ♦  
| E |     |   | ♦ |    |           |   |   |   |   |   |     |     |   | ♦ |  
| E'|     |   |   |    |           |   |   |   |   |   |     |     |   | ♦ |  
| T |     |   |   |    |           | ♦ | ♦ |   |   |   |     |     |   |   |  
| T'|     |   |   |    |           |   |   |   |   |   |     |     |   | ♦ |  
| F |     |   |   |    |           |   |   | ♦ | ♦ |   |     |     |   |   |  
| G |     | ♦ |   |    |           |   |   |   |   | ♦ |  ♦  |  ♦  |   |   |  
| X |     |   |   |    |           |   |   |   |   |   |     |     | ♦ |   |  
  
  ### Directeurs
1. A -> X = E ; A
2. A -> ε
3. E -> X = E
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
17. X -> var
18. X -> (E)
19. X -> immediate
20. X -> pi

|   | var | ( | ) | pi | immediate | + | - | * | / | ^ | sin | cos | = | ; | $ 
| A | 17  |   |   | 20 |     19    |   |   |   |   |   |   |   | 1 |   | 2 |
| E | 17  |   |   | 20 |     19    |   |   |   |   |   |   |   | 3 |   |   |
| E'|     |   |   |    |           | 5 | 6 |   |   |   |   |   | 7 | 7 | 7 |
| T | 17  |   |   | 20 |     19    |   |   |   |   |   |   |   | 8 |   |   |
| T'|     |   |   |    |           |   |   | 9 | 10| 11|   |   | 11| 11| 11| 
| F | 17  |   |   | 20 |     19    |   |   |   |   | 12|   |   | 13|   |   |
| G | 17  |   |   | 20 |     19    |   |   |   |   |   | 14| 15| 16|   |   |
| X | 17  | 18|   | 20 |     19    |   |   |   |   |   |   |   |   |   |   |


## Difficultés rencontrées

Si vous en avez eu...

## Remarques éventuelles

Un document PDF pour le rendu serait plus adapté, permettant directement de mettre des "beaux" tableaux pour les Premiers, Suivants, et Directeurs


