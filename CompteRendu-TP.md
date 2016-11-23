*** Code Mardown ***
*********************

Vendredi 18/11/2016:

**TP1**

HYPERVISEUR VIRTUAL BOX & MACHINE VIRTUELLE
Installation de la machine virtuel Debian dans VirtualBox.
Installation des packets (sous root) aptitude  install lynx sudo tcpdump vim.

SUDO
Prise d'information sur commande sudo.
Droit d'accès sudo pour notre utilisateur:
	- visudo (et non vim /etc/sudoers) qui permet d'éditer le fichier /etc/sudoers
	- rajouter la ligne: nomutilisateur ALL=(ALL:ALL) ALL
	- enregistrer et relancer le service sudo (service sudo restart)
	- tester si l'utilisateur a bien les droit de sudo
Nom des groupes de l'utilisateur (avec la commande groups):
	- cdrom		- floppy	- audio		- dip
	- video		- plugdev	- netdev	- bluetooth

CLONAGE
Pour faire un clone de la VM "debian_base", il suffi de faire un clic droit sur celle-ci et de cliquer sur "Cloner" (ou raccourci Ctrl+O).

SNAPSHOT
rm -rf / (cette ligne de commande supprime tout les fichiers)
En essayant de relancer le snapshot, on tombe sur grub rescue puisque tout à été supprimer au préalable.
Il suffi de restaurer le snapshot "Avant rm" afin de pouvoir relancer la VM.

CONFIGURATION RÉSEAUX
Après avoir éxecuter les commandes (ip addr, lynx), on tombre sur une page qui nous demande de s'identifier avec nos username et mot de passe. Un e fois fait, il suffit de faire shift+G (Aller à) pour rechercher la page https://www.duckduckgo.com. De la même manière, on peut se connecter à la page https://www.startpage.com on observe alors que cette page est en faite notre page d'acceuil sur un navigateur web affiché seulement en ligne de commande (sans graphique).
On constate que l'on peut naviguer sur internet avec des lignes de commandes lorsqu'on possède unsystème d'exploitation sans interface graphique.

**TP2**

LA PASSERELLE
0) Configuration de la carte réseau de la VM "Gateway" en mode d'accès réseau par pont.
1) Il suffit de faire la commande apt-get install ssh (qui va installer toutes les autres librairies dont on aura besoin) ou avec la commande aptitude ssh.
Il faut néanmoins veiller avant celà à bien se connecter au réseau avec lynx.
2) Pour se connecter en ssh, il suffit de faire la commande ip a et de selectionner l'adresse IP de la VM Gateaway. Une fois cela réalisé, il faut lancer la commande ssh nomutilisateur@adresseIPdelaVM sur la terminal de la machine.
En essayant avec l'utilisateur root, un message apparait lorsqu'on entre le mot de passe qui nous informe que nous n'avons pas la permission.
3) Pour la modification du port de lecture de ssh, il suffit d'ouvrir le fichier sshd_config via la commande nano /etc/ssh/sshd_config et de modifier le port (ici 22 en 26).
Veiller ici à exécuter cette commande en super utilisateur sinon la permission ne sera pas accordée.
En réessayant la connection ssh quentin@192.168.99.132 sur le terminal, on observe que la connection est impossible puisqu'il faut préciser le port sur lequel on veut se connecter ce qui donne ssh quentin@192.168.99.132 -p 26.
4) 
5) 
6) 
7) 

