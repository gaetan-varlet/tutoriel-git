# Git, logiciel de gestion de versions


## Introduction

Un logiciel de gestion de version permet de suivre l'évolution d'un fichier texte ligne par ligne
et garde les anciennes versions de chacun d'eux.  
Il permet de savoir qui a effectué chaque modification, quand et pourquoi, il permet d'assembler (fusionner)
des modifications si deux personnes ont travaillé sur le même fichier sans perdre d'information.


## Logiciels centralisés versus logiciels distribués

* **centralisés** : un serveur conserve l'historique des versions des fichiers et les utilisateurs ont sur leur poste
uniquement la dernière version des fichiers
  * exemple : CVS, SVN
* **distribués** : il n'y a pas de serveur, chacun possède l'historique de l'évolution des fichiers. Les développeurs se transmettent
chacun directement les modifications.  
Dans la pratique, pour les logiciels distribués, on utilise souvent un serveur qui sert de point de rencontre et d'échange entre
les utilisateurs, qui eux possèdent également l'historique des modifications
  * exemple : Mercurial, Bazaar, Git

Pour commencer à utiliser Git, on peut :
* créer un nouveau dépôt, pour commencer un nouveau projet
* cloner un dépôt existant, c'est-à-dire récupérer l'historique des changements d'un projet pour travailler dessus.


## Les commandes de base

* `git clone lienFourniParGitHub ` : copie un repository depuis un serveur sur notre machine en SSH ou en HTTPS, ici avec le lien HTTPS du dépôt.  
Attention à se placer au bon endroit sur le disque dur pour copier le reposity.

* `git status` : permet de voir l'état du dépôt local, notamment voir les fichiers que l'on a récemment modfié

* `git pull` : récupère des modifications sur le serveur, penser à se mettre dans le dossier

* ajouter un fichier à l'index Git.  
Pour gérer un repository, Git génère un index de tous les fichiers dont il doit faire le suivi.
Lorsque qu'on crée un fichier dans un repository, on doit donc l'ajouter à l'index Git à l'aide de la commande suivante
`git add nomFichier.extension`.
Pour ajouter tous les fichiers dans le répertoire courant, on peut utiliser la commande suivante `git add .`

* enregistrer des modifications : ` git commit -m "ajout du fichier nomFichier.extension"`.
-m permet de décrire les modifications effectués.
Un commit est local, personne ne sait qu'il a été fait, ce qui donne la possibilité de l'annuler.

* faire un commit d'un fichier qui a déjà été modifié (qui est déjà dans l'index Git) sans faire `git add .` :
`git commit -a -m "modification du fichier nomFichier.extension"`.
-a demande à Git de mettre à jour les fichiers déjà existants dans son index

* `git push` envoie le code sur le dépôt distant. Il faut se positonner dans le repo local.  
Tous les commits du dépôt local sont envoyés vers le dépôt distant.
La commande complète est `git push [nom-distant] [nom-de-branche]`, par exemple `git push origin master`.  


## Créer un nouveau projet

* `git init` : activer un dossier comme repository Git en se plaçant dans le dossier
Cela crée un dossier caché *.git* à la racine du projet qui stocke l'historique des fichiers et la configuration

* `git remote add origin https://github.com/nomutilisateur/MonProjet.git` : permet de connecter le dépôt local au dépôt distant après le `git init` que l'on vient de créer vide sur GitHub.

* `git branch --set-upstream-to=origin/master` permet d'associer la branche master locale à la branche master du remote

* `git pull` pour récupérer les éventuels fichiers créé avec le projet (README.md et .gitignore). S'il n'y a pas de fichier .gitignore, il faut en créer un pour lister tous les fichiers qui ne doivent pas être versionné.

* `git add .` pour ajouter à l'index Git tous les fichiers créés ou modifiés

* `git commit -m "premier commit du projet"` : crée une version du projet

* `git push` : Lors du premier push, il faut associer la branche master locale avec la branche master du remote. Si la commande `git branch --set-upstream-to=origin/master` n'a pas été utilisé, on peut aussi faire lors du premier push `git push --set-upstream origin master`. Après cela, on peut faire un simple `git push`, sinon on est obligé de faire `git push origin master`


## Les commandes "avancées"

* `git diff` : permet de voir précisément ce qui a été modifié dans tous les fichiers où il y a eu des modifications,
les lignes ajoutés sont précédés d'un +, les lignes supprimés précédés d'un -.  
`git diff fichier` permet de ne voir les modifications que d'un fichier

* `git log` : afficher la liste de tous les commits réalisés.  
Chaque commit est identifié grâce à un numéro hexadécimal de 40 caractères nommé *SHA-1*.  
-p pour le détail de chaque ligne ajoutée ou retirée.  
--stat pour un résumé plus court des commits.  
Appuyer sur *Q* pour quitter

* `git checkout` a un double usage  
  * `git checkout SHADuCommit` : se positionne sur un commit donnée ou dans une branche donnée.  
  revenir à la branche principale (commit le plus récent) : `git checkout master`
  * `git checkout nomFichier` : le fichier redeviendra comme il était lors du dernier commit

* `git revert SHADuCommit` : "annule un commit" en créant un nouveau commit qui fait l'inverse du précédent

* `git commit --amend -m "nouveau message"` : modifie le message du dernier commit s'il n'a pas encore été pushé

* `git reset HEAD^` : annule le dernier commit et revient à l'avant-dernier.  
Pour savoir à quel commit revenir, `HEAD` pour le dernier, `HEAD^` pour l'avant-dernier, `HEAD^^` ou `HEAD~2`
pour l'avant-avant-dernier, ou la clé SHA-1 du commit précis.  
Seul le commit est retiré de Git, les fichiers eux, restent modifiés. On peut alors les remodifier et refaire un commit

* `git reset --hard` : annuler tous les changements depuis le dernier commit **et**
les changements effectués dans les fichiers.  

* `git blame nomDuFichier.extension` liste les modifications faites sur un fichier et qui les a fait quand.  
Pour savoir pourquoi cette modificaiton a été faite :
  * `git log` avec le SHA du commit
  * `git show debutDuSHA` qui renvoie les détails du commit

* `git stash` met de côté une modification en cours sans faire de commit, pour par exemple aller dans une autre branche.  
A ce moment `git status` ne doit plus afficher aucun fichier en cours de modification.
Pour reprendre lors du retour dans la branche et récupérer les modifications mises de côté, faire `git stash pop`.  
Attention, **pop** vide le stash des modifications qu'on a mis dedans. Si on veut garder les modifications dans le stash,
on peut utiliser à la place `git stash apply`.  
Si on change de branche avec des changements "non commités", les fichiers modifiés resteront comme ils étaient dans
la nouvelle branche, ce qui n'est pas ce qu'on souhaite en général.

* `git remote -v` : voir la liste des dépôts distants.    
Il doit au moins y avoir `origin`, qui est le nom par défaut donné par Git au serveur
à partir duquel on a cloné le projet.  
-v permet d'afficher l'URL que Git a stocké pour chaque nom court

* `git tag` permet de lister les tags existants.  
`git tag nomTag idCommit` ajoute un tag sur un commit, par exemple `git tag v1.3 2f7c8b3428aca535fdc970feeb4c09efa33d809e`.  
`git show v1.3` permet de voir à quoi correspond cette version.  
Un tag n'est pas envoyé lors d'un push, il faut le préciser avec l'option --tags : `git push --tags`.  
`git tag -d NOMTAG` permet de supprimer un tag créer.  

* ignorer des fichiers : créer le fichier **.gitignore** et y lister ligne par ligne
les fichiers qu'on ne veut pas versionner dans Git, en indiquant leurs chemins complets.
Il est possible d'utiliser une étoile comme joker. Par exemple :
```
motsdepasse.txt
config/application.yml
*.tmp
cache/*
```


## Les branches

Lors de l'initialisation du repo Git, le code est par défaut dans la branche principale appelée **master**

* `git branch` affiche la liste des branches (une étoile est placée devant la branche dans laquelle on est)

* `git branch nouvelle-branche` crée une nouvelle branche
une branche créée est locale, il est ensuite possible de la publier

* `git push --set-upstream origin nouvelle-branche` publie la nouvelle branche sur le remote.  
L'option --set-upstream permet de dire à Git de se souvenir qu'un « git push » de notre branche « nouvelle-branche » envoie les changements à la branche « nouvelle-branche » du dépôt distant. Le prochain push pourra donc se faire simplement avec un `git push`

* `git checkout nouvelle-branche`   permet de se placer dans une autre branche à l'intérieur du repo
A noter que lorsqu'on fait `git log`, on ne voit que les commits effectués sur la branche sur laquelle on se trouve

* fusionner des branches, par exemple ajouter dans une brancheA les mises à jour faites dans le master.
On se place dans A (`git checkout brancheA`) et on fusionne : `git merge master -m "merge du master dans la brancheA"`

* résoudre les conflits : si en fusionnant Git signale qu'il y a un conflit, il faut ouvrir le fichier en question dans l'éditeur de texte, par exemple Vim (`vim nomFichier.extension`), et choisir quel contenu garder, sauvegarder le fichier et revenir à la console.  
Une fois le conflit résolu, il faut le dire à dire en faisant un commit sans message (`git commit`), ce qui va permettre à Git de voir que le conflit est résolu et il va proposer un message par défaut qu'on peut personnaliser. On le sauvegarde en tapant `:x`. Git confirme ensuite que les branches sont fusionnés.

* `git branch -d nom-branche` supprime une branche locale

* `git push -d origin  nom-branche` supprime une branche sur le remote
