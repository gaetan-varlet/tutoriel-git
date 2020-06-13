# Pull requests / Merge requests

## Définition

- action qui consiste à demander au détenteur du dépôt original de prendre en compte les modifications que nous avons apporté sur notre fork et que l'on souhaite partager
- *Pull Request* sur GitHub est équivalent à *Merge Request* sur GitLab

## Comment contribuer sur un projet via une Merge Request

- forker le projet pour l'avoir dans notre compte GitHub
- clôner le fork dans le dépôt local sur notre machine : `git clone git@github.com:username/depot.git`
- notre fork correspond donc au dépôt distant *origin* : `git remote -v`
- ajout d'un lien avec le dépôt d'origine pour récupérer les évolutions qu'il y aura. Par convention, on l'appelle **upstream** : `git remote add upstream https://github.com/microsoft/vscode.git`
- récupérer les modifications de la branche master du dépôt original sur notre branche master : `git checkout master; git pull upstream master`
- envoyer ces modifications sur notre fork : `git push origin master`
- depuis notre branche master à jour, créer une nouvelle branche pour développer une nouvelle fonctionnalité ou corriger un bug : `git checkout -b feature-branch`
- commiter et pousser dans notre fork : `git push origin feature-branch`
- il faut que notre *feature branch* soit à jour avec le master pour ne pas avoir à gérer de conflits entre notre branche et la branche master du dépôt d'origine
    - merger *master* dans la *feature branch* : `git checkout feature-branch; git merge master`
    - rebaser master
        - faire un *rebase* pour garder l'historique de la branche propre. Les commits de la *feature branch* seront réappliqués à la suite des commits du master comme s'il n'y avait qu'une seule branche : `git checkout feature-branch; git rebase master`
        - comme l'historique des commits de la branche a été réécrit, il diffère de l'historique sur notre dépôt distant, il faut donc **force pusher** les modifications sur notre branche pour écraser l'historique des commits de la branche sur notre dépôt distant : `git push -f origin feature-branch`
- créer une pull request, en choisissant dans *base repository* la branche master du dépôt d'origine et dans *head repository* la branche de notre fork
- possibilité de continuer à faire des commits dans la branche de notre fork, ils seront automatiquement ajouté dans le pull request
- possibilité de voir tout le code modifié dans la branche, de la comparer au code de la branche cible, d'avoir une discussion dans la pull request...
- une fois la pull request acceptée, supprimer la branche de notre fork et de notre dépôt local (`git branch -D feature-branch`)