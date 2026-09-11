story1 :

US 1: Mise en place d’un environnement sécurisé

EN TANT QUE analyste malware,

JE VEUX exécuter le Malware dans un environnement isolé,

AFIN DE protéger le système hôte et observer le comportement réel du malware.

Pour mettre en place un environnement sécurisé, nous devons dans un premier temps créer une VM (machine virtuelle). Nous pouvons utiliser différent logiciel pour en créer une, 
virtualbox pour les membres possédant un ordinateur windows, UTM pour les utilisateurs de mac. Étant donné que le malware a été conçue pour infecter des ordinateur utilisant
windows, les VM devront l'avoir comme systeme d'exploitation.

Une fois la machine virtuelle créer, nous pouvons télécharger le dossier zip contenant le virus, nous devons cependant faire attention à ne pas encore le décompresser pour le 
moment. Dans un premier temps, nous devons nous assurer que notre machine virtuelle ne soit pas connecter à notre ordinateur qui fait tourner la VM, ni au réseau de celui ci
ou internet. Nous pouvons le déconnecter de la NAT qui le relie à notre PC et internet. 
Pour vérifier que la VM ne soit plus liier à notre ordinateur, nous pouvons faire un ping vers ce dernier. D'après l'image 1 que l'on voie, l'ordinateur ne répond pas au ping,
il ne sont donc plus connecter l'un à l'autre.
Nous devons aussi nous assurer que la machine virtuel et notre ordinateur n'ont pas de dossier, presse papier ou glisser déposer partagé. Nous pouvons voir dans les photos 2 
et 3 que ceci est bien désactivé. Notre environnement est prêt à executer le malware.

Avant toute execution, nous devons faire un save state de la VM. Cela revient à garder une sauvegarde de la machine virtuelle avant d'executer le malware. Cela nous permettra 
après l'execution du malware de revenir sur un environnement sain si l'on a besoin de reconnecter notre VM à notre ordinateur sans risque.

Nous décompressons alors le dossier contenant le malware et executons l'un des programmes présent dans celui-ci. Un invité de commande s'ouvre et affiche un message. Nous
pouvons aussi voir qu'il est noté que certains dossier soit copié.
 


