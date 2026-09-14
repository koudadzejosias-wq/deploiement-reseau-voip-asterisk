# Intégration des équipements VoIP

## Passerelle FXO GXW4104

La passerelle doit être placée dans le VLAN 10 avec une adresse fixe ou un bail
DHCP réservé. Créer un trunk SIP entre la passerelle et Asterisk, puis définir
les ports FXO selon le plan d'appel réel.

Avant la mise en production :

1. Remplacer les identifiants SIP d'exemple par des secrets uniques.
2. Limiter l'adresse du serveur SIP à l'adresse Asterisk.
3. Configurer le codec et le mode DTMF de manière identique sur les deux appareils.
4. Tester un appel entrant, un appel sortant et la détection de raccrochage.
5. Interdire les destinations coûteuses qui ne sont pas nécessaires.

## Portiers vidéo DS-KIS602

Les portiers doivent être placés dans le VLAN 10 ou dans un VLAN dédié autorisé
par pfSense. Pour chaque portier, documenter :

- adresse IP et identifiant de l'équipement ;
- poste ou groupe appelé ;
- touche DTMF d'ouverture ;
- relais ou serrure commandée ;
- comportement en cas de perte du serveur Asterisk.

Le flux vidéo et le flux SIP doivent être testés séparément. Ne pas ouvrir
l'interface d'administration des portiers depuis Internet.

## Téléphones et softphones

Pour chaque poste, renseigner dans un inventaire privé :

| Champ | Exemple |
| --- | --- |
| Extension | `1000` |
| Modèle | Yealink SIP-T33G |
| Adresse MAC | À renseigner localement |
| VLAN | `10` |
| Utilisateur | À renseigner localement |
| Mot de passe SIP | À conserver hors Git |

Les fichiers PJSIP du dépôt contiennent seulement quelques extensions d'exemple.
Dupliquer le modèle avec un identifiant unique pour les autres postes et changer
chaque mot de passe avant l'installation.
