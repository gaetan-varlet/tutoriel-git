# tutoriel-git

## Les commandes de base

* `git status` : voir l'état du dépôt local

* copier un repository sur notre machine (avec le lien HTTPS du dépôt) : `git clone lienFourniParGitHub `.  
Attention à se placer au bon endroit sur le disque dur pour copier le reposity.

* récupérer des modifications : se mettre dans le dossier et utiliser la commande suivante `git pull`

* activer un dossier comme repository Git en se plaçant dans le dossier : `git init`

* ajouter un fichier à l'index Git.  
Pour gérer un repository, Git génère un index de tous les fichiers dont il doit faire le suivi. 
Lorsque qu'on crée un fichier dans un repository, on doit donc l'ajouter à l'index Git à l'aide de la commande suivante 
`git add nomFichier.extension`. 
Pour ajouter tous les fichiers dans le répertoire courant, on peut utiliser la commande suivante `git add .`

* enregistrer des modifications : ` git commit -m "ajout du fichier nomFichier.extension"`. 
-m permet de décrire les modifications effectués

* faire un commit d'un fichier qui a déjà été modifié (qui est déjà dans l'index Git) sans faire `git add .` : 
`git commit -a -m "modification du fichier nomFichier.extension"`. 
-a demande à Git de mettre à jour les fichiers déjà existants dans son index

* envoyer le code sur le dépôt distant : `git push` en se plaçant dans le repo local.  
Tous les commits du dépôt local sont envoyés vers le dépôt distant.
La commande complète est `git push [nom-distant] [nom-de-branche]`, par exemple `git push origin master`

* afficher la liste de tous les commits réalisés : `git log`

* se positionner sur un commit donnée `git checkout SHADuCommit`.  
revenir à la branche principale (commit le plus récent) : `git checkout master`

* "annuler un commit" en créant un nouveau commit qui fait l'inverse du précédent : `git revert SHADuCommit`

* modifier le message du dernier commit s'il n'a pas encore été pushé : `git commit --amend -m "nouveau message"`

* annuler tous les changements depuis le dernier commit : `git reset --hard`

* lister les modifications faites sur un fichier et qui les a fait quand : `git blame nomDuFichier.extension`.  
Pour savoir pourqoi cette modificaiton a été faite :
  * `git log` avec le SHA du commit
  * `git show debutDuSHA` qui renvoie les détails du commit

* ignorer des fichiers : créer le fichier **.gitignore** et y lister ligne par ligne
les fichiers qu'on ne veut pas versionner dans Git, en indiquant leurs chemins complets. Par exemple :
```
motsdepasse.txt
config/application.yml
```

* mettre de côté une modification en cours sans faire de commit, pour par exemple aller dans une autre branche : `git stash`.  
Pour reprendre lors du retour dans la branche et récupérer les modifications mises de côté, faire `git stash pop`.  
Attention, **pop** cide le stash des modifications qu'on a mis dedans. Si on veut garder les modifications dans le stash,
on peut utiliser à la place `git stash apply`

* voir la liste des dépôts distants : `git remote -v`.  
Il doit au moins y avoir `origin`, qui est le nom par défaut donné par Git au serveur
à partir duquel on a cloné le projet.  
-v permet d'afficher l'URL que Git a stocké pour chaque nom court


## Les branches

Lors de l'initialisation du repo Git, le code est par défaut dans la branche principale appelée **master**

* voir la liste des branches (une étoile est placée devant la branche dans laquelle on est) : `git branch`

* créer une nouvelle branche : `git branch nouvelle-branche`

* pour se placer dans une autre branche à l'intérieur du repo : `git checkout nouvelle-branche`

* fusionner des branches, par exemple ajouter dans une brancheA les mises à jour faites dans une branche B.
On se place dans A (`git checkout brancheA`) et on fusionne : `git merge brancheB`

* résoudre les conflits : si en fusionnant Git signale qu'il y a un conflit,
il faut ouvrir le fichier en question dans l'éditeur de texte, par exemple Vim (`vim nomFichier.extension`),
et choisir quel contenu garder, sauvegarder le fichier et revenir à la console.  
Une fois le conflit résolu, il faut le dire à dire en faisant un commit sans message (`git commit`), ce qui va permettre à Git
de voir que le conflit est résolu et il va proposer un message par défaut qu'on peut personnaliser. On le sauvegarde en tapant `:x`.
Git confirme ensuite que les branches sont fusionnés.

* supprimer une branche : `git branch -d nom-branche`


