# Docker_Kubernetes 

**L'invite de commande est TOUJOURS celui de docker.exe**

Docker est:

* Flexible : même les applications les plus complexes peuvent être conteneurisées.
* Léger : les conteneurs exploitent et partagent le noyau hôte, ce qui les rend beaucoup plus efficaces en termes de ressources système que les machines virtuelles.
* Portable : vous pouvez créer localement, déployer sur le cloud et exécuter n'importe où.
* Couplage faible : les conteneurs sont hautement autonomes et encapsulés, vous permettant de remplacer ou de mettre à niveau l'un sans perturber les autres.
* Évolutif : vous pouvez augmenter et distribuer automatiquement des répliques de conteneurs dans un centre de données.
* Sécurisé : les conteneurs appliquent des contraintes agressives et des isolements aux processus sans aucune configuration requise de la part de l'utilisateur.


## Installation Docker desktop pour windows home

	C:\ choco install virtualbox
	
	C:\ choco install docker-machine
	
À l'aide du terminal Git Bash, utilisez Docker Machine pour installer Docker Engine. Cela téléchargera une image Linux contenant le moteur Docker et l'exécutera en tant que machine virtuelle à l'aide de VirtualBox. Exécutez simplement la commande suivante:

	$ docker-machine create --driver virtualbox default

![installation](Document/install)

* default: Est le nom de  la VM

Poursuivre l'installation en suivant le lien:
	
https://www.sitepoint.com/docker-windows-10-home/


Verifier la version de votre docker

	docker -v

Lister les machines

	docker-machine ls

Lister les containers:
	
	docker container ls -all


**Pour desinstaller** Docker il faut commencer par retirer toute les machine installé se reporter à https://docs.docker.com/toolbox/toolbox_install_windows/

## Cheat sheet

![cheat sheet docker](Document/cheatSheetDocker.pdf)
<object data="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf">Download PDF</a>.</p>
    </embed>
</object>

## Demo

1. Construire une image de votre projet: allez à la racine de votre projet et tapper:
	
	docker image build -t userName/Nom_de_projet:TAG .
	
Exemple:

	docker image build -t jeanyvesruffin/projet_test:2 .
	
*Cela vas creer une image (docker image build), avec un tag (-t) avec comme nom de docker image jeanyvesruffin/projet_test ayant le tag (version) 2 et le démon Docker recuperera le fichier Docker présent dans le repertoire courant (c'est ce que fait le . à la fin)*


Enfin remarquez que lors de la generation du dock une image Alpine est par defaut monté.


**Attention** le nom du projet est case sensitive, il doit etre tout en minuscule


2. Verifier la creation de l'image

	docker image ls userName/Nom_de_projet:TAG
	
3. Se connecter à votre container docker 

https://hub.docker.com/settings/general

Cliquer sur votre profil>Account Setting>Security>New Access Token. Indiquer une description ex: tokken_00

Puis executer les commandes demandées dans votre terminal.

	docker login --username jeanyvesruffin
	Renseigner votre password

4. Push sur le container distant

	docker image push jeanyvesruffin/projet_test:2

5. Tester votre application

	docker container run -d --name web -p 8000:8000 jeanyvesruffin/projet_test:2

* Demarre le conteneur du dock jeanyvesruffin/projet_test:2
* -d :  Exécute le conteneur en arrière-plan et imprime l'ID du conteneur.
* --name : Attribue un nom au conteneur
* web : pour le demarrage du web service
* -p 8000:8000 : Publie les ports du container (8000:8000) sur l'hote 

Dans le cas ou l'erreur suivante apparait:

	 Error response from daemon: Conflict. The container name "/web" is already in use by container
	 
Verifier que le container existe deja :

	docker ps -a

Supprimer le

	docker rm -f <nom> 

## Docker command line

* Consulter tous les container:

	docker container ls
	
* Consulter tous les containers ainsi que ceux en execution

	docker container ls -all

* Consulter les images:

	docker images
	
* Supprimer une image, dans le nom_image bien specifier le tag et -f si besoin de forcer la suppression dans le cas ou cette image est utilisé:

	docker image rm <nom_image> -f
	
Exemple:

![docker image rm <nom_image> -f](Documents/docker_image_rm.bmp)

* Liste de tous les conteneurs Docker en cours d'exécution!

	docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" webserver


## Bug-fix:

Installation docker:

Installation Failed: one prerequisite is not fulfilled
Docker Desktop requires Windows 10 Pro or Enterprise version 15063 to run.

https://medium.com/@mbyfieldcameron/docker-on-windows-10-home-edition-c186c538dff3

https://docs.docker.com/toolbox/toolbox_install_windows/

Attention une configuration du bios est necessaire.

////////////////////////////////////////////////////////////

![error_VT-X](Documents/error_VT-X.bmp)

Execution docker run -p 80:80 **TROMPEUSE!!!!**:

Lorsque vous utilisez Docker Toolbox, l'exécution docker run -p 80:80 peut être trompeuse. Cela signifie qu'il transmettra le port 80 de votre conteneur au port 80 de votre machine Docker, pas à l'hôte Windows!

Si vous souhaitez accéder au conteneur via votre hôte Windows, vous devez également transférer le port 80 de votre machine Docker vers cet hôte.

Je vois que vous utilisez VirtualBox, ce qui vous permet de le faire en ajoutant une entrée dans Paramètres> Réseau> Avancé> Transfert de port .

Exemple de tutoriel avec des images: https://www.howtogeek.com/122641/how-to-forward-ports-to-a-virtual-machine-and-use-it-as-a-server/

https://medium.com/@peorth/using-docker-with-virtualbox-and-windows-10-b351e7a34adc


resolve:

https://stackoverflow.com/questions/57441382/error-with-pre-create-check-this-computer-doesnt-have-vt-x-amd-v-enabled-ena


	docker-machine create -d virtualbox --virtualbox-memory=4096 \
    --virtualbox-cpu-count=4 --virtualbox-disk-size=40960 \
    --virtualbox-no-vtx-check default

**Bug non fixe sur la methode "classique" avec download source code sur docker officiel**
Activer la virtualisation:

- Faire Ctrl+Alt+Suppr > Gestionnaire de tache > Performance
- Puis controler le parametre de virtualisation.
- Si desactiver activer le dans votre bios, pour ma part equipe d'une carte mere MSI, je vais dans overclock > CPU feature > puis passe l'option SVM Mode à Enable.
- Allez à la racine de l'executable docker.exe est tapper la ligne de commande ci-dessous pour skip le check

	docker-machine create default --virtualbox-no-vtx-check
	
Ouvrir l'invitez de commande **docker**

	docker quickstart terminal.exe
![DockerInstallTool](Document/installDockerTool.bmp)

**ABORT** Methode d'installation a l'aide de chocolat sans docker tool