# **Partie 5**

_21/03/2026_

Création de la VM cliente et jonction au domaine.


## **Environnement**

|                                 |                                    |
| ------------------------------- | ---------------------------------- |
| **Élément**                     | **Détail**                         |
| Nom de la VM cliente            | WIN10-Client                       |
| OS invité                       | Windows 10 Entreprise Evaluation   |
| RAM allouée                     | 2048 Mo                            |
| Adresse IP reçue                | 192.168.1.100                      |
| Méthode d'attribution IP        | Attribution par DHCP sur l'étendue |
| Domaine rejoint                 | monlab.local                       |
| Compte utilisé pour la jonction | MONLAB/Administrateur              |


## **Ce que j'ai fait**

J'ai tout d'abord téléchargé l'ISO de Windows 10 Entreprise Evaluation afin de créer une nouvelle VM avec 2048 Mo de RAM, 40 Go de disque, et de la connecter au réseau interne 'Réseau Lab'. J'ai ensuite procédé à l'installation de Windows 10 dans la VM.

J'ai ensuite vérifié la connectivité au réseau et à l'autre VM via la commande ipconfig /all, qui confirme que l'IP attribuée (192.168.1.100) est bien dans la plage d'adresses de l'étendue mise en place au préalable. Les deux VM sont bien sur le même réseau et le DHCP a correctement fonctionné. J'ai également envoyé un ping au serveur via la commande ping 192.168.1.1 afin de vérifier la qualité de la connexion, avec comme résultat entre 0 et 1 ms de temps de réponse.

Pour confirmer la connexion, j'ai testé la résolution DNS via la commande nslookup monlab.local, qui retourne l'IP du DNS, 192.168.1.1.

J'ai ensuite joint le domaine via la fonction avancée de renommage sur le client, j'ai inscrit le nom du domaine monlab.local et complété la jonction avec l'identifiant et le mot de passe administrateur. Après redémarrage, j'ai pu ouvrir une session sur la VM cliente avec le compte administrateur DC.

J'ai vérifié la jonction sur le DC dans la console Utilisateurs et ordinateurs Active Directory, dans le conteneur Computers où la VM cliente apparaît bien. J'ai également confirmé l'attribution de l'IP dans les baux DHCP de la console DHCP, attestant du bon fonctionnement du DHCP.

J'ai fini par un snapshot sur chaque VM après la jonction.


## **Concepts et apprentissages**

Le domaine confie la gestion du réseau au DC via AD DS : tout utilisateur disposant d'un compte de domaine peut ainsi ouvrir une session sur n'importe quel poste membre, sous réserve des autorisations définies par le DC. Dans un workgroup, les utilisateurs sont propres à l'ordinateur et ne peuvent pas être simplement interchangés avec un autre. De plus, dans un domaine géré par un DC, il est possible de restreindre plus aisément les capacités de chaque utilisateur sur les ordinateurs composant le domaine, réduisant ainsi les risques de sécurité. Le domaine apparaît indispensable en entreprise car l'ordinateur y est avant tout un outil de travail, propriété de l'entreprise, laquelle a donc pleinement le droit d'y imposer ses règles.

Quand un poste rejoint le domaine, il interroge le DNS pour obtenir l'adresse IP du DC du domaine qu'il souhaite rejoindre (d'où l'intérêt d'un DNS fonctionnel). Le client contacte ensuite le DC et présente ses identifiants ; le DC utilise le protocole Kerberos pour authentifier. Le DC crée ensuite un objet ordinateur dans son AD DS, dans le conteneur Ordinateurs, cet objet contenant le nom de l'ordinateur, son OS, la date de jonction et le compte machine (se terminant par $). Un échange de clé secrète permet à l'ordinateur de s'authentifier automatiquement auprès du DC. Le redémarrage permet à Windows de se configurer en mode membre de domaine, de contacter directement le DC dès le démarrage, de s'identifier avec son compte machine pour récupérer les GPO, et au service Netlogon de s'initialiser afin de maintenir un canal sécurisé entre le poste de travail et le DC.

Les raisons pour lesquelles la première connexion avec un compte de domaine est lente sont multiples. Le client transmet ses identifiants au DC via Kerberos. Le DC les valide et retourne un ticket d'authentification accompagné des GPO applicables à cet utilisateur sur ce poste. Windows crée alors un profil local, les lecteurs réseau mappés par GPO se connectent, et la session s'ouvre. L'ensemble de ces opérations explique la lenteur de cette première connexion.


## **Commandes et outils utilisés**

|                       |                                                                                                                  |                                                                                 |
| --------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Commande**          | **Utilité**                                                                                                      | **Résultat observé**                                                            |
| whoami                | Permet d'identifier quel compte est connecté sur un poste.                                                       | Affiche un compte utilisateur par défaut créé par l'installation de Windows 10. |
| ipconfig /all         | Permet de voir la configuration de la connexion réseau d'un ordinateur.                                          | Affiche l'IP 192.168.1.100 ; le DHCP a bien fonctionné.                         |
| ping 192.168.1.1      | Permet d'envoyer un ping à une adresse IP afin de tester la connexion entre l'ordinateur et sa cible, ici le DC. | Entre 0 et 1 ms ; connexion correcte.                                           |
| nslookup monlab.local | Permet de vérifier l'état du DNS et son adresse IP.                                                              | Affiche l'adresse IP du DC, 192.168.1.1.                                        |
