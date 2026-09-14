# Procédure de déploiement

## Pré-requis

- Ubuntu Server 22.04 LTS à jour.
- Adresse IP fixe dans `192.168.10.0/24`.
- DNS, passerelle et règles pfSense déjà définis.
- Certificat TLS et secrets fournis hors du dépôt.

## Dépendances

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential linux-headers-$(uname -r) git curl autoconf gcc g++ \
  libxml2-dev libxml2-utils libcurl4-openssl-dev libeditline-dev libsqlite3-dev libc-dev \
  libssl-dev openssl ncurses-dev libopus-dev libvpx-dev libsrtp2-dev
```

## Compilation d'Asterisk 18

```bash
wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18-current.tar.gz
tar xf asterisk-18-current.tar.gz
cd asterisk-18.*
sudo contrib/scripts/get_mp3_source.sh
sudo contrib/scripts/install_prereq install
./configure --with-jansson-bundled
make menuselect
make -j"$(nproc)"
sudo make install
sudo make samples
sudo make config
sudo ldconfig
sudo systemctl enable --now asterisk
```

## Installation de la configuration

Valider chaque fichier avant copie, remplacer les valeurs `CHANGE_ME`, puis appliquer les permissions adaptées :

```bash
cp asterisk/config/pjsip.conf.example /tmp/pjsip.conf
cp asterisk/config/voicemail.conf.example /tmp/voicemail.conf
# Edit the copies, then install the validated production files.
sudo install -o root -g asterisk -m 0640 asterisk/config/extensions.conf /etc/asterisk/extensions.conf
sudo install -o root -g asterisk -m 0640 /tmp/pjsip.conf /etc/asterisk/pjsip.conf
sudo install -o root -g asterisk -m 0640 /tmp/voicemail.conf /etc/asterisk/voicemail.conf
sudo asterisk -rx 'core reload'
sudo asterisk -rx 'pjsip reload'
```

Les fichiers `.example` servent de référence et ne doivent pas être copiés tels quels en production.
