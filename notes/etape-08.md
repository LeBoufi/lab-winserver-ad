# **Partie 8**

_22/03/2026_

Première GPO : fond d'écran imposé.


## **GPO créée**

|                     |                                                                                                     |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| **Élément**         | **Détail**                                                                                          |
| Nom de la GPO       | GPO - FondEcran                                                                                     |
| Liée à quelle OU    | OU - Utilisateurs                                                                                   |
| Paramètre configuré | Configuration utilisateur > Stratégies > Modèles d'administration > Bureau > Papier peint du bureau |
| Chemin de l'image   | \\\SRV-DC01\FondEcran\image.jpg                                                                     |
| Format de l'image   | .jpg                                                                                                |


## **Ce que j'ai fait**

J'ai commencé par créer un dossier C:\Partages\FondEcran et l'ai partagé à tout le domaine via les paramètres de partage avancés. J'ai ensuite inséré dans ce dossier une image qui servira de fond d'écran au labo virtuel, et l'ai renommée image.jpg. J'ai ensuite ouvert l'outil de Gestion des Stratégies de Groupe afin de créer ma première GPO selon les paramètres ci-dessus.

Afin de vérifier la bonne application de GPO - FondEcran, j'ai ouvert sur la VM cliente le compte de j.dupont et exécuté gpupdate /force, attendu la mise à jour des GPO, déconnecté puis reconnecté le compte afin de constater la bonne implémentation du fond d'écran. J'ai vérifié que l'utilisateur ne peut pas le modifier, et exécuté la commande gpresult /r dans le CMD pour y voir apparaître la GPO dans la liste des GPO s'appliquant à cet utilisateur sur cette machine.

J'ai fini par un snapshot du serveur à la fin des vérifications.


## **Concepts et apprentissages**

Une GPO permet l'application forcée d'un paramètre à un ensemble d'utilisateurs ou de postes. Ces paramètres permettent une uniformité de l'environnement informatique professionnel et donc une réduction des disparités entre les différents environnements de travail, amenant à une meilleure efficacité.

Une GPO liée à la configuration ordinateur s'applique aux paramètres de la machine indépendamment de l'utilisateur en session ; elle s'initialise au démarrage de l'ordinateur. Une GPO de configuration utilisateur affecte ce dont est capable l'utilisateur, quel que soit son poste de travail ; elle s'applique à l'ouverture de session, lors de l'authentification par le DC.

Le chemin UNC est un format similaire à l'URL mais sur un réseau local ; il permet une désignation simplifiée de l'emplacement des ressources partagées sur le réseau, sans avoir à noter spécifiquement l'IP de l'emplacement.

Créer une GPO permet de définir les paramètres qu'elle appliquera et dans quelle mesure ; la lier à une OU lui donne véritablement effet.


## **Commandes et outils utilisés**

|                 |                                                                                                                                        |                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| **Commande**    | **Utilité**                                                                                                                            | **Résultat observé**       |
| gpupdate /force | Permet de forcer la mise à jour des GPO s'appliquant à l'utilisateur ou à l'ordinateur.                                                | GPO mises à jour.          |
| gpresult /r     | Permet d'afficher quelles GPO sont actives pour cet utilisateur sur ce poste, et à quels groupes de sécurité l'utilisateur appartient. | GPO FondEcran bien active. |