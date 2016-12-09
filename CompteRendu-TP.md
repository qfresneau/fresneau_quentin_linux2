# Compte-rendu TP Linux 2
*************************

-----
##TP1
-----

####HYPERVISEUR VIRTUAL BOX & MACHINE VIRTUELLE
Installation de la machine virtuel Debian dans VirtualBox.
Installation des packets (sous root) aptitude  install lynx sudo tcpdump vim.

####SUDO
Prise d'information sur commande sudo.
Droit d'accès sudo pour notre utilisateur:
	- visudo (et non vim /etc/sudoers) qui permet d'éditer le fichier /etc/sudoers
	- rajouter la ligne: nomutilisateur ALL=(ALL:ALL) ALL
	- enregistrer et relancer le service sudo (service sudo restart)
	- tester si l'utilisateur a bien les droit de sudo
Nom des groupes de l'utilisateur (avec la commande groups):
	- cdrom		- floppy	- audio		- dip
	- video		- plugdev	- netdev	- bluetooth

####CLONAGE
Pour faire un clone de la VM "debian_base", il suffi de faire un clic droit sur celle-ci et de cliquer sur "Cloner" (ou raccourci Ctrl+O).

####SNAPSHOT
rm -rf / (cette ligne de commande supprime tout les fichiers)
En essayant de relancer le snapshot, on tombe sur grub rescue puisque tout à été supprimer au préalable.
Il suffi de restaurer le snapshot "Avant rm" afin de pouvoir relancer la VM.

####CONFIGURATION RÉSEAUX
Après avoir éxecuter les commandes (ip addr, lynx), on tombre sur une page qui nous demande de s'identifier avec nos username et mot de passe. Un e fois fait, il suffit de faire shift+G (Aller à) pour rechercher la page https://www.duckduckgo.com. De la même manière, on peut se connecter à la page https://www.startpage.com on observe alors que cette page est en faite notre page d'acceuil sur un navigateur web affiché seulement en ligne de commande (sans graphique).
On constate que l'on peut naviguer sur internet avec des lignes de commandes lorsqu'on possède unsystème d'exploitation sans interface graphique.

-----
##TP2
-----

####FEUILLE DE ROUTE

0) Configuration de la carte réseau de la VM "Gateway" en mode d'accès réseau par pont.

1) Il suffit de faire la commande apt-get install ssh (qui va installer toutes les autres librairies dont on aura besoin) ou avec la commande aptitude ssh.
Il faut néanmoins veiller avant celà à bien se connecter au réseau avec lynx.

2) Pour se connecter en ssh, il suffit de faire la commande ip a et de selectionner l'adresse IP de la VM Gateaway. Une fois cela réalisé, il faut lancer la commande ssh nomutilisateur@adresseIPdelaVM sur la terminal de la machine.
En essayant avec l'utilisateur root, un message apparait lorsqu'on entre le mot de passe qui nous informe que nous n'avons pas la permission.

3) Pour la modification du port de lecture de ssh, il suffit d'ouvrir le fichier sshd_config via la commande nano /etc/ssh/sshd_config et de modifier le port (ici 22 en 26).
Veiller ici à exécuter cette commande en super utilisateur sinon la permission ne sera pas accordée.
En réessayant la connection ssh quentin@192.168.99.132 sur le terminal, on observe que la connection est impossible puisqu'il faut préciser le port sur lequel on veut se connecter ce qui donne ssh quentin@192.168.99.132 -p 26.

4) Pour commencer, il faut lancer la commande ssh-keygen sur le terminal de l'ordinateur afin de créer une clé ssh. Suite à ça, il faut envoyer la clé publique vers le port de la machine souhaité (ici la VM) avec la commande ssh-copy-id -i ~/.ssh/id_rsa.pub quentin@192.168.99.132 -p 26. Après cela, le terminal nous demande le mot de passe de la session de la VM afin de se connecter dirrectement à la VM (sans demande de password) les prochaines fois.

5) Une fois l'échange de clé réalisé, il suffit de lancer la commande ssh quentin@192.168.99.132 -p 26

6) L'installation des paquets pour le serveur dhcp se fait grâce à la commande apt-get install isc-dhcp-server en veillant encore une fois à bien être connecter au réseau.
Note : On ne peut être connecter à internet avec la VM et l'ordinateur en même temps ?

7)
**Sur le terminal de l'odrinateur (en ssh)
mv dhcpd.conf dhcpd.conf.old (pour expliquer option)
fichier de base garder.

GATEAWAY
`nano /etc/dhcp/dhcp.conf :`

`# Le fichier contient (aide dans les commentaires)`
`ddns-update-style none;`
`# DNS de Google`
`# Si la machine sert de DNS`
`default-lease-time 600;`
`max-lease-time 7200;`
`authoritative;`
`log-facility local7;`
`subnet 192.168.1.0 netmask 255.255.255.0 {`
`    range 192.168.1.100 192.168.1.120;`
`    option subnet-mask 255.255.255.0;`
`    option broadcast-address 192.168.1.255;`
`    option routers 192.168.1.254;`
`    interface eth1;`
`}`
`subnet 192.168.2.0 netmask 255.255.255.0 {`
`    range 192.168.2.100 192.168.2.120;`
`    option subnet-mask 255.255.255.0;`
`    option broadcast-address 192.168.1.255;`
`    option routers 192.168.1.254;`
`    interface eth1;`
`}`
`subnet 192.168.2.0 netmask 255.255.255.0 {`
`    range 192.168.2.100 192.168.2.120;`
`    option subnet-mask 255.255.255.0;`
`    option broadcast-address 192.168.2.255;`
`    option routers 192.168.2.254;`
`    interface eth2;`
`}`

`host serveurweb {`
`        hardware ethernet 08:00:27:2d:11:7a;`
`        fixed-address 192.168.2.10;`
`}`

sudo service isc-dhcp-server restart

client et serveur_web (que les trois premieres lignes)
sudo nano /etc/network/interfaces :


`# The primary network interface`
`allow-hotplug eth0`
`iface eth0 inet dhcp`

`allow-hotplug eth1`
`iface eth1 inet static`
`        address 192.168.1.254`
`        netmask 255.255.255.0`

`allow-hotplug eth2`
`iface eth2 inet static`
`        address 192.168.2.254`
`        netmask 255.255.255.0`


**Sur la VM Serveur_web :
`service networking restart`
`ifconfig pour vérifier s'il a bien pris l'adresse écrit.`

**Sur la VM Serveur_web :
`service networking restart`
`ifconfigeth1 pour vérifier s'il a bien pris l'adresse écrit. (grace à la mac adress (option hagrdware))`
`ifconfigeth2 pour vérifier s'il a bien pris l'adresse écrit.`


Configuration du réseau de Serveur web :
réseau en NAT, nom natNetwork2

Configuration du réseau de Gateaway : (3 cartes réseau)
la premiere:
réseau acces par pont, nom enp0s25
la deuxieme:
réseau NAT, nom natnetwork1
la troisieme:
réseau NAT, nom natnetwork2
deux réseau nat network:
natnetwork1, natnetwork2

*Sources:*
<https://www.molivier.com/isc-dhcp-server-debian.html>

-----
##TP3
-----
