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

### Creation d'une machine virtuel

https://docs.docker.com/machine/reference/create/

Dans le Gitbash d'une machine virtuelle virtualbox (--driver virtualbox) portant le nom de vmDocker00

	$ docker-machine create --driver virtualbox vmDocker00
	
![installation machine docker](Document/install.bmp)



## Ressources

https://www.sitepoint.com/docker-windows-10-home/