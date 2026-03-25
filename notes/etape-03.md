# **Partie 3**

_19/03/2026_

Promotion en DC et création du domaine.


## **Environnement**

|                            |                     |
| -------------------------- | ------------------- |
| **Élément**                | **Détail**          |
| Nom de domaine             | monlab.local        |
| Nom NetBIOS                | MONLAB              |
| Niveau Fonctionnel Forêt   | Windows Server 2025 |
| Niveau Fonctionnel Domaine | Windows Server 2025 |
| Adresse IP du DC           | 192.168.1.1         |
| Rôles actifs sur SRV-DC01  | AD DS, DNS          |


## **Ce que j'ai fait**

J'ai commencé par le lancement de l'assistant de promotion afin de faire de SRV-DC01 le DC du nouveau domaine. J'ai ainsi créé une nouvelle forêt avec un seul domaine racine, monlab.local, que j'ai paramétré avec les spécifications vues ci-dessus. Le serveur a redémarré et je me suis connecté pour la première fois à mon nouveau domaine, dont mon DC est le seul nœud pour le moment. J'ai exploré la console AD, notamment les sections Utilisateurs et Ordinateurs, j'ai vérifié le DNS et effectué un snapshot du contrôleur de domaine opérationnel.

Pas de véritables problèmes rencontrés sur cette étape.


## **Concepts et apprentissages**

Ce qu'est une forêt et la hiérarchie des domaines (forêts, domaines et sous-domaines).

Le passage d'un serveur vers un rôle de DC et les conséquences que cela implique.

Les nouveaux concepts découverts : le domaine, le niveau fonctionnel, le SYSVOL.

Les différentes consoles et leur importance dans la gestion quotidienne du domaine.

**Forêt Active Directory**

Échelon hiérarchique supérieur à celui du domaine. Elle regroupe tous les domaines tournant sur la même infrastructure, permettant une meilleure communication et coordination entre eux, et constitue également une limite de sécurité. Tout domaine doit appartenir à une forêt ; ainsi, dans la majorité des PME, la forêt AD de l'entreprise ne comporte qu'un seul domaine racine.

**Domaine**

Groupement administratif de plusieurs nœuds de réseau au sein d'une même infrastructure. Le DC est un serveur en charge de l'authentification des utilisateurs, de la gestion des comptes et de l'application des politiques du domaine.

**Niveau fonctionnel**

Détermine quelles capacités AD DS sont disponibles pour la forêt ou le domaine géré, et quelle version de Windows Server doit tourner sur les DC. Le niveau fonctionnel du domaine ne peut pas être inférieur à celui de la forêt dont il fait partie.

**DSRM (Directory Services Restore Mode)**

Mode de démarrage du DC permettant d'effectuer des opérations de maintenance ou de restauration sur la base de données AD DS, notamment après une panne critique.

**SYSVOL**

Dossier présent sur tous les DC, contenant les fichiers de stratégies de groupe (GPO) et les scripts de connexion. Il est répliqué sur l'ensemble des DC du domaine et doit être accessible par toutes les stations de travail.

**Catalogue global**

Rôle endossé par un DC consistant à maintenir un annuaire étendu d'AD, comportant en plus une réplique partielle de tous les attributs de tous les domaines de la forêt et de leurs objets. C'est l'annuaire central que consultent les autres DC pour effectuer des recherches.


## **Commandes et outils utilisés**

**nslookup monlab.local (PowerShell)**

Utilisée pour vérifier la bonne mise en place et le bon fonctionnement du DNS ainsi que son adresse IP.

**Console Utilisateurs et ordinateurs AD**

Permet de gérer, ajouter ou supprimer des profils utilisateurs sur le réseau. Elle permet aussi d'attribuer certains groupes d'utilisateurs ou utilisateurs spécifiques à certains ordinateurs ; c'est la console principale dans la gestion quotidienne du domaine.

**Console DNS**

Permet de voir quelle IP correspond à quel nom dans le domaine. La Zone de recherche directe permet d'explorer les noms contenus dans le domaine monlab.local. La Zone de recherche inversée permet de retrouver un nom à partir d'une IP.