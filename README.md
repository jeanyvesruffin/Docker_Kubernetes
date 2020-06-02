# Docker_Kubernetes 

**L'invite de commande est TOUJOURS celui de docker.exe**

Docker est:

* Flexible : m�me les applications les plus complexes peuvent �tre conteneuris�es.
* L�ger : les conteneurs exploitent et partagent le noyau h�te, ce qui les rend beaucoup plus efficaces en termes de ressources syst�me que les machines virtuelles.
* Portable : vous pouvez cr�er localement, d�ployer sur le cloud et ex�cuter n'importe o�.
* Couplage faible : les conteneurs sont hautement autonomes et encapsul�s, vous permettant de remplacer ou de mettre � niveau l'un sans perturber les autres.
* �volutif : vous pouvez augmenter et distribuer automatiquement des r�pliques de conteneurs dans un centre de donn�es.
* S�curis� : les conteneurs appliquent des contraintes agressives et des isolements aux processus sans aucune configuration requise de la part de l'utilisateur.


## Installation Docker desktop pour windows home

	C:\ choco install virtualbox
	
	C:\ choco install docker-machine
	
� l'aide du terminal Git Bash, utilisez Docker Machine pour installer Docker Engine. Cela t�l�chargera une image Linux contenant le moteur Docker et l'ex�cutera en tant que machine virtuelle � l'aide de VirtualBox. Ex�cutez simplement la commande suivante:

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


**Pour desinstaller** Docker il faut commencer par retirer toute les machine install� se reporter � https://docs.docker.com/toolbox/toolbox_install_windows/

## Cheat sheet

![cheat sheet docker](Document/cheatSheetDocker.pdf)
<object data="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="https://github.com/jeanyvesruffin/Docker_Kubernetes/blob/master/Document/cheatSheetDocker.pdf">Download PDF</a>.</p>
    </embed>
</object>

## Demo

1. Construire une image de votre projet: allez � la racine de votre projet et tapper:
	
	docker image build -t userName/Nom_de_projet:TAG .
	
Exemple:

	docker image build -t jeanyvesruffin/projet_test:2 .
	
*Cela vas creer une image (docker image build), avec un tag (-t) avec comme nom de docker image jeanyvesruffin/projet_test ayant le tag (version) 2 et le d�mon Docker recuperera le fichier Docker pr�sent dans le repertoire courant (c'est ce que fait le . � la fin)*


Enfin remarquez que lors de la generation du dock une image Alpine est par defaut mont�.


**Attention** le nom du projet est case sensitive, il doit etre tout en minuscule


2. Verifier la creation de l'image

	docker image ls userName/Nom_de_projet:TAG
	
3. Se connecter � votre container docker 

https://hub.docker.com/settings/general

Cliquer sur votre profil>Account Setting>Security>New Access Token. Indiquer une description ex: tokken_00

Puis executer les commandes demand�es dans votre terminal.

	docker login --username jeanyvesruffin
	Renseigner votre password

4. Push sur le container distant

	docker image push jeanyvesruffin/projet_test:2

5. Tester votre application

	docker container run -d --name web -p 8000:8000 jeanyvesruffin/projet_test:2

* Demarre le conteneur du dock jeanyvesruffin/projet_test:2
* -d :  Ex�cute le conteneur en arri�re-plan et imprime l'ID du conteneur.
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
	
* Supprimer une image, dans le nom_image bien specifier le tag et -f si besoin de forcer la suppression dans le cas ou cette image est utilis�:

	docker image rm <nom_image> -f
	
Exemple:

![docker image rm <nom_image> -f](Documents/docker_image_rm.bmp)

* Liste de tous les conteneurs Docker en cours d'ex�cution!

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

Lorsque vous utilisez Docker Toolbox, l'ex�cution docker run -p 80:80 peut �tre trompeuse. Cela signifie qu'il transmettra le port 80 de votre conteneur au port 80 de votre machine Docker, pas � l'h�te Windows!

Si vous souhaitez acc�der au conteneur via votre h�te Windows, vous devez �galement transf�rer le port 80 de votre machine Docker vers cet h�te.

Je vois que vous utilisez VirtualBox, ce qui vous permet de le faire en ajoutant une entr�e dans Param�tres> R�seau> Avanc�> Transfert de port .

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
- Si desactiver activer le dans votre bios, pour ma part equipe d'une carte mere MSI, je vais dans overclock > CPU feature > puis passe l'option SVM Mode � Enable.
- Allez � la racine de l'executable docker.exe est tapper la ligne de commande ci-dessous pour skip le check

	docker-machine create default --virtualbox-no-vtx-check
	
Ouvrir l'invitez de commande **docker**

	docker quickstart terminal.exe
![DockerInstallTool](Document/installDockerTool.bmp)

**ABORT** Methode d'installation a l'aide de chocolat sans docker tool