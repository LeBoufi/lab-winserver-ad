# **Partie 2**

_18/03/2026_

Préparation du serveur et installation du rôle AD DS.


## **Environnement**

|                       |                                                        |
| --------------------- | ------------------------------------------------------ |
| **Élément**           | **Détail**                                             |
| Nom du serveur        | SRV-DC01                                               |
| Adresse IP            | 192.168.1.1                                            |
| Masque de sous-réseau | 255.255.255.0                                          |
| DNS configuré         | 192.168.1.1 (le serveur se désigne lui-même comme DNS) |
| Rôle installé         | AD DS                                                  |


## **Ce que j'ai fait**

J'ai tout d'abord renommé le serveur afin de l'identifier plus facilement. J'ai choisi le nom SRV-DC01 : le préfixe SRV indique qu'il s'agit d'un serveur et non d'un poste de travail, et DC01 désigne son rôle de premier contrôleur de domaine. Je lui ai ensuite attribué une adresse IPv4 fixe, puis me suis familiarisé avec le Server Manager de Windows Server 2025 et ses différentes fonctionnalités. J'ai enfin procédé à l'installation du rôle AD DS via le Server Manager, puis effectué un snapshot une fois l'installation complétée.

Pas de véritables problèmes rencontrés sur cette étape.


## **Concepts et apprentissages**

L'intérêt d'une IP fixe pour un serveur : elle garantit qu'il reste joignable par les autres nœuds du domaine, son adresse ne changeant pas de manière dynamique.

Découverte du Server Manager et de son intérêt comme interface graphique centralisée, plus accessible qu'une gestion exclusivement en ligne de commande.

Le rôle d'AD DS dans la gestion et l'attribution des comptes, le lien entre les différentes stations de travail d'un réseau et la mise en place des règles attribuées aux machines du réseau, essentiel en milieu professionnel pour des questions d'efficacité, de sécurité et de réglementation de l'outil de travail.

Rôle serveur : fonction spécialisée qu'un serveur assure au sein d'un réseau afin de contribuer à son bon fonctionnement (ex. : AD DS, DNS, DHCP).


## **Commandes et outils utilisés**

**AD DS (Active Directory Domain Services)**

Annuaire centralisé des comptes utilisés dans les stations de travail d'un réseau, comportant les utilisateurs, les ordinateurs du réseau, les groupes d'utilisateurs et les politiques (GPO) à appliquer aux différents utilisateurs, ordinateurs ou groupes.

**Contrôleur de domaine (DC)**

Serveur sur lequel tourne l'AD DS, et qui va donc stocker la base de données de tous les objets nécessaires à son bon fonctionnement. Il vérifie les mots de passe lors d'une connexion, distribue les politiques aux machines du réseau et fait tourner le DNS intégré permettant aux machines de se trouver entre elles. On en déploie généralement deux pour assurer une redondance.

**DNS (Domain Name System) intégré**

Le DNS est le système chargé de traduire un nom de domaine en adresse IP. Dans un réseau d'entreprise, il permet d'accéder aux différents nœuds (serveurs et stations de travail) par leur nom plutôt que par leur adresse IP. Il est ici essentiel au fonctionnement d'AD DS, qui localise le DC via son nom de domaine (ex. : monlab.local). Il est dit 'intégré' car il est installé automatiquement lors de la promotion du serveur en contrôleur de domaine.

**ipconfig /all (PowerShell)**

Utilisée pour vérifier la bonne attribution de l'IPv4 fixe.