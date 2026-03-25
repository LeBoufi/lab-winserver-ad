# Lab Windows Server 2025 – Active Directory
Durée : 6 jours

## Technologies : 

VirtualBox, Windows Server 2025, Windows 10, Active Directory, DNS, DHCP, GPO

## Infrastructure déployée :

Machine      OS	                    Rôles	            IP
SRV-DC01 	 Windows Server 2025 	AD DS, DNS, DHCP 	192.168.1.1 
WIN10-CLIENT Windows 10 Enterprise 	Poste client     	192.168.1.100 

## Ce qui a été configuré :

Un domaine Active Directory complet a été établi sous le nom monlab.local sur un contrôleur de domaine Windows Server 2025 hébergé sous VirtualBox 7.0.22. Le domaine intègre un serveur DNS et un serveur DHCP configurés pour distribuer automatiquement les adresses IP sur le réseau interne. La structure organisationnelle comprend trois Unités d’Organisations principales (Utilisateurs, Direction, Groupes) subdivisées en sous-OU par département (Informatique, Direction, Comptabilité) pour les utilisateurs, par type d’ordinateur (Serveurs ou Postes clients) et enfin 6 comptes utilisateurs répartis dans leurs OU respectives, et trois groupes de sécurité à étendue globale. Un poste client Windows 10 a été joint au domaine et placé sous l’OU – Postes clients. 3 stratégies de groupes ont été déployées : une GPO imposant un fond d’écran commun, une GPO restreignant l’accès au panneau de configuration pour les utilisateurs standards , et une GPO mappant automatiquement un lecteur réseau partagé à la connexion de chaque utilisateur du domaine.

## Compétences apprises :

    • Installation et configuration de Windows Server 2025 sous hyperviseur VirtualBox
    • Déploiement et administration d’un domaine Active Directory (AD DS)
    • Configuration d’un serveur DNS intégré à Active Directory et d’un serveur DHCP avec étendue et options réseau
    • Création et organisation d’une structure OU, gestion des utilisateurs et groupes de sécurité
    • Jonction d’un poste Windows 10 à un domaine Active Directory
    • Création, liaison et débogage de stratégies de groupe (GPO) : paramètres utilisateur, restrictions, préférences de mappage réseau
    • Utilisation des outils de diagnostic : gpresult, gpupdate, nslookup, ip config, ping, observateur d’événements
    • Gestion des permissions de partage réseau des permissions NTFS (principe du double verrou)
    • Prise de snapshots et gestion d’environnements virtualisés

## Incidents et résolutions :

    • GPO de fond d’écran active mais non effective sur le poste client
        ◦ gpresult confirmait son application sur WIN10-CLIENT mais le fond d’écran ne changeait pas. 
        ◦ Le chemin de l’image (qui était le bon chemin UNC) n’était pas le problème, ni le format du fichier (qui était bien en JPEG), et les droits d’accès au partage réseau pour le compte utilisateur concerné étaient les bons.
        ◦ Le problème résidait en réalité dans la suppression d’utilisateurs authentifiés au lieu de passer seulement le groupe en lecture seule. Une fois l’erreur rectifiée le fond d’écran apparaît.