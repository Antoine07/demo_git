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

## git diff

$ git diff

## git log

$ git log

``` 