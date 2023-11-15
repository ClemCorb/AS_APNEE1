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
A -> X = E ; '\n' A | ε  
E -> X = E | E + T | E - T | T 
T -> T * F | T / F | F   
F -> F ^ X | G 
G -> sin X | H 
H -> cos X | X  

X -> var | (A) | Y | pi  
 

### G1 sans récursivité à Gauche
A -> X = E ; '\n' A | ε  
E -> X = E | T E'  
E'-> + T E' | - T E' | ε  
T -> F T'  
T'-> * F T' | / F T' | ε  
F -> G F'  
F'-> ^ X | ε  
G -> sin G | H  
H -> cos H | X  

X -> var | (A) | immediate | pi  


## Table LL1 calculée

Donnez la table LL1 de votre grammaire.
### Premiers  
|   | var | ( | ) | pi | immediate | + | - | * | / | ^ | sin | cos | = | ; |   
| A |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  
| E |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   | 
| E'|     |   |   |    |           | ♦ | ♦ |   |   |   |     |     |   |   |   
| T |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  
| T'|     |   |   |    |           |   |   | ♦ | ♦ |   |     |     |   |   |  
| F |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  
| F'|     |   |   |    |           |   |   |   |   | ♦ |     |     |   |   |  
| G |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |  ♦  |     |   |   |  
| H |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |  ♦  |   |   |  
| X |  ♦  | ♦ |   | ♦  |     ♦     |   |   |   |   |   |     |     |   |   |  
  
### Suivants
Soit S la source tel que : 
S -> A$
|   | var | ( | ) | pi | immediate | + | - | * | / | ^ | sin | cos | = | ; | $ 
| A |     |   | ♦ |    |           |   |   |   |   |   |     |     |   |   | ♦  
| E |     |   |   |    |           |   |   |   |   |   |     |     |   | ♦ |  
| E'|     |   |   |    |           | ♦ | ♦ |   |   |   |     |     |   |   |  
| T |     |   |   |    |           |   |   |   |   |   |     |     |   |   |  
| T'|     |   |   |    |           |   |   | ♦ | ♦ |   |     |     |   |   |  
| F |     |   |   |    |           |   |   |   |   |   |     |     |   |   |  
| F'|     |   |   |    |           |   |   |   |   | ♦ |     |     |   |   |  
| G |     |   |   |    |           |   |   |   |   |   |  ♦  |     |   |   |  
| H |     |   |   |    |           |   |   |   |   |   |     |  ♦  |   |   |  
| X |     |   |   |    |           |   |   |   |   |   |     |     | ♦ |   |  
  

## Difficultés rencontrées

Si vous en avez eu...

## Remarques éventuelles

Si nécessaires...
