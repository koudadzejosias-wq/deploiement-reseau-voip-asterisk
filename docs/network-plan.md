# Plan réseau et pare-feu

Ce document fournit les paramètres à adapter dans pfSense, le switch Cisco et le serveur Asterisk. Les adresses réelles et les secrets doivent rester hors du dépôt.

## VLAN et adressage

| VLAN | Réseau | Passerelle indicative | Usage | Priorité |
| --- | --- | --- | --- | --- |
| 10 | `192.168.10.0/24` | `192.168.10.1` | Asterisk, téléphones, softphones, GXW4104 et portiers | DSCP EF |
| 20 | `192.168.20.0/24` | `192.168.20.1` | NVR et caméras | DSCP AF41 |
| 30 | `192.168.30.0/24` | `192.168.30.1` | Administration et supervision | Standard |
| 40 | `192.168.40.0/24` | `192.168.40.1` | Postes utilisateurs | Standard |

Réserver des adresses fixes ou des baux DHCP pour le serveur Asterisk, le NVR,
la passerelle FXO, les portiers et les équipements réseau.

## Ports à autoriser

| Source | Destination | Ports | Action |
| --- | --- | --- | --- |
| VLAN 10 | Asterisk | UDP 5060, UDP 10000-20000 | Autoriser SIP et RTP |
| VLAN 10 | Asterisk | TCP 5061 | Autoriser uniquement si TLS est activé |
| VLAN 30 | Asterisk | TCP 22, TCP 443 ou interface d'administration | Autoriser depuis les postes d'administration seulement |
| Asterisk | DNS/NTP | UDP 53, UDP 123 | Autoriser pour la résolution et l'heure |
| VLAN 40 | VLAN 10 | Selon besoin métier | Refuser par défaut |
| Internet | Asterisk | Aucun accès direct par défaut | Refuser |

Les plages RTP doivent être identiques dans Asterisk et dans les règles pfSense.
Ne pas exposer le port SIP sur Internet sans mécanisme de protection, limitation
par adresse, TLS et surveillance des tentatives.

## Switch et QoS

- Configurer le lien pfSense-switch en trunk 802.1Q avec les VLAN 10, 20, 30 et 40.
- Configurer les ports des téléphones et de la passerelle dans le VLAN 10.
- Configurer les ports caméras et NVR dans le VLAN 20.
- Réserver le VLAN 30 à l'administration.
- Activer la QoS voix avec DSCP EF et la vidéo avec DSCP AF41.
- Désactiver les ports inutilisés et utiliser un VLAN natif non utilisé si le matériel le permet.

## Vérifications réseau

```bash
ip addr
ip route
ping -c 3 192.168.10.1
sudo ss -lunp | grep -E '5060|10000'
```

Depuis chaque VLAN, vérifier uniquement les flux prévus et conserver les résultats
dans un rapport de recette séparé.
