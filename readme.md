## remote

## ajouter une branche distante

``` bash
$ git remote add [nom_court] [url]
``` 
### quelques commandes utiles pour le remote

``` bash

# branches distantes
$ git remote -v

# toutes les branches

$ git branch -a

# supprimer un remote origin

$ git remote rm origin

# fetch pour récupérer en local l'historique de la branche sans fusionner

$ git fetch origin

# on peut comparer les différences entre la branche master locale et la branche origin/master

$ git log master..origin/master

# git pull équivalent à git fetch + git merge

$ git fetch origin
$ git merge origin/master
$ git oneline

``` 

