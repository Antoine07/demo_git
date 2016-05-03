## configuration Git

Git est décentralisé et distribué

Définir un utilisateur (obligatoire)

``` bash
# dans le fichier .gitconfig
$ git config --global user.name Tony 
$ git config --global user.email tony@tony.fr

# pour un dépôt en particulier, dans le fichier config du dossier .git/
$ git config --local user.name Tony 
$ git config --local user.email tony@tony.fr

``` 

### message de commit règle

- un titre de 49 caractères max

``` bash

$ git commit -m "étudiant/tp: si terminé, attribuer note"

# ouvrir l'éditeur et mettre un titre et texte

$ git commit

```

### alias

``` bash

# ajouter un alias à la configuration globale
$ git config config --global alias.changes "diff --name-status"
# localement ajouter --local à la place de --global

# dans le fichier .gitconfig voici mes alias

[user]
	email = antoine.lucsko@wanadoo.fr
	name = Antoine07
[alias]
	amend = commit --amend
    st = status
    who = shortlog -sne
	oneline = log --pretty=oneline --abbrev-commit --graph
    changes = diff --name-status
    dic = diff --cached
    diffstat = diff --stat
    co = checkout
    ci = commit
    br = branch
	last = log -1 HEAD
	unstage = reset HEAD --
[core]
	autocrlf = false
	whitespace = cr-at-eol
	editor = subl --wait
[man]
	viewer = chrome
[web]
	browser = firefox
[status]
	showUntrackedFiles = all
[pull]
	rebase = true

``` 


## configuration serveur avec GitHub

En SSH (secure shell), clé privée et clé publique, crypto asymétrique, la clé publique est distribuée la clé privée reste sur sa machine (ne jamais la diffuser).
Principe: la clé publique crypte le message, tout le monde peut crypter un message. La clé privée permet de décrypter le message, seul celui qui possède la clé privée peut décoder le message.

La clé publique est à mettre sur GitHub dans les settings

### création de la clé

Une passe phrase sera demandée, elle est facultative, on ne le fera pas, la pass phrase est un mot de passe protégeant la clé. Si on vous vole la clé privée, et que vous avez défini une passe phrase personne ne pourra l'utiliser.

``` bash
# création d'une paire de clé publique et privée
$ ssh-keygen -t rsa -b 4096 -C "antoine.lucsko@gmail.com"

$ cd ~

$ ls -al .ssh/
id_rsa
id_rsa.pub

``` 

Une fois la clé sur le serveur GitHub vérifier que GitHub vous connaît:

``` bash
$ ssh -T git@github.com
Hi Antoine07! You've successfully authenticated, but GitHub does not provide shell access.

```