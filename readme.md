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

$ git init # création du dossier .git 
$ git add .
$ git commit -m "first commit"

# un fichier créé dans le dépôt n'est pas tracké par défaut, il faut l'ajouter à la staging area une première fois

$ touch readme.md
$ git add readme.md && git commit -m "ajout readme"

# une fois tracké le fichier readme, ajout dans la staging area puis commité

$ git commit -am "modification contenu readme"

``` 

## message de commit règles

- un titre de 49 caractères max
- un texte plus long
- on peut commiter qu'avec un titre option -m

``` bash

$ git commit -m "étudiant/tp: si terminé, attribuer note"

# ouvrir l'éditeur et mettre un titre et texte plus long

$ git commit

```

## commandes d'aide 

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

# étiquetter après coup
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

Git fait un fast-foward sur la branche master et dev par exemple quand tous les commits de la branche master sont atteignables depuis la branche dev,
sinon Git fait un commit de merge

``` bash

$ git checkout master
$ git merge dev

# forcer un commit de merge sur un fast-foward

$ git merge dev --no-ff

# supprimer une branche 

$ git branch -d dev

```

## gestion de conflit

Pour fusionner deux branches Git repère le dernier commit commun entre les deux branches et utilise les modifs des deux branches.

Il analyse trois version différente du dépôt:

- version dernier commit
- version de la première branche à fusionner actuelle
- version de la deuxième branche à fusionner actuelle

Il faut résoudre chaques conflits, lorsque deux versions du même fichier on été modifié aux mêmes endroits.

``` bash
tagtagtag HEAD 
	Music Pi 
tagtagtag
	SONIC 
tagtagtag branchB  

$ git add . 
$ git commit # message de merge par défaut

```
On peut avoir plusieurs conflits à gérer dans plusieurs fichiers

## reset annulation
Commande reset modifie l'historique, il ne faut jamais le faire sur des commits déjà publiés!

``` bash
# annule le dernier commit et met tout le WD sans perte
$ git reset HEAD~ 

# annule le dernier commit et met tout dans la staging area sans perte
$ git reset --soft HEAD~

# annule le dernier commit et supprime les modifications...(danger)
$ git reset --hard HEAD~

# retire un fichier de la staging area, sans perte de modif
$ git add [fileName]
$ git reset HEAD [fileName]

# on peut faire un git reset sur un commit, attention tous les commits suivants le commit annulé seront perdus:
$ git reste  f597d47552d 

...

```

## checkout

### retour en arrière sans rien changer

``` bash
# mettre le dépôt tel qu'il était lors du commit f597d47552d, le pointeur se déplace sur le commit
$ git checkout f597d47552d
# pour remettre le pointeur sur le dernier commit
$ git checkout master

```

### modifier un fichier dans l'historique

``` bash
# le fichier index.html est stagé dans l'état dans lequel il se trouvait pour ce [sha1]
$ git checkout [sha1] index.html

$ git commit -m "retour en arrière pour le code dans index.html"

```

### revert

défaire ce qui a été fait, cela peut entrainer des conflits qu'il faudra résoudre

``` bash
# crée un commit qui annulera ce qui a été fait pour le commit 12916f5
$ git revert 12916f5

```

## amend

Dans le cas où on a oublié du code dans un commit

``` bash

# on ajoute le code qui manquait
vim index.html

$ git add index.html
$ git commit --amend # associe les changements au dernier commit, le message du dernier commit s'ouvrira dans l'éditeur par défaut

# si on souhaite uniquement modifier le message du commit

$ git commit --amend

```

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

