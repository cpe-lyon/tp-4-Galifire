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
## Exercice 3

```bash
#!/bin/bash

if dpkg -l | grep -q $1 ; then
        echo "INSTALLÉ"
else
        echo "NON INSTALLÉ"
fi
```

## Exercice 4

Afin de lister tous les programmes livré par un dépot, ici coreutils, il faut entrer la commande `dpkg -L coreutils`

## Exercice 5 - Aptitude

Pour installer des paquets via aptitude, il convient d'abord d'installer le package aptitude `sudo apt install aptitude` qui offre ensuite une interface graphique de gestion des paquets : installés, non installés, gestion, recherche, etc...
Emacs est un éditeur de texte auto indenté, et lynx est un navigateur interne.

## Exercice 6 - - Installation d'un paquet par PPA

1. Si l'on cherche à installer des paquets introuvables dans les dépôts officiels, il faut passer par PPA, un dépôt "personnel"
Il faut alors utiliser les commandes suivantes :
`sudo add-apt-repository ppa:linuxuprising/java`, `sudo apt update`, `sudo apt install oracle-java15-installer`

2. Un fichier a bien été ajouté au dossier /etc/apt/sources.list.d/, le fichier : linuxuprising-ubuntu-java-jammy.list

## Exercice 7 - Installation d'un logiciel à partir du code source

1. On clone d'abord le dépôt git cbonsai.

2. On commence ensuite par installer les librairies manquantes : `sudo apt install libncursesw5-dev` et `sudo apt install pkg-config`
On exéctute ensuite les commandes fournies par `make install PREFIX=~/.local` : `mkdir -p /home/tp/.local/bin`, `mkdir -p /home/tp/.local/share/man/man1`, `install -m 0755 cbonsai /home/tp/.local/bin/cbonsai`. Le bonsai est installé, on peut ensuite l'exécuter.

3. On veut ensuite le transformer nous même en paquet, il faut donc installer le package checkinstall. `sudo apt install checkinstall -y`

4. Un fichier à été créé, on peut utiliser le bonsai depuis n'importe quel dossier !

## Exercice 8 - Création de dépôt personnalisé

### Création d'un paquet Debian avec dpkg-deb

