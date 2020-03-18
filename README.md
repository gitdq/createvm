# learndocker

L'objectif est de démarrer une application après avoir installé Node et Nmp sur le host

Mise en place:
 - outils :
   * VitualBox : version 5.2.18
   * Git Bash  version 3.1.4
   * Vagrant Docker : 2.1.2 (lastest 2.2.7 à upgrader)
   To upgrade to the latest version, visit the downloads page and
   download and install the latest version of Vagrant from the URL below:

   https://www.vagrantup.com/downloads.html

   If you're curious what changed in the latest release, view the
   CHANGELOG below:

   https://github.com/hashicorp/vagrant/blob/v2.2.7/CHANGELOG.md
   
- Démarrage du Vagran-Docker : 
   - se positionner sous Vagrant-Docker
   - lancer vagrant up
   - se connecter à la machine virtuelle vagrant : vagrant ssh
   
- Start the registry automatically
   - docker run -d -p 5000:5000 --restart=always --name registry registry:2
   
- Télécharger une image nodejs : 
   - docker pull node sous le repertoire images (ou voir sur Docker hub) 
- Tager l'image node pullé : 
   - docker tag node localhost:5000/my-node
- Créer un registry local : https://docs.docker.com/registry/deploying/ : Pusher l'image tagué dans le registry : 
   - docker push localhost:5000/my-node
- Supprimer le cache localement node et localhost:5000/my-node, mais celui-ci ne supprime pas l'image du registry :
   - docker image remove node
   - docker image remove localhost:5000/my-node
- teste de pull pour récupérer l'image node tagué :
   - docker pull localhost:5000/my-node
   ou
   -  docker pull myregistry.local:5000/testing/test-image
*******************************************************************************************************************


   

*******************************************************************************************************************
Notion de base sur npm :
************************

1) Installer npm ==> sudo yum install npm (gestionnaire de package)
2) Créer un répertoire tempo1 ==> mkdir tempo1
3) créer un fichier "package.json" et inserer le contenu ci-dessous ==> vi package.json
   {
   "name": "docker_web_app",
   "version": "1.0.0",
   "description": "Node.js on Docker",
   "author": "First Last <first.last@example.com>",
   "main": "server.js",
   "scripts": {
     "start": "node server.js"
   },
   "dependencies": {
     "express": "^4.13.3"
   }
 }

4) créer un fichier "server.js" et inserer le contenu ci-dessous ==> vi server.js
   [vagrant@localhost tempo1]$ more server.js
var express = require('express');
 // var PORT = 8080;
 var PORT = process.env.PORT;  => cette ligne permet de commprendre comment passer la variable PORT en paramétre lors du lancement de 
                                   npm
 var app = express();
 app.get('/', function (req, res) {
  // res.send('Hello world\n');
 var msg = process.env.MSG ==> cette ligne permet de commprendre comment passer la variable PORT en paramétre lors du lancement de 
                               npm
   res.send(msg +'\n');
 });

 app.listen(PORT);
 console.log('Running on http://localhost:' + PORT);

5) Lancer nmp install . ==> permet de ramener les dépendances spécifiées dans le packages.json (express)
   Resultat :
└─┬ express@4.17.1
  ├─┬ accepts@1.3.7
  │ ├─┬ mime-types@2.1.26
  │ │ └── mime-db@1.43.0
  │ └── negotiator@0.6.2
  ├── array-flatten@1.1.1
  ├─┬ body-parser@1.19.0
  │ ├── bytes@3.1.0
  │ ├─┬ http-errors@1.7.2
  │ │ ├── inherits@2.0.3
  │ │ └── toidentifier@1.0.0
  │ ├─┬ iconv-lite@0.4.24
  │ │ └── safer-buffer@2.1.2
  │ └── raw-body@2.4.0
  ├── content-disposition@0.5.3
  ├── content-type@1.0.4
  ├── cookie@0.4.0
  ├── cookie-signature@1.0.6
  ├─┬ debug@2.6.9
  │ └── ms@2.0.0
  ├── depd@1.1.2
  ├── encodeurl@1.0.2
  ├── escape-html@1.0.3
  ├── etag@1.8.1
  ├─┬ finalhandler@1.1.2
  │ └── unpipe@1.0.0
  ├── fresh@0.5.2
  ├── merge-descriptors@1.0.1
  ├── methods@1.1.2
  ├─┬ on-finished@2.3.0
  │ └── ee-first@1.1.1
  ├── parseurl@1.3.3
  ├── path-to-regexp@0.1.7
  ├─┬ proxy-addr@2.0.6
  │ ├── forwarded@0.1.2
  │ └── ipaddr.js@1.9.1
  ├── qs@6.7.0
  ├── range-parser@1.2.1
  ├── safe-buffer@5.1.2
  ├─┬ send@0.17.1
  │ ├── destroy@1.0.4
  │ ├── mime@1.6.0
  │ └── ms@2.1.1
  ├── serve-static@1.14.1
  ├── setprototypeof@1.1.1
  ├── statuses@1.5.0
  ├─┬ type-is@1.6.18
  │ └── media-typer@0.3.0
  ├── utils-merge@1.0.1
  └── vary@1.1.2

6) Lance ensuite npm en passant les paramétres ==>  env PORT=8085 MSG="Hello world" npm start
7) passer en background : Ctrl+z puis bg (pour revenir en fordground : fg)
8) Pour tester, lancer un curl ==> curl -X GET http://localhost:8085

Notes : Dans package.json, on peut customiser les npm (voir exemple ci-dessou :
        {
   "name": "docker_web_app",
   "version": "1.0.0",
   "description": "Node.js on Docker",
   "author": "First Last <first.last@example.com>",
   "main": "server.js",
   "scripts": {
     "start": "node server.js",
     "coucou": "echo 'here i am'",
     "build": "npm install ."
   },
   "dependencies": {
     "express": "^4.13.3"
   }
 }

on a ajouté ==> build: npm install, ce qui fait lorsque nous lançons : npm run build . celui-ci lance npm install

*******************************************************************************************************************


   

*******************************************************************************************************************

Cours sur Docker-composer:

créer un fichier docker-compose.yml pour orchestrer vos conteneurs Docker.

Pour rappel, voici les arguments que nous avons pu voir dans ce chapitre :

image qui permet de spécifier l'image source pour le conteneur ;

build qui permet de spécifier le Dockerfile source pour créer l'image du conteneur ;

volume qui permet de spécifier les points de montage entre le système hôte et les conteneurs ;

restart qui permet de définir le comportement du conteneur en cas d'arrêt du processus ;

environment qui permet de définir les variables d’environnement ;

depends_on qui permet de dire que le conteneur dépend d'un autre conteneur ;

ports qui permet de définir les ports disponibles entre la machine host et le conteneur
             

   
