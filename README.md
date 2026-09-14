# Déploiement Réseau VoIP Asterisk avec Vidéosurveillance IP et Portier Téléphonique

Projet PPE 300 : conception et déploiement d'une infrastructure réseau segmentée réunissant une téléphonie IP Asterisk, une vidéosurveillance IP et des portiers vidéo.

## Objectifs

- Déployer un IPBX Asterisk 18 LTS sur Ubuntu Server 22.04 LTS.
- Fournir les extensions Yealink et les softphones Zoiper.
- Intégrer la passerelle FXO GXW4104 et les portiers vidéo DS-KIS602.
- Segmenter les usages par VLAN et appliquer la QoS voix/vidéo.
- Documenter les tests de recette et les contrôles de sécurité.

## Architecture réseau

| VLAN | Réseau | Usage | QoS |
| --- | --- | --- | --- |
| 10 | `192.168.10.0/24` | VoIP, Asterisk, téléphones, softphones, passerelle FXO et portiers | DSCP EF |
| 20 | `192.168.20.0/24` | NVR et caméras IP | DSCP AF41 |
| 30 | `192.168.30.0/24` | Administration des équipements | Standard |
| 40 | `192.168.40.0/24` | Postes utilisateurs et données | Standard |

Équipements principaux : Netgate 2100 sous pfSense, switch Cisco SG250-26HP, serveur Dell PowerEdge R250 et NVR Hikvision.

## Plan de numérotation

| Extensions | Usage |
| --- | --- |
| `1000-1019` | Postes physiques Yealink |
| `2000-2009` | Softphones et télétravail |
| `8000` | IVR principal |
| `8081` | Test de synthèse vocale |
| `9000` / `9001` | Conférences utilisateurs / administration |
| `236` | Consultation de la messagerie |
| `*8` | Interception d'appel |

## Arborescence

```text
asterisk/
  config/
    pjsip.conf.example
    extensions.conf
    voicemail.conf.example
docs/
  deployment.md
  acceptance-tests.md
```

Les fichiers `*.example` sont des modèles. Les mots de passe, certificats TLS, adresses réelles et données de messagerie doivent être fournis dans un emplacement local non versionné.

## Déploiement rapide

1. Télécharger l'[ISO officielle Ubuntu Server 22.04.5 LTS](https://releases.ubuntu.com/22.04/) et l'installer sur le serveur ou dans une machine virtuelle. Attribuer ensuite une adresse fixe dans le VLAN 10.
2. Installer les dépendances décrites dans [docs/deployment.md](docs/deployment.md).
3. Compiler et installer Asterisk 18 LTS.
4. Copier les fichiers de configuration dans `/etc/asterisk/`, puis remplacer toutes les valeurs d'exemple.
5. Générer ou installer les certificats TLS et configurer le pare-feu pfSense.
6. Redémarrer Asterisk et exécuter la matrice de recette dans [docs/acceptance-tests.md](docs/acceptance-tests.md).

## Téléchargements officiels

- [Ubuntu Server 22.04.5 LTS](https://releases.ubuntu.com/22.04/) : image ISO du système serveur.
- [Asterisk 18 LTS](https://downloads.asterisk.org/pub/telephony/asterisk/) : sources officielles à compiler sur Ubuntu.

Le projet ne distribue pas d'image ISO Asterisk personnalisée. Les fichiers VMware présents sur la machine de développement sont exclus de GitHub, car ils sont volumineux et peuvent contenir l'état ou des données privées de la machine virtuelle.

## Sécurité

Ce dépôt ne doit contenir aucun mot de passe, certificat privé, sauvegarde de VM, capture réseau ou export de configuration pfSense. Les ports SIP/RTP doivent être limités aux réseaux nécessaires et l'administration doit rester sur le VLAN 30.

## Licence

À compléter selon les règles de diffusion du projet PPE 300.
