# 🖥️ Active Directory Lab – Déploiement d’une infrastructure Windows Server

## Description :

Ce projet documente la mise en place complète d’une infrastructure Active Directory dans un environnement virtualisé.  
L’objectif est de simuler un réseau d’entreprise fonctionnel comprenant :

- Un contrôleur de domaine (Domain Controller)
- Un service DNS et DHCP
- Une machine cliente intégrée au domaine
- Une structuration Active Directory (OU, utilisateurs, groupes)
- Des stratégies de groupe (GPO)

## Environnement technique :

| Élément            | Détail |
|--------------------|--------|
| Hyperviseur        | VirtualBox 7.0.22 |
| Serveur            | Windows Server 2025 Standard (Desktop Experience) |
| Client             | Windows 10 Entreprise Evaluation |
| RAM serveur        | 4 Go |
| RAM client         | 2 Go |
| Réseau             | Réseau interne (Lab) |

## Architecture :

- **Nom du domaine** : `monlab.local`
- **Contrôleur de domaine** : `SRV-DC01`
- **IP serveur** : `192.168.1.1`
- **Services installés** :
  - Active Directory Domain Services (AD DS)
  - DNS
  - DHCP

## Étapes du projet :

### 1. Virtualisation et installation du serveur

- Création d’une VM sous VirtualBox
- Installation de Windows Server 2025
- Installation des Guest Additions
- Création d’un snapshot initial

**Compétences acquises :**
- Prise en main d’un hyperviseur
- Gestion des problèmes d’installation
- Bonnes pratiques (snapshots)

---

### 2. Configuration du serveur

- Attribution d’une IP fixe
- Renommage en `SRV-DC01`
- Installation du rôle **AD DS**

**Concepts clés :**
- Importance d’une IP fixe
- Rôle d’un serveur dans un réseau
- Introduction à Active Directory

---

### 3. Promotion en contrôleur de domaine

- Création d’une forêt et d’un domaine :
  - `monlab.local`
- Activation DNS automatique

**Concepts clés :**
- Domaine vs forêt
- SYSVOL
- Niveau fonctionnel
- Rôle du contrôleur de domaine

---

### 4. Mise en place du DHCP

- Création d’une étendue :
  - `192.168.1.100 → 192.168.1.200`
- Durée du bail : 8 jours
- Configuration DNS et passerelle

**Concepts clés :**
- Attribution dynamique d’IP
- Autorisation DHCP dans AD
- Gestion des plages d’adresses

---

### 5. Machine cliente et jonction au domaine

- Création d’une VM Windows 10
- Attribution automatique d’IP via DHCP
- Tests réseau :
  - `ping`
  - `ipconfig`
  - `nslookup`
- Jonction au domaine `monlab.local`

**Concepts clés :**
- Fonctionnement DNS dans un domaine
- Authentification Kerberos
- Différence Workgroup / Domaine

---

### 6. Structuration Active Directory

#### Utilisateurs créés

- j.dupont
- s.martin
- p.bernard
- m.leroy
- p.durand
- c.petit

#### Groupes

- GRP-Informatique
- GRP-Comptabilité
- GRP-Direction

#### Organisation

- OU Utilisateurs
- OU Groupes
- OU Ordinateurs

**Concepts clés :**
- OU vs conteneurs
- Groupes de sécurité
- Délégation de contrôle

---

### 7. Stratégies de groupe (GPO)

#### GPO 1 – Fond d’écran imposé

- Chemin : `\\SRV-DC01\FondEcran\image.jpg`

#### GPO 2 – Restriction du panneau de configuration

- Blocage des paramètres système
- Test de filtrage par groupe

#### GPO 3 – Lecteur réseau

- Lecteur mappé : `P:`
- Partage : `\\SRV-DC01\Partage-Entreprise`

**Concepts clés :**
- GPO utilisateur vs ordinateur
- Héritage des GPO
- Filtrage de sécurité
- Différence stratégie / préférence

---

## Commandes utiles

ipconfig /all
ping 192.168.1.1
nslookup monlab.local
gpupdate /force
gpresult /r
net share
net use