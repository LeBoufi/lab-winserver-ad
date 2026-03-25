# **Partie 6**

_21/03/2026_

Structuration de l'Active Directory.


## **Environnement**

Voir schéma de l'arborescence AD ci-joint dans le document original.


## **Utilisateurs créés**

|                  |                 |                   |                  |
| ---------------- | --------------- | ----------------- | ---------------- |
| **Identifiants** | **Nom complet** | **OU**            | **Groupes**      |
| j.dupont         | Jean Dupont     | OU - Informatique | GRP-Informatique |
| s.martin         | Sophie Martin   | OU - Informatique | GRP-Informatique |
| p.bernard        | Pierre Bernard  | OU - Comptabilité | GRP-Comptabilité |
| m.leroy          | Marie Leroy     | OU - Comptabilité | GRP-Comptabilité |
| p.durand         | Paul Durand     | OU - Direction    | GRP-Direction    |
| c.petit          | Claire Petit    | OU - Direction    | GRP-Direction    |


## **Groupes créés**

|                   |                    |             |                      |
| ----------------- | ------------------ | ----------- | -------------------- |
| **Nom du groupe** | **Type**           | **Étendue** | **Membres**          |
| GRP-Informatique  | Groupe de sécurité | Global      | j.dupont et s.martin |
| GRP-Comptabilité  | Groupe de sécurité | Global      | m.leroy et p.bernard |
| GRP-Direction     | Groupe de sécurité | Global      | c.petit et p.durand  |


## **Ce que j'ai fait**

Après avoir exploré la structure par défaut, je me suis mis à la création des Unités d'Organisation dans le but de simuler une structure professionnelle. J'en ai profité pour créer 6 utilisateurs et 3 groupes, le tout selon les schémas et tableaux ci-dessus. J'ai ensuite ajouté les membres des différentes OU Utilisateurs aux groupes correspondants. J'ai déplacé WIN10-CLIENT dans l'OU - Postes Clients à l'intérieur de l'OU - Ordinateurs et vérifié la bonne création et attribution des utilisateurs en me connectant avec leur session sur le poste client. Une fois sur la session, j'ai confirmé l'ensemble via les commandes whoami, qui retourne l'identité du compte actif (monlab\j.dupont), et echo %logonserver%, qui retourne \\\SRV-DC01, confirmant la bonne authentification de la connexion par le DC.

J'ai fini par un snapshot de l'état.


## **Concepts et apprentissages**

J'ai compris la différence entre conteneurs et unités d'organisation. Les conteneurs sont des dossiers créés par défaut qui ne peuvent pas être supprimés par les administrateurs. Les unités d'organisation sont entièrement configurables par les administrateurs, bien plus malléables que les conteneurs et donc bien plus appropriées à l'application de GPO, rendant leur ciblage plus précis. Il faut donc impérativement transférer dans des OU les objets AD que l'on souhaite affecter par des GPO.

Les OU servent à organiser les objets AD et à les compartimenter précisément pour appliquer les GPO le plus justement possible. Celles de ce projet sont organisées en 3 OU mères avec des branches : l'OU - Utilisateurs, regroupant les utilisateurs dans des OU selon leurs départements ; l'OU - Groupes, contenant les groupes correspondants aux départements ; et l'OU - Ordinateurs, scindée en 2 OU selon qu'il s'agit d'un serveur ou d'un poste client. Cette organisation permet une application de GPO plus ou moins précise, et la présence des groupes en complément des OU permet de filtrer efficacement l'application de GPO plus générales.

Les groupes de sécurité permettent de gérer les permissions et les politiques de sécurité selon ce à quoi peut accéder quel utilisateur et ce qu'il peut faire. Les groupes de distribution ne servent qu'à la communication, notamment dans les listes de distribution.

L'étendue d'un groupe permet de réguler les ressources accessibles et les utilisateurs pouvant l'utiliser : soit seulement le domaine (Domaine Local), soit le domaine et les domaines disposant d'un canal sécurisé (Global), soit l'intégralité de la forêt (Universelle).

La délégation de contrôle Active Directory consiste à accorder à un groupe d'utilisateurs des droits limités sur une OU : modification, peuplement, liaison de GPO. Cela présente un intérêt dans les entreprises de taille importante, où déléguer certaines tâches de gestion permet de gagner en réactivité sans solliciter constamment le service informatique.


## **Commandes et outils utilisés**

|                    |                                                              |                      |
| ------------------ | ------------------------------------------------------------ | -------------------- |
| **Commande**       | **Utilité**                                                  | **Résultat observé** |
| whoami             | Permet d'identifier quel compte est connecté.                | monlab\j.dupont      |
| echo %logonserver% | Permet d'identifier quel serveur a authentifié la connexion. | \\\SRV-DC01          |