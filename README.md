# TP Git & Github

Ce TP fait suite à un cours théorique qui présente Git et Github.
Les slides sont disponible ici : https://speakerdeck.com/volpeo/introduction-a-git

## 0. Quelques pré requis 

#### Quelques commandes pratiques sur bash dont vous aurez besoin tout au long du TP, si vous ne les connaissez pas, veuillez bien y faire attention.

	pwd : affiche le chemin courant
	ls : lister les fichiers et répertoires dans le répertoire courant
	cd <repertoire>: change de répertoire pour celui demandé
	touch <nom-du-fichier> : créé le fichier (vide)
	mkdir <nom-du-repertoire> : créé le répertoire

#### Sous VIM (éditeur de texte), vous en aurez aussi besoin lors du TP, lisez bien :

Il existe 2 modes dans cet éditeur : 
- Le mode insertion, accessible en appuyant la touche i quand on est en mode commande. Ce mode permet de taper du texte.
- Le mode commande, pour y accéder, il faut appuyer sur la touche échap. On peut ensuite taper quelques commandes :

		:q pour quitter
		:w pour enregistrer
		:x pour enregistrer puis quitter

Notez qu'on peut rajouter un point d'exclamation à la suite de la commande pour forcer celle-ci (par exemple quitter un fichier qu'on aurait modifié sans l'enregistrer).

## 1. Installer et configurer git

### Installation

Rendez-vous sur la page de téléchargement de git, celle-ci contient des instructions selon votre système d'exploitation. http://git-scm.com/

Attention, si vous êtes sous windows, installez la version bash de git. Choisissez l'option "Checkout Windows-style, commit Unix-style" à l'écran suivant (toutes les options par défaut).

### Configuration

La toute première chose à faire est de configurer l'auteur, donc vous, votre nom et votre adresse email.
Dans votre terminal (ou git bash), tapez (ces commandes ne répondent rien) :

	git config --global user.name "Prénom Nom"

	git config --global user.email "monemail@fournisseur.com"

	git config --global color.ui true

Cela va ajouter des lignes à votre fichier de configuration .gitconfig, la dernière ligne permet de mettre un peu de couleur dans la réponse des différentes commande de git, ce n'est pas du luxe.
On peut vérifier que tout c'est bien passé en tapant :

	cat ~/.gitconfig

## 2. Initiatlisation du dépôt

Nous sommes prêt à travailler, il est temps d'initialiser un nouveau dépôt et de travailler sur nos fichier. Nous allons ici créer un clone de flappy bird en javascript.

Créez un nouveau répertoire (nomez le "flappy_clone" si vous n'avez pas d'inspiration), **placez vous dedans avec le terminal**, puis tapez :

	git init

Votre dépôt est initialisé mais bien vide, nous allons ajouter un peu de contenu.
Listez les fichiers du répertoire courant, si vous voyez une longue liste, c'est que vous vous êtes trompé quelque part et que vous allez partager tous vos fichiers personels sur internet.

## 3. Premiers commits

Commencez par créer un fichier nommé index.html, puis tapez :
	
	git status

Vous aurez un aperçu de la situation, git nous dit que nous avons un fichier non suivi. Nous allons alors l'ajouter avec :

	git add index.html

Un nouveau status vous montrera que le fichier est désormais validé (et donc suivi).

Nous allons maintenant faire un instantané ou "commit" de ces modifications avec :

	git commit -m "Init + added empty index.html file"

Préférez rédiger vos messages de commit en anglais.

Nous allons maintenant insérer du contenu dans ce fichier qui est bien vide. Collez le code suivant dans le fichier index.html avec votre éditeur de code préféré (utilisez notepad++ ou sublime text si vous n'en avez aucun installé sur votre machine, pas de textedit !) :

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <title>Flappy Clone</title>
    </head>
    <body>
      <p>Appuyer sur la barre espace pour sauter</p>
      <div id="game"></div>
    </body>
    </html>
   
Vous pouvez constater qu'en ouvrant le fichier index.html avec un navigateur (clic droit, ouvrir avec...), que le contenu s'affiche bien à l'écran, malheureusement ce n'est pas encore très amusant.

On peut faire un status pour voir ce qu'il se passe. On s'apperçoit que git a compris notre modification.
On peut maintenant essayer de taper :
	
	git diff

On voit alors les modifications apportées, ici uniquement des lignes ajoutées.

Ajoutons nos modifications à notre dépôt avec un nouveau commit, essayez d'être précis dans votre message. Et n'oubliez pas que le fichier index.html est déjà suivi, donc inutile de faire un git add si vous passez l'option -a lors du commit : git commit -am "mon message".

Il manque deux choses à notre jeu pour qu'il fonctionne : un fichier de librairie et un fichier qui contient notre jeu.

Nous allons d'abord ajouter la librairie, pour ce faire, coller la ligne suivante juste après la ligne qui contient la balise \<title\> :

	<script src="http://cdnjs.cloudflare.com/ajax/libs/phaser/2.1.1/phaser.min.js"></script>

Après un status, voir un diff, faites un commit, toujours en s'appliquant sur le message. Il faut qu'on comprenne quelle a été la modification apportée.

Nous allons maintenant ajouter le code javascript qui contient notre jeu.

Insérez la balise suivante après la balise script que nous venons d'insérer :

	<script src="game.js"></script>

Créez aussi un fichier à côté de index.html qui s'appellera game.js

Tapez ensuite :
	
	git commit -am "Added game file"

En faisant un git status, vous vous appercevrez que nous avons fait une bourde, nous avons commité uniquement le fichier index.html car il était suivi par git, mais pas game.js, car lui n'était pas suivi.
Nous avons deux choix, soit faire un nouveau commit qui ajoute le fichier game.js, ou utiliser l'option --amend qui permet d'ajouter un fichier suivi au commit précédent. Pour la deuxième solution, ajouter le fichier game.js à l'index, puis tapez :

	git commit --amend

Git lancera votre éditeur de texte pour que vous puissiez corriger le message de commit si nécessaire.

Nous allons maintenant passer au choses sérieuses, rendez-vous à l'adresse suivante  https://gist.github.com/volpeo/479af6400b6d8207affc

Copiez le code et collez-le dans game.js.
Testez le fichier index.html dans un navigateur.

Notre jeu est fonctionnel, commitez les modifications.

Nous pouvons observer l'historique des commits avec :
	
	git log

## 4. Branches

Nous décidons maintenant d'ajouter une fonctionnalité à notre jeu. Le saut du carré jaune représentant l'oiseau n'est pas très réaliste, nous allons ajouter quelques lignes qui vont améliorer ça.
Cependant, nous ne voulons pas créer cette fonctionnalité sur la branche master, pour ne pas la polluer, mais plutôt sur une nouvelle branche.

Créez une nouvelle branche nomée jump en tapant :

	git branch jump

La nouvelle branche est créée, on peut le vérifier en tapant :

	git branch

La branche courrante est celle qui est précédée d'une étoile. Pour changer de branche, tapez :

	git checkout jump

Nous sommes sur la branche consacrée à notre nouvelle fonctionnalité, nous pouvons effectuer nos modifications tranquillement.

Dans game.js, cherchez le commentaire "Rotate the bird" et insérez les lignes suivante juste en dessous :

		if (this.bird.angle < 20)
            		this.bird.angle += 1; 


Cherchez ensuite le commentaire "Jump animation" quelques lignes plus bas, et copiez juste en dessous :

	game.add.tween(this.bird).to({angle: -20}, 100).start();

Testez dans votre navigateur la nouvelle animation pour le saut.

Une fois que vous êtes satisfait de votre code et que vous êtes sûr de n'avoir rien cassé, commitez les changements.

Nous nous retrouvons ici avec une branche qui contient une fonctionnalité, et la branche principale (master) qui n'en profite pas. Maintenant que nous sommes sûr que la fonctionnalité marche, nous pouvons la rappatrier sur la branche principale (master).

Avant de changer de branche, faite un git log pour observer votre historique, constatez que votre dernier commit conscerne l'animation du saut.

Changez de branche pour la branche principale, et inspectez à nouveau votre historique, ici le commit de l'animation du saut n'est pas présent.

Fusionnez alors la branche jump vers la branche master en tapant :

	git merge jump

Un nouveau git log nous montrera le dernier commit qu'il nous manquait, maintenant présent sur la branche master.


## 5. Branche et fusion avec conflit

Nous décidons d'essayer le jeu avec une nouvelle couleur de fond.
Créez une nouvelle branche nommée "background", positionnez vous dessus et éditez le fichier game.js.

Cherchez dans les premières ligne, juste après le début de la fonction preload, la ligne contenant "game.stage.backgroundColor", et modifier la valeur de "#71c5cf" en "#FF6A5E"

Observez les changements dans le navigateur (le fond doit être rouge) et commitez vos changements.

Repassez sur la branche master, observez que le fond est redevenu bleu.

Le but de cette partie est de créer un conflit, ce que nous allons faire n'est donc pas très logique.

Sur la branche master, éditez le code couleur en "#D0FFC2", vérifiez dans votre navigateur et commitez les changements.

Fusionnez la branche background avec le master. Git devrait vous signaler un conflit, un git status nous le confirme.

Éditez game.js, observez les lignes que git a mis en place pour vous signaler la position du conflit. Supprimez les une fois que vous avez fait un choix entre les deux propositions.
Il faut supprimer les lignes entières contenant "<<<<<<<<<<<<< HEAD" le séparateur des deux versions "=================", la ligne ">>>>>>>>>>>>>> background", ainsi que l'une des deux lignes en conflit dans notre cas (car il se peut que dans le conflit, on garde les lignes des 2 commits).

Commitez les changements en signalant le merge dans le message du commit.

## 6. Remote et Github

Nous allons maintenant travailler avec un dépôt distant, histoire de montrer au monde entier notre code, nous choisissons de le mettre sur GitHub.

Créez un compte et créer un nouveau dépôt nommé "flappy_clone" (créez ce dépôt sans Readme !!). Github va vous donner la marche à suivre pour configurer ce nouveau dépôt distant en local. Dans votre terminal, toujours dans votre dépôt local, tapez :

	git remote add origin <adresse du dépot en https>
	
	git push -u origin master

Votre projet se trouve maintenant sur github, vous pouvez rafraîchir la page pour observer les résultats.

## 7. Fork et pull request

Maintenant que votre projet est en ligne, on va contribuer à un projet. "Forkez" le projet tp_git, il devrait ensuite se trouver dans votre compte GitHub. Récupérez le grâce à la commande git clone (l'adresse pour récupérer le dépôt se trouve sur la droite, choisissez le mode https). N'oubliez pas de vous placer dans un autre répertoire que flappy_clone pour cloner le dépôt. La commande git clone créé elle même un répertoire et y place le code à l'intérieur.

Une fois le dépôt téléchargé sur votre machine, vous avez accès à l'historique (et donc vous verrez mes différents commits qui suivent l'évolution de ce document, mes fautes de frappes corrigées, etc.).

Éditez le README.md, et ajouter y votre nom et prénom ainsi que l'url de votre compte GitHub en suivant le modèle établi (Si vous avez trouvé des fautes d'orthographe dans ce tutoriel, n'hésitez pas à les corriger).

Commitez le tout et envoyer le code vers le dépôt distant.

Observez vos changements sur la page du dépôt sur GitHub, ces changements sont uniquement dans votre dépôt et pas dans mon dépôt qui est pourtant l'original.

Le but ici est de proposer vos modifications à mon projet original, pour se faire, sur la page du projet tp_git, cliquez sur le bouton de "pull request", vous ne pouvez pas le manquer il est à gauche et vert.

![Bouton Pull Request](https://dl.dropboxusercontent.com/u/586198/pull-request.png)

Remplissez le message correctement, il faut qu'on comprenne ce que vous proposez comme modification.

Une fois réalisée, vous pouvez observer sur le projet original toutes les proposition de pull request, et leur état.


## 8. Élèves ayant validé ce cours d'initiation

Sylvain Peigney - https://github.com/volpeo
Mussard Lo�c - https://github.com/Archloy
