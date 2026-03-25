# **Partie 9**

_22/03/2026_

Deuxième GPO : restriction du panneau de configuration.


## **GPO créée**

|                            |                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| **Élément**                | **Détail**                                                                                   |
| Nom de la GPO              | GPO - Restriction Panneau de Configuration                                                   |
| Liée à quelle OU           | OU - Utilisateurs                                                                            |
| Paramètre configuré        | Interdire l'accès au Panneau de configuration et à l'application Paramètres du PC            |
| Emplacement dans l'éditeur | Configuration utilisateur > Stratégies > Modèles d'administration > Panneau de configuration |
| Filtrage de sécurité       | Test en inclusion unique du GRP-Comptabilité                                                 |


## **Ce que j'ai fait**

J'ai créé une nouvelle GPO dans le gestionnaire de stratégies de groupe, l'ai nommée GPO - Restriction Panneau de Configuration et configurée avec les paramètres ci-dessus. J'ai ensuite testé la restriction sur la machine cliente via le compte de j.dupont : après exécution d'un gpupdate /force et reconnexion, il m'est impossible d'ouvrir le panneau de configuration ou les paramètres de l'ordinateur ; la restriction est donc effective. J'ai ensuite vérifié sur le même poste avec le compte administrateur que la restriction ne l'affecte pas, ce qui est bien le cas.

J'ai tenté par la suite de circonscrire la restriction aux seuls comptes appartenant au groupe GRP-Comptabilité, mais j'ai fait une erreur. En ajoutant GRP-Comptabilité au filtrage de sécurité, j'ai supprimé le groupe Utilisateurs authentifiés du filtrage, ce qui a eu pour effet que la GPO ne s'appliquait plus du tout, même après un gpupdate /force et un redémarrage. Après avoir identifié le problème et rétabli Utilisateurs authentifiés dans le filtrage de sécurité en décochant uniquement le paramètre 'Appliquer la stratégie de groupe' pour ce groupe, la restriction ne s'applique plus qu'aux seuls comptes appartenant au groupe GRP-Comptabilité.

J'ai vérifié une dernière fois avec le compte j.dupont, sur lequel il m'est possible d'ouvrir les paramètres et le panneau de configuration. J'ai ensuite rétabli l'application de la restriction à l'ensemble des utilisateurs authentifiés.

Depuis le compte de j.dupont, j'ai exécuté la commande gpresult /r afin d'obtenir un résumé des GPO effectives sur la session. J'ai également utilisé la commande gpresult /h %userprofile%\Desktop\rapport-gpo.html pour exporter ce rapport au format HTML sur le bureau de la session.


## **Concepts et apprentissages**

J'ai choisi d'utiliser une GPO de configuration utilisateur plutôt qu'une GPO ordinateur afin de ne pas affecter le compte administrateur s'il y a nécessité d'ouvrir une session sur le poste pour des raisons de maintenance.

Une GPO appliquée à une OU s'applique automatiquement aux sous-OU de cette OU : c'est ce que l'on appelle l'héritage des GPO. Cela permet une stratification des GPO, avec les plus générales au niveau le plus large et des GPO de plus en plus précises à mesure que les OU sont réduites.

Le filtrage de sécurité permet une précision encore plus grande en ciblant une GPO directement sur un groupe précis. L'héritage peut être bloqué pour permettre des exceptions dans une sous-OU afin de l'exempter des GPO des strates supérieures.


## **Commandes et outils utilisés**

|                                                    |                                                                                                                                        |                                                                         |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Commande**                                       | **Utilité**                                                                                                                            | **Résultat observé**                                                    |
| gpupdate /force                                    | Permet de forcer la mise à jour des GPO s'appliquant à l'utilisateur ou à l'ordinateur.                                                | GPO mises à jour.                                                       |
| gpresult /r                                        | Permet d'afficher quelles GPO sont actives pour cet utilisateur sur ce poste, et à quels groupes de sécurité l'utilisateur appartient. | GPO FondEcran et GPO Restriction Panneau de Configuration bien actives. |
| gpresult /h %userprofile%\Desktop\rapport-gpo.html | Permet d'exporter le rapport GPO en un fichier HTML sur le bureau de la session.                                                       | Le fichier HTML est bien exporté sur le bureau.                         |
