# **Partie 4**

_19/03/2026_

Configuration du DHCP.


## **Environnement**

|                           |                  |
| ------------------------- | ---------------- |
| **Élément**               | **Détail**       |
| Nom de l'étendue          | LAN-Lab          |
| Adresse IP de début       | 192.168.1.100    |
| Adresse IP de fin         | 192.168.1.200    |
| Masque de sous-réseau     | 255.255.255.0    |
| Durée du bail             | 8 jours          |
| Rôles actifs sur SRV-DC01 | AD DS, DNS, DHCP |


## **Ce que j'ai fait**

J'ai commencé par ajouter le rôle DHCP à SRV-DC01 via l'assistant d'ajout de rôles, ce qui inclut une étape d'autorisation du serveur DHCP dans AD DS. J'ai ensuite ouvert l'outil DHCP afin de créer une nouvelle étendue en IPv4. J'ai défini la plage d'adresses distribuables de façon à exclure l'adresse IP fixe de mon DC (192.168.1.1), qui se trouve ainsi en dehors de l'étendue. J'ai indiqué comme routeur (passerelle) et comme DNS l'adresse IP de mon DC, et noté comme nom de domaine DNS monlab.local. J'ai ensuite exploré la console DHCP et son arborescence afin de confirmer la présence des bons paramètres, notamment la plage d'adresses et les options d'étendue.

J'ai ensuite exécuté la commande Get-DhcpServerv4Scope afin de confirmer la bonne application de l'étendue LAN-Lab, puis effectué un snapshot.

Pas de véritables problèmes rencontrés sur cette étape.


## **Concepts et apprentissages**

Le bail DHCP permet d'attribuer temporairement des adresses IP, sur une plage définie, à chaque nouvelle connexion au réseau, et de réserver ces adresses pendant une durée déterminée.

Le serveur doit bénéficier d'une IP fixe car c'est à lui que la requête DHCP est adressée lors d'une nouvelle connexion ; si son adresse IP n'est pas fixe, les requêtes des clients ne pourront pas l'atteindre de manière fiable.


## **Commandes et outils utilisés**

**Étendue DHCP**

Plage d'adresses IP ouverte à attribution par le DHCP, selon plusieurs paramètres et conditions définis par l'administrateur.

**Réservation DHCP**

Mécanisme permettant d'exclure une adresse IP de la plage distribuable, afin de la maintenir disponible pour un usage fixe, comme l'adresse d'un serveur.

**Autorisation DHCP dans AD DS**

Mécanisme de validation qui empêche tout serveur DHCP non déclaré de distribuer des adresses sur le réseau. Seuls les serveurs explicitement autorisés par un administrateur de domaine peuvent opérer.

**Get-DhcpServerv4Scope (PowerShell)**

Permet d'afficher les étendues DHCP créées ainsi que leur plage, la durée du bail, leur état d'activation et leur nom.

**Console DHCP**

Permet de gérer, créer ou supprimer des étendues en IPv4 ou en IPv6. Permet également d'établir des options de serveur ou des stratégies à l'ensemble des étendues du serveur.