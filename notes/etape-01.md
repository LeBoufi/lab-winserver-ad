# **Partie 1**

_18/03/2026_

Installation et configuration de VirtualBox, téléchargement et lancement d'une Machine Virtuelle (VM) avec l'OS Windows Server 2025.


## **Environnement**

|                   |                                                                  |
| ----------------- | ---------------------------------------------------------------- |
| **Élément**       | **Détail**                                                       |
| Hyperviseur       | VirtualBox 7.0.22                                                |
| OS invité         | Windows Server 2025 Standard Evaluation (Desktop Experience) x64 |
| RAM allouée       | 4096 Mo                                                          |
| Disque virtuel    | 50 Go, allocation dynamique                                      |
| Contrôleur disque | AHCI                                                             |
| Réseau            | Réseau interne (Lab)                                             |


## **Ce que j'ai fait**

Après installation de VirtualBox et création de la VM avec les paramètres ci-dessus, j'ai monté l'ISO Windows Server 2025 sur le lecteur optique virtuel et procédé à l'installation de l'OS en sélectionnant la version Desktop Experience. Une fois l'installation terminée, j'ai installé les Guest Additions puis effectué un snapshot de l'état propre.


## **Problèmes rencontrés**

- Absence de la possibilité de passer la configuration réseau de la VM du NAT vers un réseau interne ; rétrogradation vers une version antérieure de VirtualBox (de 7.2.6 vers 7.0.22).

- Problème lors de l'installation de l'OS dans la VM : un lecteur de disquette gênait l'installation ; suppression du contrôleur et rectification de l'ordre de démarrage.

- Gel de la VM en cours d'installation, entraînant une perte de progression après redémarrage. Résolution : redémarrage forcé de la VM, démontage de l'ISO une fois les fichiers copiés, ce qui a permis de booter directement sur le disque SATA virtuel pour finaliser l'installation.


## **Concepts et apprentissages**

Prise en main d'un hyperviseur de type 2 (VirtualBox) et compréhension de son intérêt en contexte professionnel. Montage et configuration d'une première VM, avec résolution autonome de plusieurs problèmes courants. Acquisition du réflexe de snapshot dès qu'un état stable est atteint.