# Docker_Kubernetes 


Docker est:

* Flexible : m�me les applications les plus complexes peuvent �tre conteneuris�es.
* L�ger : les conteneurs exploitent et partagent le noyau h�te, ce qui les rend beaucoup plus efficaces en termes de ressources syst�me que les machines virtuelles.
* Portable : vous pouvez cr�er localement, d�ployer sur le cloud et ex�cuter n'importe o�.
* Couplage faible : les conteneurs sont hautement autonomes et encapsul�s, vous permettant de remplacer ou de mettre � niveau l'un sans perturber les autres.
* �volutif : vous pouvez augmenter et distribuer automatiquement des r�pliques de conteneurs dans un centre de donn�es.
* S�curis� : les conteneurs appliquent des contraintes agressives et des isolements aux processus sans aucune configuration requise de la part de l'utilisateur.

Kubernetes est:

https://www.redhat.com/fr/topics/containers/what-is-kubernetes#:~:text=Le%20principal%20avantage%20de%20la,de%20machines%20physiques%20ou%20virtuelles.


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
	
Oracle VM est le serveur de virtualisation. Oracle VM Server pour machines x86 inclut l'hyperviseur libre et open-source Xen, supporte Microsoft Windows, diff�rentes distributions Linux2 (Ubuntu, Debian, Fedora, openSUSE) et int�gre une console de gestion Web. Oracle VM met en �uvre la pile d'applications Oracle Applications test�es et certifi�es pour un environnement de virtualisation en entreprise.
	
	
	C:\ choco install docker-machine

Docker est un logiciel libre permettant de lancer des applications dans des conteneurs logiciels.

## Creation d'une machine virtuel

https://docs.docker.com/machine/reference/create/

Creation d'une machine virtuelle virtualbox (--driver virtualbox) portant le nom de vmDocker00

	$ docker-machine create --driver virtualbox vmDocker00
	
![installation machine docker](Document/install.bmp)


### Configuration ports de la VM

Dans Oracle VM, selectionner votre machine (vmDocker00) > Settings>Network>Adapter1>Port Forwarding et y renseigner les ports de votre VM qui seront expos�s lorts de l'execution de votre dock:


![vm_ports](Document/vm_ports.bmp)

### Suppression erreurs parametres graphique de la VM

![vm_ports](Document/err_gui_vm.bmp)

![vm_ports](Document/solve_00.bmp)

### Configuration des volumes

Dans Oracle VM, selectionner votre machine (vmDocker00) > Settings>Shared Folders et y ajouter les volumes � monter en virtualisation et cocher Auto-mount et permanent:

![vm_volume](Document/config_volu.bmp)

### Demarrer votre machine virtuelle

	$ docker-machine start vmDocker00

### Configuration variables d'environnement docker

	docker-machine env vmDocker00
	eval $(docker-machine env vmDocker00 --shell linux)
	
![dock_machine_run](Document/dock_machine_run.bmp)

Vous devrez d�finir les variables d'environnement � chaque d�marrage d'un nouveau terminal Git Bash. Si vous souhaitez �viter cela, vous pouvez copier la sortie eval et l'enregistrer dans votre fichier .bashrc. �a devrait ressembler a quelque chose comme ca:

	export DOCKER_TLS_VERIFY="1"
	export DOCKER_HOST="tcp://192.168.99.103:2376"
	export DOCKER_CERT_PATH="C:\Users\admin\.docker\machine\machines\vmDocker00"
	export DOCKER_MACHINE_NAME="vmDocker00"
	export COMPOSE_CONVERT_WINDOWS_PATHS="true"

**IMPORTANT**: pour DOCKER_CERT_PATH, vous devrez modifier le chemin d'acc�s au fichier Linux au format de chemin d'acc�s Windows. Notez �galement qu'il est possible que l'adresse IP attribu�e soit diff�rente de celle que vous avez enregistr�e � chaque d�marrage de la machine virtuelle par d�faut.


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
	
- -d - ex�cuter le conteneur en mode d�tach� (en arri�re-plan)
- -p 80:80 - mapper le port 80 de l'h�te au port 80 dans le conteneur 
- docker/getting-started - l'image � utiliser

Equivalent �

	docker run -dp 80:80 docker/getting-started

- -dp equivalent � -d et -p

Puis clique sur open port 80 

Dans la dossier de votre projet.

### Creation de l'image de conteneur de l'application
Cr�ez un fichier nomm� Dockerfile avec le contenu suivant.


	FROM node:12-alpine
	WORKDIR /app
	COPY . .
	RUN yarn install --production
	CMD ["node", "/app/src/index.js"]

Cr�ez l'image conteneur � l'aide de la docker buildcommande.

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

Nettoie toutes les ressources - images, conteneurs, volumes et r�seaux - qui sont en suspens (non associ�es � un conteneur) 

	docker system prune
	
Supprimer en plus tous les conteneurs arr�t�s et toutes les images non utilis�es

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