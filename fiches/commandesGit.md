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
- créer un nouveau dépôt, pour commencer un nouveau projet : `git init` en se positionnant dans le répertoire du projet. Cela crée un dossier caché *.git* à la racine du projet qui stocke l'historique des fichiers et la configuration
- cloner un dépôt existant, c'est-à-dire récupérer l'historique des changements d'un projet pour travailler dessus : `git clone <url>`. `git clone <url> nomRepertoire` permet de cloner le dépôt dans un répertoire nommé différemment

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

- `git log` : afficher la liste de tous les commits réalisés en ordre chronologique inversé avec l'identifiant, l'auteur, la date et le message du commit
  -  appuyer sur *q* pour quitter
  - `-p` affiche le détail de chaque ligne ajoutée ou retirée
  - `--stat` pour un résumé plus court des commits en indiquant la liste des fichiers modifiés ainsi que le nombre de lignes ajoutées ou retirées dans les fichiers
  - `--pretty` permet de modifier la mise en forme du journal des commits
    - `git log --pretty=oneline` affiche un commit par ligne
    - l'option format permet de décrire précisément le format de sortie, par exemple `git log --pretty=format:"%h - %an, %ar : %s"`
  - `--graph` ajoute un graphique pour décrire l'historique des branches et fusions
  - limiter la longueur de l'historique
    - `-<n>` affiche les *n* derniers commits, par exemple `git log -2` affiche les 2 derniers commits
    - `--since` ou `--after` (affiche les commits réalisés après la date spécifiée) et `--until` ou `--before` (affiche les commits réalisés avant la date spécifiée) sont des options de limitation dans le temps
      - fonctionne avec de nombreux formats : *2.weeks*, *2008-01-05*, *2 years 1 day 3 minutes ago*
      - exemple : `git log --since=2.weeks`
    - `--author` permet de filtrer sur un auteur spécifique
    - `--committer` permet de filtrer sur un validateur spécifique
    - `--grep` permet de filtrer les commits dont le message contient la chaîne de caractères spécifiée
    - `-S` permet de filtrer les commits dont les modifications ajoutent ou retirent le texte comportant la chaîne spécifiée, `git log -S"ma chaîne"`

- annuler des actions :
  - modifier le dernier commit : `git commit --amend`
    - `git commit --amend -m "nouveau message"` modifie le message du dernier commit si aucune modification n'a été réalisée depuis la dernière validation
    - ajouter un fichier que l'on a oublié d'indexer dans le dernier commit sans créer un nouveau commit : `git commit -m 'validation initiale'`, `git add fichier_oublie`, `git commit --amend`
  - désindexer un fichier déjà indexé : `git reset HEAD fichier`
  - réinitialiser un fichier que l'on a modifié et le ramener à son état du dernier instantané `git checkout -- fichier` ou `git checkout fichier`. Toutes les modifications non validées seront alors perdues

- travailler avec les dépôts distants
  - afficher les dépôts distants : `git remote -v`, l'option *verbose* permet d'afficher l'URL des dépôts distants en plus de leur nom court. *origin* est le nom par défaut du dépôt distant
  - ajouter un dépôt distant : `git remote add [nomcourt] [url]`, par exemple `git remote add origin https://...`
  - retirer un dépôt distant : `git remote rm [nomcourt]`
  - renommer un dépôt distant : `git remote rename ancienNomCourt nouveauNomCourt`
  - obtenir les données d'un dépôt distant : `git fetch [remote-name]` récupère sur le dépôt distant les données du projet qu'on n'a pas encore récupéré sous une branche spécifique, sans fusionner avec notre copie de travail
  - obtenir et fusionner automatiquement les données d'un dépôt distant : `git pull`
  - pousser son travail sur un dépôt distant : `git push`, ou de manière plus complète `git push [nom-distant] [nom-de-branche]`, par exemple `git push origin master`. Il faut d'abord être à jour avec le dépôt distant en ayant tirer et fusionner les modifications du dépôt distant avec notre dépôt local
  - inspecter un dépôt distant : `git show [nom-distant]` permet d'avoir plus d'informations sur un dépôt distant particulier, notamment la liste des branches suivies

- les étiquettes, ou *tags* en anglais
  - lister les étiquettes dans l'ordre alphabétique : `git tag`
    - filtrer les étiquettes, par exemple celles commençant par *v1.8* : `git tag -l 'v1.8*'`
    - voir à quoi correspond une étiquette : `git show nomTag`
  - créer des étiquettes
    - étiquettes annotées, qui ont une somme de contrôle, le nom et l'email du créateur, la date, le message d'étiquetage
    - étiquettes légères, qui stocke juste la somme de contrôle d'un commit
      - `git tag nomTag`  ajoute un tag sur le dernier commit
      - `git tag nomTag idCommit` ajoute un tag sur un commit plus ancien, par exemple `git tag v1.3 2f7c8b3428aca535fdc970feeb4c09efa33d809e`
  - supprimer une étiquette
    - `git tag -d nomTag` permet de supprimer une étiquette locale
  - partager les étiquettes
    - un tag n'est pas envoyé lors d'un push, il faut le préciser : `git push origin [nom-du-tag]`, ou avec l'option --tags : `git push --tags`

- créer des alias
  - ce sont des raccourcis pour gagner du temps sur des commandes fréquentes
  - lister les alias existant : `git config --get-regexp alias` ou `git config --list | grep alias`
  - définir un alias : `git config --global alias.st status`. On peut donc écrire `git st` à la place de `git status`
  - quelques alias intéressants :
    - `git config --global alias.unstage 'reset HEAD --'`, permet de désindexer un fichier avec la commande `git unstage fichier`
    - `git config --global alias.hist 'log --pretty=format:"%h - %an, %ar : %s"'` permet de voir proprement la liste des derniers commits avec la commande `git hist`

## Les branches avec Git

- lors de l'initialisation du repo Git, le code est par défaut dans la branche principale appelée **master**
- `HEAD` est un pointeur spécial, qui permet de savoir sur quelle branche on se trouve
- `git branch` affiche la liste des branches (une étoile est placée devant la branche sur laquelle le pointeur HEAD se situe)
  - `git branch -v` permet de visualiser le dernier commit de chaque branche
  - `--merged` et `--no-merged` permettent de filtrer les branches selon qu'elles ont été fusionnées avec la branche courante. Exemple `git branch --merged`
- `git branch [nom-branche]` crée une nouvelle branche
  - cela crée un nouveau pointeur vers le commit courant
  - cela ne fait pas basculer la copie de travail vers cette branche
  - une branche créée est locale, il est ensuite possible de la publier
- `git branch -d [nom-branche]` supprime une branche locale
  - si une branche n'a pas encore été fusionné, on ne peut pas la supprimer avec `-d`. Il faut forcer la suppresison avec `-D`

- `git checkout [nom-branche]` permet de changer de branche
  - avoir sa copie de travail propre au moment de changer de branche
  - cela déplace *HEAD* sur la branche sur laquelle on va
  - `git checkout -b [nom-branche]` crée une nouvelle branche et y bascule immédiatement

- fusionner des branches, par exemple ajouter dans une brancheA les mises à jour faites dans le master. On se place dans A (`git checkout brancheA`) et on fusionne : `git merge master -m "merge du master dans la brancheA"`
  - avance rapide : lorsque l’on cherche à fusionner un commit qui peut être atteint en parcourant l’historique depuis le commit d’origine, Git se contente d’avancer le pointeur car il n’y a pas de travaux divergents à fusionner — ceci s’appelle un fast-forward (avance rapide)
  - commit de fusion : Git crée un nouvel instantané qui résulte de la fusion à trois sources et crée automatiquement un nouveau commit qui pointe dessus. On appelle ceci un commit de fusion (merge commit) qui est spécial en cela qu’il a plus d’un parent

- résoudre les conflits : si en fusionnant Git signale qu'il y a un conflit, il faut ouvrir le fichier en question et choisir quel contenu garder, sauvegarder le fichier et revenir à la console
  - Git n’a pas automatiquement créé le commit de fusion. Il a arrêté le processus le temps que vous résolviez le conflit. Si vous voulez vérifier, à tout moment après l’apparition du conflit, quels fichiers n’ont pas été fusionnés, vous pouvez lancer la commande `git status` : tout ce qui comporte des conflits et n’a pas été résolu est listé comme unmerged
  - Git ajoute des marques de résolution de conflit standards dans les fichiers qui comportent des conflits (<<<<<<<, ======= et >>>>>>>), pour qu'on puisse les ouvrir et résoudre les conflits manuellement. Après avoir résolu chacune de ces sections dans chaque fichier comportant un conflit, lancez `git add` sur chaque fichier pour le marquer comme résolu. Placer le fichier dans l’index marque le conflit comme résolu pour Git.
  - Une fois les conflits résolu, il faut le dire à dire en faisant un commit, éventuellement sans message car Git propose un message par défaut qu'on peut personnaliser. Git confirme ensuite que les branches sont fusionnés.

- les branches distantes
  - `git ls-remote [remote]` et `git remote show [remote]` permet d'obtenir les références des éléments (branches, tags...) du dépôt distant [remote]
  - les branches de suivi
    - l’extraction d’une branche locale à partir d’une branche distante crée automatiquement ce qu’on appelle une **branche de suivi** (**tracking branch**) et la branche qu’elle suit est appelée **branche amont** (**upstream branch**). Les branches de suivi sont des branches locales qui sont en relation directe avec une branche distante. `git push` et `git pull` fonctionnent directement sans autre configuration
    - création d'une branche locale de suivi : `git checkout -b [branche] [remote]/[branche]` ou encore `git checkout --track [remote]/[branche]`. La branche locale poussera et tirera automatiquement vers [remote]/[branche]
    - associer la branche locale sur laquelle on est à une branche distante existante `git branch -u [remote]/[branche]` pu `git branch --set-upstream-to=origin/master`
    - voir les branches de suivi configuées : `git branch -vv`
      - quelques infos notamment sur le nombre de commits d'avance et de retard
      - basé sur l'état de la branche la dernière fois, qu'elle a était synchronisée. Pour mettre à jours toutes les branches distantes depuis les serveurs, faire `git fetch --all`
  - `git push --set-upstream origin nouvelle-branche` ou `git push -u origin nouvelle-branche` publie la nouvelle branche sur le dépôt distant origin. L'option *--set-upstream* permet de dire à Git de se souvenir qu'un « git push » de notre branche « nouvelle-branche » envoie les changements à la branche « nouvelle-branche » du dépôt distant. Le prochain push pourra donc se faire simplement avec un `git push`
  - `git push origin -d nom-branche` supprime une branche sur le dépôt distant

- le rebasage
  - la fusion (`git merge`) crée un nouvel instantané entre les deux derniers instantanés de chaque branche et l'ancêtre commun le plus récent
    - on a donc dans l'historique des commits de notre branche principale tous les commits de la branche fusionnée + le nouveau commit de fusion
  - le rebasage (`git rebase`) prend toutes les modifications qui ont été validées sur une branche et les rejoue sur une autre
    - l'idée est de travailler dans une branche puis de rebaser notre travail sur la branche *master* quand on est prêt à soumettre nos patchs au projet principal, comme ça il y a juste un avance-rapide à faire sur la branche *master* pour intégrer nos modifications faites dans la branche de travail
    - on rejoue sur la branche *experience* toutes les modifications qui ont été validées sur la branche *master* : `git checkout experience` puis `git rebase master`
    - on peut retourner sur la branche *master* et faire une **fusion en avance-rapide** : `git checkout master` puis `git merge experience`
    - on a donc dans l'historique des commits de notre branche principale tous les commits de la branche fusionnée mais pas de commit de fusion
    - il n'y a pas de différence entre les résultats des deux types d’intégration, mais rebaser rend l’historique plus clair
  - les dangers du rebasage
    - ne jamais rebaser des commits qui ont déjà été poussés sur un dépôt public, car rebaser modifie l'historique des commits (on abandonne les commits existants et on en crée de nouveaux qui sont similaires mais différents) et si d'autres personnes se sont basés sur ces commits pour travailler, ça va compliquer leur tâche car ils devront fusionner leur travail avec ce qu'on a rebasé, ce qui peut être compliquer quand on voudra tirer leur travail dans notre dépôt
    - si on a rebasé une branche qui était déjà poussé sur un dépôt distant, on ne peut plus faire de `git push` car l'historique des commits a été modifié et Git refuse donc le push. On peut faire `git push --force`.
    - Si on a modifié l'historique des commits, pour éviter les problèmes sur les postes des autres développeurs, il vaut mieux supprimer la branche locale en faisant `git branch -d [branche]`, puis `git pull` pour récupérer les modifications du dépôt distant et enfin un `git checkout [branche]` pour retourner sur la nouvelle version de la branche avec l'historique révisé


## Git sur le serveur

- les protocoles
  - **local** : le dépôt distant est un autre répertoire dans le système de fichiers. Système simple utilisant les permissions du système de fichiers, mais peut être difficle de rendre disponible le partage réseau depuis de nombreux endroits plutôt que de gérer un accès réseau
  - **HTTP** : peut être utilisé de manière anonyme, ou avec authentification (nom d’utilisateur et mot de passe) et chiffrement pour pousser, le tout avec une unique URL. Système très simple, quie l'on peut coupler avec un système de mise en cache d'informations d'authentification pour ne pas s'authentifier à chaque *push*
  - **SSH** (*Secure Shell*) : protocole authentifié et sécurisé qui a l'inconvéniant de ne pas proposer un accès anonyme au dépôt, même pour un accès en lecture seule
  - **Git** : protocole géré par un daemon (processus en arrière-plan) livré avec Git. Similaire au protocole SSH, mais non sécurisé. Protocole avec la vitesse de transfert la plus rapide.

- les dépôts nus
  - un dépôt distant est souvent un dépôt nu (*bare repository*), c'est-à-dire qui n'a pas de copie de travail
  - par convention, les répertoires de dépôt nu finissent en *.git*
  - pour installer un serveur Git, il faut créer un dépôt nu en clonant un dépôt existant avec la commande suivante `git clone --bare <url>`, le placer sur un serveur et régler les protocoles

- les clés SSH
  - stockés par défaut dans *~/.ssh* sous le nom *id_dsa* ou *id_rsa* avec *.pub* pour la clé publique, l'autre étant la clé privée
  - `ssh-keygen` permet de générer une paire de clé. Il demande deux fois d’entrer un mot de passe qui peut être laissé vide si on ne souhaite pas devoir le taper quand on utilise la clé
  - il faut ensuite envoyer la clé publique au serveur Git

- GitWeb : interface web de visualisation simpliste

- GitLab
  - serveur Git plud moderne et complet
  - application web reposant sur une base de données, installation plus lourde que d'autres serveurs Git
  - les utilisateurs correspondent à des personnes. Les groupes sont des assemblages de projets, avec un espace de nom de projet comme pour les utilisateurs
  - un projet correspond à un dépôt Git unique

- site en hébergement externe : simple et rapide à créer, mais les données sont hébergées sur des serveurs tiers. Exemple : GitHub

## Git distribué

- développements distribués
  - *gestion centralisée* : dépôt central qui accepte le code et tout le monde doit synchroniser son travail avec (classique avec SVN)
  - *mode du gestionnaire d'intégration* : cloner le dépôt distant du projet (créer un fork), faire des modifications et les pousser sur son propre dépôt distant et ouvrir une requête de tirage (pull requests) depuis notre fork vers le dépôt principal. Le mainteneur du dépôt principal fusionne alors les modifications, ce qui lui permet de garder le contrôle total de ce qui entre dans le dépôt
  - *mode dictateur et ses lieutenants* : variante utilisé sur les projets immenses comme le noyau Linux. Des gestionnaires d'intégration, appelés *lieutenants*, gèrent une partie du projet et ces lieutenants n'ont qu'un seul gestionnaire d'intégration, *le dictateur bienveillant*. Le dictateur fusionne les branches master de ses lieutenants dans sa propre branche master et la pousse sur le dépôt de référence pour que les développeurs se rebasent dessus

- bonnes pratiques pour créer des commits
  - ne pas soumettre des patchs comportants des erreurs d'espace (espaces inutiles en fin de ligne ou entralecement d'espaces et de tabulations). `git diff --check` permet de vérifier cela avant chaque validation
  - faire de chaque validation une modification atomique, en utilisant la zone d'index pour découper le travail en au moins une validation par problème. Cela rend plus simple la vérification par les autres développeurs de notre travail, et l'éventuelle retrait ou inversion ultérieure des modifications. A noter que `git add --patch` permet d'indexer partiellement des fichiers
  - écrire des messages de validation de qualité, qui décrivent concisément la modification sur une ligne. En plus, on peut ajouter une ligne vide, suivie d'une explication plus détaillée, qui inclue la motivation de la modification en contrastant le nouveau comportement par rapport à l'ancien. Utiliser des verbes substantivés par exemple *Ajout de tests pour ...* au lieu de *j'ai ajouté des tests pour ...*

- contribution à un projet
  - gestion d'un projet par une petite équipe : on peut avoir une gestion centralisée. Il faut d'abord fusionner les modifications du dépôt distant dans notre dépôt local avant de pouvoir pousser nos modifications, alors qu'avec Subversion la fusion se fait automatiquement quand les fichiers modifiés ne sont pas les mêmes
  - gestion d'un projet avec une équipe importante : il faut plutôt utiliser le mode du gestionnaire d'intégration. Des petits groupes collaborent sur des fonctionnalités en travaillant dans des branches et des intégrateurs mettent à jour la branche master
  - contribution à un projet public : on ne peut pas mettre à jour des branches du projet directement, il faut créer un fork du projet, créer une branche pour faire ses modifications et éventuellement rebaser sa branche sur la branche master du projet principale si celle-ci a avancé. Il faut ensuite notifier le maintenant via une requête de tirage généré via le site web, ou via la commande `git request-pull`

- maintenance d'un projet : il s'agit d'intégrer des contributions
  - essayer la nouveauté dans une branche créé à partir de master (nommer la branche de manière explicite), ce qui permet de le laisser de côté s'il ne fonctionne pas
  - application d'un patch reçu par courriel
    - avec `git apply` si le patch a été généré avec `git diff`
    - avec `git am` si le patch a été généré avec `format-patch`, qui a l'avantage de contenir le message de validation et l'auteur
  - patch dans le dépôt public du contributeur
    - ajouter le dépôt public du contributeur en tant que dépôt distant et tirer localement la branche pour réaliser la fusion
    - `git diff master...contrib` montre les modifications de la branche thématique a introduite depuis son ancêtre commun avec master
    - fusionner la branche thématique dans la branche master, ou utiliser le rebasage pour avoir un historique linéaire
    - on peut aussi introduire des modifications par *picorage* (cherry-pick), c'est-à-dire un rebasage appliqué à un commit unique, si un seul des commits de la branche nous intéresse. Pour cela, lancer la commande `git cherry-pick <SHA-1>` et un nouveau commit sera créé avec la même modification que celle du commit sélectionné


## GitHub

- plus grand hébergeur de dépôts Git
- beaucoup de dépôts Git publics pour les projets open-source




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

* `git checkout` a un double usage  
  * `git checkout SHADuCommit` : se positionne sur un commit donnée ou dans une branche donnée.  
  revenir à la branche principale (commit le plus récent) : `git checkout master`
  * `git checkout nomFichier` : le fichier redeviendra comme il était lors du dernier commit

* `git revert SHADuCommit` : "annule un commit" en créant un nouveau commit qui fait l'inverse du précédent

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
