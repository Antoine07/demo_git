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

# voir toutes les branches non mergés
$ git branch --no-merged

# forcer la suppression d'une branche, non mergé donc, en perdant le travail dessus:
$ git branch -D ma_branche

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

```
## bisect

trouver le commit responsable d'un bug, méthode dichotomique

``` bash

# rechercher un commit à partir duquel on a pas de bug, noté le sha1
$ git checkout [sha1]

$ git checkout master

# démarrer le bisectage
$ git bisect start

# si la version courante est buguée
$ git bisect bad

# version non buguée
$ git bisect good [sha1]

# l'algo de bisectage commence
bisecting ...
[sha1]

# si version pour ce sha1 est encore bugué
$ git bisect bad
bisecting ...
[sha1]

# si encore bugué ...
$ git bisect bad

# si non bugué ...
$ git bisect good

etc...

# si git n'a plus de revision à tester, il arrive à la fin de la bisection il propose un commit
$ git show [sha1] # pour voir le problème...

# puis on arrête le bisectage
$ git bisect reset

# on fait un revert sur le sha1 trouvé on corrige les conflits et le bug si besoin
$ git revert [sha1]

```

## workflow

- initialisation du projet
``` bash

$ git init
# exécute une commande sans effet --dry-run, liste les fichiers stagés
$ git add --all --dry-run > list.txt
$ rm list.txt
$ touch .gitignore
$ git commit -m "C1: ajout du projet"
$ git tag v1.0.0

```
- création de la branche dev

``` bash

$ git checkout -b dev
# création d'un commit vide --allow-empty
$ git commit -m "C2: branche dev" --allow-empty

```
- envoie des deux branches créés sur le dépôt commits et réfs

``` bash
# option -u commits + réfs
$ git push -u -all

```
- exemple de développement d'une feature, pour Simon

``` bash

$ git pull --all
$ git branch -a
* master
remotes/origin/HEAD -> origin/master
remotes/origin/dev
remotes/origin/master
```
Attention, la branche dev n'est pas automatiquement créée, il faut faire localement

``` bash

$ git checkout remotes/origin/dev
$ git checkout -b dev

```

- création d'une feature à partir de la branch dev

``` bash
$ git branch
master
*dev
$ git checkout -b feature_routes
$ git commit -m "feature: task route" --allow-empty
# prévenir que l'on commence la feature sur la branch dev
$ git push -u origin feature_routes

```
- fin de la feature, Simon doit commiter

``` bash

$ git pull --all
$ git add .
$ git commit -m "feature: task route terminée"
$ git push -u --all

```
- revu de code par Antoine
si tout marche bien, Antoine va merger la nouvelle feature dans la branche dev

``` bash

$ git pull --all
$ git merge --no-ff feature_routes -m "feature: task route terminée -> branch dev"
$ git push -u origin dev

```
- suppression de la branche feature_routes par Simon

``` bash

$ git pull --all
$ git branch -d feature_routes
$ git push origin :feature_routes

```
- Création de la branche de version par Lead_Antoine, elle n'acceptera plus que des correctifs

``` bash

$ git pull --all
$ git checkout dev
$ git checkout -b release-v1.0
$ git commit --allow-empty -m "Release-v1.0"
$ git push -u origin release-v1.0

```

- Intégration de la release dans la branche master

``` bash

$ git pull --all
$ git checkout master
$ git merge --no-ff release-v1.0 -m "version stable v1.0"
$ git push 
$ git branch -d release-v1.0
$ git push origin :release-v1.0

```

## sous module

Dépôt intégré, pour ne pas polluer les commits et statistiques de notre dépôt