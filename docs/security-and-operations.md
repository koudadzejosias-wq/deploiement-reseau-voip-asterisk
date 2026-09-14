# Sécurité et exploitation

## Secrets et certificats

- Remplacer chaque valeur `CHANGE_ME` avant la mise en service.
- Utiliser des mots de passe uniques et longs pour chaque extension.
- Conserver les certificats et clés privées dans `/etc/asterisk/keys/`, avec des permissions strictes.
- Ne jamais publier de clé privée, export pfSense, capture réseau ou sauvegarde contenant des données réelles.
- Renouveler les certificats TLS avant leur date d'expiration.

## Durcissement Asterisk

- Désactiver les comptes et modules inutiles.
- Limiter l'accès SSH au VLAN 30 et utiliser des clés plutôt qu'un mot de passe.
- Activer Fail2ban ou un mécanisme équivalent si un service est exposé.
- Refuser les appels anonymes et les destinations internationales non nécessaires.
- Vérifier régulièrement `pjsip show endpoints`, les journaux et les tentatives d'authentification.

## Sauvegardes

Sauvegarder séparément :

- `/etc/asterisk/` après validation d'une configuration ;
- les messages vocaux si leur conservation est nécessaire ;
- la configuration pfSense ;
- les certificats, dans un coffre sécurisé ;
- les journaux utiles à l'audit.

Tester régulièrement la restauration sur une machine isolée. Une sauvegarde non
restaurée au moins une fois n'est pas considérée comme validée.

## Contrôles périodiques

| Fréquence | Contrôle |
| --- | --- |
| À chaque changement | Appel interne, messagerie, conférence et validation de configuration |
| Hebdomadaire | État du service, stockage, journaux et échecs d'authentification |
| Mensuelle | Mise à jour de sécurité, test de restauration et contrôle des règles pfSense |
| Avant expiration | Certificats TLS, comptes utilisateurs et documentation |

## Commandes utiles

```bash
sudo systemctl status asterisk
sudo journalctl -u asterisk --since today
sudo asterisk -rx 'core show channels'
sudo asterisk -rx 'pjsip show endpoints'
```

Les sorties de commandes et les captures de recette doivent être anonymisées avant
un partage ou une publication.
