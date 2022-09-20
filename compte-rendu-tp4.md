*Raphaël DUMAS - 3ICS*

# TP4 - Gestion des paquets

## Exercice 1 - Commandes de base

1. On commence par mettre le système à jour via `sudo apt update` et `sudo apt upgrade`

2. On crée alors un alias pour les commandes ci-dessus avec `alias maj="sudo apt update -y ; sudo apt upgrade -y"`

3. On cherche à obtenir le nom des 5 derniers paquets installés avec le DPKG. On utilise alors la commande `tail -5 /var/log/dpkg.log`

4. Via la commande `apt list -i` on obtient la liste des tous les paquets installés via apt.

5. On cherche le nombre de paquets installés. Avec la commande `dpkg -l | grep "^ii" | wc -l`. On voit alors qu'il y a 756 paquets installés. On teste une variante avec `sudo apt list -i | wc -l`. Le résultat est alors de 757 paquets.

6. Si on cherche le nombre de paquets disponibles sur tous les dépôts Ubuntu, il faut entrer la commande `sudo apt list | wc -l`, qui nous retourne alors un total de 68744 paquets.

7. Le package glance permet d'afficher plus de données système. Le paquet tldr permet d'avoir un affichage résumé et plus propre des pages de manuel pour chaque commande. Le package hollywood permet de simuler une fenêtre de hacker, comme au cinéma.

8. Grâce à l'auto-complétion, lorsque l'on cherche un package, si l'on fait `sudo apt install sudok` et que l'on appuie sur tab, le shell nous propose tous les packages commençant par "sudok". On découvre alors l'existence du package "sudoku".

## Exercice 2

Grâce à la commande `dpkg -S /bin/ls`, on trouve que le paquet à l'origine de cette commande est "coreutils". Si on veut obtenir le résultat avec une seule commande, il faut entrer `which -a ls | xargs dpkg -S | cut -d ":" -f1`. On peut donc aller créer un script "origine-commande" :

```bash
#!/bin/bash

which -a $1 | xargs dpkg -S 2> /dev/null | cut -d':' -f1
```
