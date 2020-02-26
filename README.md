# learndocker

L'objectif est de démarrer une application Node qui tourne dans un conteneur

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
   
- Créer un registry local : https://docs.docker.com/registry/deploying/
   - docker run -d -p 5000:5000 --restart=always --name registry registry:2
   
- Télécharger une image nodejs : 
   - docker pull node sous le repertoire images (ou voir sur Docker hub) 
- Tager l'image node pullé : 
   - docker tag node localhost:5000/my-node
- Pusher l'image tagué dans le registry : 
   - docker push localhost:5000/my-node
- Supprimer le cache localement node et localhost:5000/my-node, mais celui-ci ne supprime pas l'image du registry :
   - docker image remove node
   - docker image remove localhost:5000/my-node
- teste de pull pour récupérer l'image node tagué :
   - docker pull localhost:5000/my-node
   
   
