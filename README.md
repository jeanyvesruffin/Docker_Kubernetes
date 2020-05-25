# Docker_Kubernetes

* Flexible : m�me les applications les plus complexes peuvent �tre conteneuris�es.
* L�ger : les conteneurs exploitent et partagent le noyau h�te, ce qui les rend beaucoup plus efficaces en termes de ressources syst�me que les machines virtuelles.
* Portable : vous pouvez cr�er localement, d�ployer sur le cloud et ex�cuter n'importe o�.
* Couplage faible : les conteneurs sont hautement autonomes et encapsul�s, vous permettant de remplacer ou de mettre � niveau l'un sans perturber les autres.
* �volutif : vous pouvez augmenter et distribuer automatiquement des r�pliques de conteneurs dans un centre de donn�es.
* S�curis� : les conteneurs appliquent des contraintes agressives et des isolements aux processus sans aucune configuration requise de la part de l'utilisateur.


Bug-fix: Installation Failed: one prerequisite is not fulfilled
Docker Desktop requires Windows 10 Pro or Enterprise version 15063 to run.

https://medium.com/@mbyfieldcameron/docker-on-windows-10-home-edition-c186c538dff3

https://docs.docker.com/toolbox/toolbox_install_windows/

## Installation Docker desktop pour windows home


Activer la virtualisation:

- Faire Ctrl+Alt+Suppr > Gestionnaire de tache > Performance
- Puis controler le parametre de virtualisation.
- Si desactiver activer le dans votre bios, pour ma part equipe d'une carte mere MSI, je vais dans overclock > CPU feature > puis passe l'option SVM Mode � Enable.
- Allez � la racine de l'executable docker.exe est tapper la ligne de commande ci-dessous pour skip le check

	docker-machine create default --virtualbox-no-vtx-check
	
![DockerInstallTool](Document/installDockerTool.bmp)



## Demo

1. Creation d'une image du container
2. Enregistre dans le registre
3. Demarrage du container

1. Allez � la racine de votre projet et tapper:

