# Git, logiciel de gestion de versions


## Introduction

### La gestion de version

Un gestionnaire de version est un système qui enregistre l’évolution d’un fichier ou d’un ensemble de fichiers au cours du temps de manière à ce qu’on puisse rappeler une version antérieure d’un fichier à tout moment. Un **système de gestion de version** (**VCS** en anglais pour *Version Control System*) permet de savoir qui a effectué chaque modification, quand et pourquoi. Il permet d'assembler des modifications si deux personnes ont travaillé sur le même fichier sans perdre d'information.

Les différents types de VCS :
- **locaux** : une base de données locale conserve les différences versions ou les différences entre versions. Exemple : *RCS*
- **centralisés**, ou **CVCS** pour *Centralized VCS* : un serveur conserve l'historique des versions des fichiers et les utilisateurs ont sur leur poste
uniquement la dernière version des fichiers. Exemples : *CVS, SVN, Perforce*
- **distribués**, **DVCS** pour *Distributed VCS* : comme pour les CVCS, un serveur conserve l'historique. Cependant, les clients n’extraient plus seulement la dernière version d’un fichier, mais ils dupliquent complètement le dépôt. Exemples : *Git, Mercurial, Bazaar, Darcs*

### Rudiments de Git

- Git a été créé en 2005 par Linus Torvalds pour gérer le code source du noyau Linux
- Git gère et stocke les données comme des instantanés, alors que la plupart des VCS gèrent et stockent des différences de fichiers. Pour être efficace, Git ne stocke pas les fichiers qui n'ont pas changé, mais juste une référence vers l'instantané
- Presque toutes les opérations sont locales car l'historique complet est stocké localement, alors qu'avec les CVCS, les opérations passent par le réseau ce qui les ralentit
- Git gère l'intégrité. Tout est vérifié par une somme de contrôle (empreinte SHA-1 de 40 caractères hexadécimaux) avant d'être stocké, qui sert de référence, ce qui rend impossible la modification d'un fichier ou dossier sans que Git ne s'en aperçoive
- Les trois états dans Git :
  - **modifié**, ou *modified*, signifie que le fichier est modifié mais qu’il n’a pas encore été validé en base
  - **indexé**, ou *staged*,  signifie que le fichier modifié dans sa version actuelle est marqué pour qu’il fasse partie du prochain instantané du projet
  - **validé**, ou *committed*, signifie que les données sont stockées en sécurité dans la base de données locale
- Les trois sections d'un projet Git :
  - le répertoire de travail, qui est une extraction unique d'une version du projet, extrait depuis la base de données compressée dans le répertoire Git
  - la zone d'index, qui est un simple fichier, situé dans le répertoire Git, qui stocke les informations concernant ce qui fera partie du prochain instantané
  - le répertoire Git, est l'endroit où Git stocke les méta-données et la base de données des objets du projet
- L'utilisation de Git peut se faire de différentes manières : en ligne de commande ou en utilisant une interface graphique
- Paramètrage à la première utilisation
  - `git config` permet de voir et modifier les variables de configuration
  - ces variables sont stockées dans */etc/gitconfig* pour tout le système, dans *~/.gitconfig* pour l'utilisateur, et dans le répertoire Git *.git/config* pour le projet. Chaque niveau surcharge le niveau précédent
  - sur Windows, Git recherche *.gitconfig* dans le répertoire *$HOME*
  - configuration de l'identité : `git config --global user.name "John Doe"` et `git config --global user.email johndoe@example.com` permet de renseigner l'identité car toutes les validations Git utilisent cette information. L'option `--global` fait que l'information est enregistré pour tous les projets de l'utilisateur sur la machine. Pour surcharger une valeurs pour un projet spécifique, il faut utiliser la commande sans l'otion `--global`
  - choisir l'éditeur de texte pour saisir les messages : `git config --global core.editor emacs`
- Pour vérifier les paramètres, utiliser la commande `git config --list`, certains paramètres apparaissent plusieurs fois car Git lit les paramètres depuis plusieurs fichiers, la dernière valeur est celle utilisée par Git. Pour vérifier la valeur effective d'un paramètre, utiliser la commande `git config <paramètre>`, par exemple `git config user.name`
- Pour obtenir de l'aide, utiliser la commande `git help <commande>`, par exemple `git help config`


## Les bases de Git

Pour commencer à utiliser Git, on peut :
- créer un nouveau dépôt, pour commencer un nouveau projet : `git init` en se positionnant dans le répertoire du projet
- cloner un dépôt existant, c'est-à-dire récupérer l'historique des changements d'un projet pour travailler dessus : `git clone <url>`

- `git clone <url>` : copie un dépôt Git depuis un serveur sur notre machine en SSH ou en HTTPS
  - attention à se placer au bon endroit sur le disque dur pour copier le dépôt
  - `git clone <url> nomRepertoire` pour cloner le dépôt dans un répertoire nommé différemment

- `git status` : permet de voir l'état des fichiers dans le dépôt local
  - l'état des fichiers : *sous suivi de version* (*tracked*) ou *non suivi* (*untracked*)
    - les fichiers suivis peuvent être *inchangés*, *modifiés* ou *indexés*
    - tous les autres sont non suivis : tous ceux qui n'appartenait pas au dernier instantané et n'ont pas été indexé
  - en éditant les fichiers, Git les considère modifiés. On indexe les fichiers modifiés et on enregistre ces modifications indexés, et le cycle se répète

- `git add` permet d'ajouter un fichier pour la prochaine validation. En effet, pour gérer un dépôt, Git gère un index de tous les fichiers dont il doit faire le suivi
  - `git add cheminFichier` permet d'ajouter un fichier non suivi (que l'on vient de créer) à l'index Git
    - `git add cheminRépertoire` ajoute récursivement tous les fichiers de ce répertoire
  - `git add cheminFichier` permet aussi d'indexer les fichiers modifiés
  - `git add .` indexe tous les fichiers dans le répertoire courant

- ignorer des fichiers : créer le fichier **.gitignore** et y lister ligne par ligne les fichiers qu'on ne veut pas versionner dans Git, en indiquant leurs chemins complets. Il est possible d'utiliser une étoile comme joker. Par exemple :
```
motsdepasse.txt
config/application.yml
*.tmp
cache/*
```

- `git diff` permet d'inspecter les modifications indexées et non indexées
  - `git diff` permet de visualiser ce qui a été modifié mais pas encore indexé
  - `git diff --cached` ou `git diff --staged` permet de visualiser les modifications indexées qui feront partie de la prochaine validation
  - `git diff fichier` permet de ne voir les modifications que d'un fichier

- `git commit` enregistre les modifications indexées :
  - cela lance l'éditeur par défaut pour que l'on renseigne le message de validation
  - on peut voir les fichiers modifiés qui vont être enregistrés. `-v` ou `--verbose` permet de voir dans chaque fichier ce qui est modifié
  - tous les fichiers qui ont été créés ou modifiés mais n’ont pas subi de git add depuis leur modification ne feront pas partie de la prochaine validation
  - `git commit -m "ajout du fichier nomFichier"`, ou `-m` ou `--message` permet de décrire les modifications effectués
  - l'ajout de l’option `-a` à la commande git commit ordonne à Git de placer automatiquement tout fichier déjà en suivi de version dans la zone d’index avant de réaliser la validation, évitant ainsi d’avoir à taper les commandes git add : `git commit -a -m "modification du fichier nomFichier"`. Attention, ne fonctionne pas pour les fichiers non suivis !

- effacer des fichiers
  - `rm fichier` efface juste le fichier de la copie de travail mais cette modification n'est pas indexé, il faut ensuite faire un git add
  - `git rm fichier` efface le fichier de la copie de travail et l'efface aussi dans la zone d'index, l'effacement du fichier est donc indexé
  - si un fichier a été modifié et indexé, il faut forcer sa suppression avec l'option `-f`

- déplacer/renommer des fichiers
  - `git mv nomOrigine nomCible` renomme et indexe le fichier
  - si on renomme manuellement le fichier, Git va voir un fichier supprimé et un nouveau fichier, il faut donc en plus supprimer l'ancien nom et ajouter le nouveau nom dans la zone d'index, ce qui fait 3 étapes mais revient au final au même car Git voit que c'est un renommage : `mv nomOrigine nomCible` `git rm nomOrigine` `git add nomCible`

- `git pull` : récupère des modifications sur le serveur, penser à se mettre dans le dossier

- `git push` envoie le code sur le dépôt distant. Il faut se positonner dans le repo local.  
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

* `git reset --hard` : annuler tous les changements depuis le dernier commit **et** les changements effectués dans les fichiers.  

* `git blame nomDuFichier.extension` liste les modifications faites sur un fichier et qui les a fait quand.  
Pour savoir pourquoi cette modification a été faite :
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
`git push --tags` permet de pusher les tags sur le dépôt distant



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
