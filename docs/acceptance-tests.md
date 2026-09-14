# Matrice de recette

| Code | Vérification | Commande ou méthode | Attendu |
| --- | --- | --- | --- |
| REC-001 | Système | `uname -a` | Ubuntu 22.04, noyau 5.15 ou supérieur |
| REC-002 | Service Asterisk | `asterisk -rx 'core show version'` | Asterisk 18 LTS actif |
| REC-003 | Endpoints SIP | `asterisk -rx 'pjsip show endpoints'` | Endpoints attendus visibles |
| REC-004 | Écoute SIP | `ss -lntup | grep -E '5060|5061'` | Ports autorisés en écoute |
| REC-005 | TLS | `openssl s_client -connect serveur:5061` | Certificat valide et protocole attendu |
| REC-104 | Appel audio | Appel 1000 vers 1001 | Audio bidirectionnel stable |
| REC-301 | Portier IP | Appui sur la platine DS-KIS602 | Sonnerie sur le poste configuré |
| REC-302 | Ouverture | Touche DTMF configurée | Déverrouillage après validation |
| REC-401 | VLAN | Tests depuis les VLAN 10, 20, 30 et 40 | Flux autorisés selon la politique pfSense |

Les résultats, adresses et captures de recette doivent rester dans un rapport séparé et ne pas exposer de secrets dans GitHub.
