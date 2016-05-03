# Git aide

Git est décentralisé et distribué

Les objets de Git: blob, tag, commit, tree

Un commit possède un parent, un ancêtre, Git est un graphe orienté

## commandes de plomberies

``` bash
$ find .git\objects\ -type f # objets Git

$ git cat-file -p 3c65 # décompresse un objet Git

```

## initialisation d'un dépôt

``` bash

$ git init # creation du dossier .git 
$ git add .
$ git commit -m "first commit"

# un fichier créer dans le dépôt n'est pas tracker par défaut, il faut l'ajouter à la staging une première fois
$ touch readme.md
$ git add readme.md && git commit -m "ajout readme"

# une fois tracker le fichier readme, ajout dans la staging puis commité

$ git commit -am "modification contenu readme"

``` 

## message de commit règle

- un titre de 49 caractères max

``` bash

$ git commit -m "étudiant/tp: si terminé, attribuer note"

# ouvrir l'éditeur et mettre un titre et texte

$ git commit

```

## commandes d'aides 

``` bash

$ git status
$ git help [commande]
$ git log 
$ git oneline # voir alias dans la configuration
$ git log master..origin/master # voir les différences entre deux branches
$ git log -5 # 5 derniers commits
$ git log -p -5 # log avec différence pour chaque commit 5 derniers
$ git log --stat # stat sur les modif par commits
$ git log --since=2.weeks # depuis deux semaines, --until existe également

```

## tags

taguer un commit, tag de version

``` bash
# tag annoté
$ git tag -a v1.0 -m "version 1 de l'application"
# tag léger peu utilisé
$ git tag v1

# étiquetté après coup
$ git tag -a v1.0.1 -m "version 1.0.1" 9fceb2

```

## branches

Une branche n'est rien d'autre qu'un pointeur sur commit

``` bash

# voir les réf branch dans le dossier .git
$ cat .git/refs/heads/master

# création
$ git branch dev 
# ou git checkout -b dev crée la branche et s'y place

```

Git fait un fast-foward sur la branche master et dev par exemple quand tous les commits de la branches master sont atteignables depuis la branche dev,
sinon Git fait un commit de merge

``` bash

$ git checkout master
$ git merge dev

# forcer un commit de merge sur un fast-foward

$ git merge dev --no-ff

# supprimer une branche 

$ git branch -d dev

```


## annulation


## remote

## ajouter une branche distante

Ajouter une branche distante (remote) dans son dépôt local

``` bash
$ git remote add [nom_court] [url]
``` 
### Commandes remotes

``` bash

# branches distantes
$ git remote -v

# toutes les branches

$ git branch -a

# respectivement supprimer, renommer un remote

$ git remote rm origin

$ git remote rename origin dev 

# fetch récupérer la branche distante localement sans fusion avec sa branche master

$ git fetch origin

# Comparer les différences entre master local et distant (après un fetch)

$ git log master..origin/master

# git pull équivalent à git fetch + git merge

$ git fetch origin
$ git merge origin/master
# pour vérifier
$ git oneline

