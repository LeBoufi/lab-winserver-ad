# **Partie 10**

_23/03/2026_

Troisième GPO : mappage d'un lecteur réseau partagé.


## **Configuration du partage**

|                           |                                |
| ------------------------- | ------------------------------ |
| **Élément**               | **Détail**                     |
| Nom du dossier local      | Partage-Entreprise             |
| Chemin local sur SRV-DC01 | C:\Partage-Entreprise          |
| Nom du partage réseau     | Partage-Entreprise             |
| Chemin UNC du partage     | \\\SRV-DC01\Partage-Entreprise |
| Lettre de lecteur mappée  | P:                             |
| Libellé du lecteur        | Partage Entreprise             |


## **Permissions configurées**

|                         |                          |                     |
| ----------------------- | ------------------------ | ------------------- |
| **Groupe**              | **Permission partage**   | **Permission NTFS** |
| Tout le monde           | Lecture                  |                     |
| Utilisateurs du domaine | Modifier                 | Modification        |
| Administrateurs         | Contrôle total           | Contrôle total      |
| System                  | (accès local uniquement) | Contrôle total      |


## **GPO créée**

|                            |                                                                                     |
| -------------------------- | ----------------------------------------------------------------------------------- |
| **Élément**                | **Détail**                                                                          |
| Nom de la GPO              | GPO - Lecteur réseau Partage-Entreprise                                             |
| Liée à quelle OU           | OU - Utilisateurs                                                                   |
| Emplacement dans l'éditeur | Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteurs |
| Paramètre configuré        | Lecteur mappé                                                                       |
| Type de configuration      | Préférence                                                                          |
| Action                     | Créer                                                                               |
| Reconnecter                | Coché                                                                               |


## **Ce que j'ai fait**

J'ai commencé cette étape par la création du dossier C:\Partage-Entreprise et l'ai configuré dans ses propriétés de partage avancées selon les paramètres ci-dessus. J'ai ensuite vérifié l'accès à ce dossier depuis le poste client via la session de j.dupont en saisissant le chemin \\\192.168.1.1\Partage-Entreprise dans l'Explorateur de fichiers, ce qui était possible ; j'ai même pu créer un fichier texte dans le dossier.

Je me suis ensuite attelé à la création de la GPO selon les paramètres ci-dessus. Celle-ci permettra de mapper automatiquement sur les sessions clientes le dossier de partage commun à l'entreprise. J'ai testé ce mappage automatique en ouvrant successivement les sessions de j.dupont, p.bernard et c.petit. Pour chacune, la commande gpresult /r confirme la bonne application des GPO.


## **Concepts et apprentissages**

Contrairement aux permissions de partage, les permissions NTFS permettent de gérer l'accès aux fichiers sur des disques Windows, que ce soit en accès local ou réseau. En cas de conflit entre les deux types de permissions, la plus restrictive s'applique.

Alors que les paramètres GPO ne peuvent pas être modifiés par les utilisateurs et s'appliquent de façon forcée, les préférences GPO sont établies par défaut mais peuvent être modifiées par l'utilisateur. Cela permet d'établir une configuration par défaut idéale tout en laissant à l'utilisateur le choix final qui lui convient le mieux.

Le chemin UNC utilise le nom DNS de la ressource plutôt que son adresse IP, ce qui le rend plus lisible et plus facile à mémoriser, d'autant plus dans une structure informatique pouvant disposer de dizaines de serveurs. Cela facilite également le dépannage à distance pour des utilisateurs non techniques, sans qu'un accès physique à la machine soit nécessaire.

L'action 'Mettre à jour' est préférable à 'Créer' lorsque le paramètre ciblé est déjà géré par une autre GPO, ou lorsque son application doit être conditionnée à l'activation d'une autre stratégie.


## **Commandes et outils utilisés**

|                 |                                                                                                                                        |                                                                         |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Commande**    | **Utilité**                                                                                                                            | **Résultat observé**                                                    |
| gpupdate /force | Permet de forcer la mise à jour des GPO s'appliquant à l'utilisateur ou à l'ordinateur.                                                | GPO mises à jour.                                                       |
| gpresult /r     | Permet d'afficher quelles GPO sont actives pour cet utilisateur sur ce poste, et à quels groupes de sécurité l'utilisateur appartient. | GPO FondEcran et GPO Restriction Panneau de Configuration bien actives. |
| net share       | Liste les partages actifs sur le serveur.                                                                                              | Affiche les partages SYSVOL, SCRIPTS, FondEcran et Partage-Entreprise.  |
| net use         | Liste les lecteurs réseau mappés sur le client.                                                                                        | Affiche le lecteur P: et son chemin UNC.                                |
