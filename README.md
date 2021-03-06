# Docker_Kubernetes 


Docker est:

* Flexible : meme les applications les plus complexes peuvent etre conteneurisees.
* Loger : les conteneurs exploitent et partagent le noyau hete, ce qui les rend beaucoup plus efficaces en termes de ressources systeme que les machines virtuelles.
* Portable : vous pouvez creer localement, deployer sur le cloud et executer n'importe ou.
* Couplage faible : les conteneurs sont hautement autonomes et encapsules, vous permettant de remplacer ou de mettre a niveau l'un sans perturber les autres.
* evolutif : vous pouvez augmenter et distribuer automatiquement des repliques de conteneurs dans un centre de donnees.
* Securise : les conteneurs appliquent des contraintes agressives et des isolements aux processus sans aucune configuration requise de la part de l'utilisateur.

Kubernetes est:

https://www.redhat.com/fr/topics/containers/what-is-kubernetes#:~:text=Le%20principal%20avantage%20de%20la,de%20machines%20physiques%20ou%20virtuelles.


<!-- TOC -->

- [Docker_Kubernetes](#docker_kubernetes)
    - [Installation Docker desktop pour windows home](#installation-docker-desktop-pour-windows-home)
        - [Installation des packages chocolateys](#installation-des-packages-chocolateys)
    - [Creation d'une machine virtuel](#creation-dune-machine-virtuel)
        - [Configuration ports de la VM](#configuration-ports-de-la-vm)
        - [Suppression erreurs parametres graphique de la VM](#suppression-erreurs-parametres-graphique-de-la-vm)
        - [Configuration des volumes](#configuration-des-volumes)
        - [Demarrer votre machine virtuelle](#demarrer-votre-machine-virtuelle)
        - [Configuration variables d'environnement docker](#configuration-variables-denvironnement-docker)
    - [Configuration des outils docker](#configuration-des-outils-docker)
        - [installation chocolatey](#installation-chocolatey)
    - [Tester votre docker](#tester-votre-docker)
    - [Docker play](#docker-play)
        - [Creation de l'image de conteneur de l'application](#creation-de-limage-de-conteneur-de-lapplication)
    - [Installation docker sous linux ubuntu](#installation-docker-sous-linux-ubuntu)
    - [Command line Docker](#command-line-docker)
        - [docker system - Commande line](#docker-system---commande-line)
        - [docker images - Commande line](#docker-images---commande-line)
        - [docker-machine - Commande line](#docker-machine---commande-line)
    - [Ressources](#ressources)

<!-- /TOC -->



## Installation Docker desktop pour windows home

**Attention** cette installation n'est pas standar.

La configuration requise ci-dessous permet de travailler avec la version desktop de docker

System Requirements
Windows 10 64-bit: Pro, Enterprise, or Education (Build 15063 or later).
Hyper-V and Containers Windows features must be enabled.
The following hardware prerequisites are required to successfully run Client Hyper-V on Windows 10:

64 bit processor with Second Level Address Translation (SLAT)
4GB system RAM
BIOS-level hardware virtualization support must be enabled in the BIOS settings. For more information, see Virtualization.

**Attention** la version desktop n'est pas fonctionnelle sous windows home, famille et genere l'erreur suivante car les services Windows de virtualisation ne sont pas disponible sous windows home (famille) :

![error](Document/error_VT-X.bmp)


https://docs.microsoft.com/fr-fr/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v

### Installation des packages chocolateys

	C:\ choco install virtualbox
	
Oracle VM est le serveur de virtualisation. Oracle VM Server pour machines x86 inclut l'hyperviseur libre et open-source Xen, supporte Microsoft Windows, differentes distributions Linux2 (Ubuntu, Debian, Fedora, openSUSE) et intogre une console de gestion Web. Oracle VM met en ouvre la pile d'applications Oracle Applications testees et certifiees pour un environnement de virtualisation en entreprise.
	
	
	C:\ choco install docker-machine

Docker est un logiciel libre permettant de lancer des applications dans des conteneurs logiciels.

## Creation d'une machine virtuel

https://docs.docker.com/machine/reference/create/

Creation d'une machine virtuelle virtualbox (--driver virtualbox) portant le nom de vmDocker00

	$ docker-machine create --driver virtualbox vmDocker00
	
![installation machine docker](Document/install.bmp)


### Configuration ports de la VM

Dans Oracle VM, selectionner votre machine (vmDocker00) > Settings>Network>Adapter1>Port Forwarding et y renseigner les ports de votre VM qui seront exposes lorts de l'execution de votre dock:


![vm_ports](Document/vm_ports.bmp)

### Suppression erreurs parametres graphique de la VM

![vm_ports](Document/err_gui_vm.bmp)

![vm_ports](Document/solve_00.bmp)

### Configuration des volumes

Dans Oracle VM, selectionner votre machine (vmDocker00) > Settings>Shared Folders et y ajouter les volumes a monter en virtualisation et cocher Auto-mount et permanent:

![vm_volume](Document/config_volu.bmp)

### Demarrer votre machine virtuelle

	$ docker-machine start vmDocker00

### Configuration variables d'environnement docker

	docker-machine env vmDocker00
	eval $(docker-machine env vmDocker00 --shell linux)
	
![dock_machine_run](Document/dock_machine_run.bmp)

Vous devrez definir les variables d'environnement a chaque demarrage d'un nouveau terminal Git Bash. Si vous souhaitez eviter cela, vous pouvez copier la sortie eval et l'enregistrer dans votre fichier .bashrc. cela devrait ressembler a quelque chose comme ca:

	export DOCKER_TLS_VERIFY="1"
	export DOCKER_HOST="tcp://192.168.99.103:2376"
	export DOCKER_CERT_PATH="C:\Users\admin\.docker\machine\machines\vmDocker00"
	export DOCKER_MACHINE_NAME="vmDocker00"
	export COMPOSE_CONVERT_WINDOWS_PATHS="true"

**IMPORTANT**: pour DOCKER_CERT_PATH, vous devrez modifier le chemin d'accas au fichier Linux au format de chemin d'acces Windows. Notez egalement qu'il est possible que l'adresse IP attribuee soit differente de celle que vous avez enregistree a chaque demarrage de la machine virtuelle par defaut.


## Configuration des outils docker

### installation chocolatey

	C:\ choco install docker-cli
	C:\ choco install docker-compose

## Tester votre docker

	$ docker info
	$ docker run hello-world

## Docker play

Commencer par monter votre docker-machine avant de jouer. Sans omettre de configurer vos variable d'environnement

	docker-machine env vmDocker00
	eval $(docker-machine env vmDocker00 --shell linux)

https://www.docker.com/play-with-docker

Cliquer sur new instance
Puis dans gitbash tapper:

	docker run -d -p 80:80 docker/getting-started
	
- -d - executer le conteneur en mode detache (en arriere-plan)
- -p 80:80 - mapper le port 80 de l'hote au port 80 dans le conteneur 
- docker/getting-started - l'image a utiliser

Equivalent :

	docker run -dp 80:80 docker/getting-started

- -dp equivalent : -d et -p

Puis clique sur open port 80 

Dans la dossier de votre projet.

### Creation de l'image de conteneur de l'application
Creez un fichier nomme Dockerfile avec le contenu suivant.


	FROM node:12-alpine
	WORKDIR /app
	COPY . .
	RUN yarn install --production
	CMD ["node", "/app/src/index.js"]

Creez l'image conteneur a l'aide de la docker buildcommande.

	docker build -t getting-started .


## Installation docker sous linux ubuntu

Suivre:

https://www.tecmint.com/install-docker-and-run-docker-containers-in-ubuntu/

Pour check le status docker

```cmd
sudo systemctl status docker 
```

Pour demarrer/ arreter docker

```cmd
sudo systemctl start docker 
sudo systemctl stop docker 
```


## Command line Docker
### docker system - Commande line

Nettoie toutes les ressources - images, conteneurs, volumes et reseaux - qui sont en suspens (non associees a un conteneur) 

	docker system prune
	
Supprimer en plus tous les conteneurs arretes et toutes les images non utilisees

	docker system prune -a
	

### docker images - Commande line

Lister les images

	docker images -a
	
### docker-machine - Commande line

Lister les machine

	docker-machine ls

Stopper une machine

	docker-machine stop <Nom de la machine>
	
Demarrer une machine

	docker-machine start <Nom de la machine>

Supprimer une machine

	docker-machine rm <Nom de la machine>


## Ressources

https://www.tecmint.com/install-docker-and-run-docker-containers-in-ubuntu/

https://www.sitepoint.com/docker-windows-10-home/

https://www.docker.com/play-with-docker

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes-fr


Utiliser une VM est une bonne idée si tu est sous Windows, mais je ne te conseil pas du tout boot2docker.
Ce que je te recommande pour fiabiliser / simplifier ton installation :
Utilise un hyperviseur que tu connais : VirtualBox / Oracle / n’importe quoi tant que tu connais un minimum (pour la configuration réseau)
Installe une VM Ubuntu Serveur (pas besoin d’interface graphique pour ce que tu vas faire)
Active le serveur SSH de la VM Ubuntu
Configure une règle NAT pour binder le port 22 de ta VM sur le port 22 de ta machine Windows
Connecte toi avec Putty depuis ton Windows en SSH à ta VM : tu sera dans ton environnement, ave le copier / coller etc et tu n’auras plus de problème de clavier
Procède à l’installation de Docker sur la VM grâce à ton terminal SSH
Créé un snapshot dès que tout est fonctionnel pour pas avoir à refaire tout ça en cas de problème