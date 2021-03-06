# Creazione Web Server Linux Ubuntu Server :computer:

## macchina virtualizzata, apache2

[INSTALLAZIONE PACCHETTI NECESSARI](#INSTALLAZIONE-PACCHETTI)<br>
[CONFIGURAZIONE DI RETE](#CONFIGURAZIONE-DI-RETE)<br>
[CREAZIONE UTENTI](#CREAZIONE-UTENTI)<br>
[CREAZIONE SPAZIO SITI](#CREAZIONE-SPAZIO-SITI)<br>
[CONTROLLO FILE APACHE2](#CONTROLLO-FILE-APACHE2)<br>
[CREAZIONE FILE DI CONFIGURAZIONE DEL SITO](#CREAZIONE-FILE-DI-CONFIGURAZIONE-DEL-SITO)<br>
[CREAZIONE SITO](#CREAZIONE-SITO)<br>
[INSTALLAZIONE SAMBA](#INSTALLAZIONE-SAMBA)<br>
[CONFIGURAZIONE FTP](#CONFIGURAZIONE-FTP)<br>
[OPZIONALE: DHCP4 + WINDOWS](#OPZIONALE)<br>

## PERSONALIZZAZIONE HOSTNAME
### tempo d'esecuzione: 5 min

>sudo hostnamectl set-hostname nuovohostname
>
>sudo reboot
>

oppure 

>sudo nano /etc/hostname
>
>sudo nano /etc/hosts
>
>sudo reboot
>

### checkpoint :white_check_mark: <br>
All'avvio l'hostname sarà cambiato

---------------------------------------------------------------------

## INSTALLAZIONE PACCHETTI
### tempo d'esecuzione: 5 min
:warning: pacchetto APACHE2 per scaricare server APACHE2 con FileZilla<br>
:warning: pacchetto OPENSSH-SERVER per comandi FTP con FileZilla<br>
:warning: pacchetto VSFTPD per comandi FTP con FileZilla<br>
:warning: pacchetto PHP per eseguire file php<br>
:warning: pacchetto driver sqlite con php<br>

>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server
>
>sudo apt-get install vsftpd
>
>sudo apt-get install php
>
>sudo apt-get install php[version]-sqlite3
>

### CHECKPOINT :white_check_mark: <br>
controllare connettività dopo aver installato APACHE2
>digitare ip su barra di ricerca nel browser
>

versione php
>php -v
>

### CHECKPOINT :white_check_mark: <br>
assegnare permessi scrittura sqlite per accesso web
>sudo chgrp -R www-data /var/www/Sitoa/cartellaapplicazioneweb 
>
>sudo chmod -R g+rw /var/www/Sitoa/cartellaapplicazioneweb 
>

---------------------------------------------------------------------

## CONFIGURAZIONE DI RETE
### tempo d'esecuzione: 10 min
:warning: in caso di virtualizzazione è necessario porre scheda in bridge

>cd /etc/netplan/00-installer-config.yaml

>
>     network:
>       ethernets:
>         enp0s3:
>           addresses: [172.16.29.113/16]
>           gateway4: 172.16.1.7
>           nameservers:
>             search: [virtual.marconi]
>             addresses: [172.16.1.18, 172.16.1.10] 
>       version: 2
>

Per accettare configurazione appena impostata: <br>
>sudo netplan try

>
>     [sudo] password for adminuser:
>     Warning: Stopping systemd-networkd.service, but it can still be activated by:
>       systemd-networkd.socket
>     Do you want to keep these settings?
>
>
>     Press ENTER before the timeout to accept the new configuration
>
>
>     Changes will revert in 119 seconds
>     Configuration accepted.
>

### CHECKPOINT :white_check_mark: <br>
controllare corretta configurazione di rete: <br>
>ip address
>

---------------------------------------------------------------------

## CREAZIONE UTENTI 
### tempo d'esecuzione: 5 min
>sudo useradd -s /bin/bash -d /var/www/SitoX -m usersitoX
>
>sudo passwd usersitoX
>

### CHECKPOINT :white_check_mark: <br>
controllare inserimento corretto per accesso unico alla cartella
>exit
>
>         accedere con user e passwd inserite
>

---------------------------------------------------------------------

## CREAZIONE SPAZIO SITI
### tempo d'esecuzione: 3 min
:warning: Lo spazio dei siti può essere creato dall'utente creato precedentemente da root
>sudo mkdir /var/www/ SitoA
>
>sudo mkdir /var/www/SitoA log
>
>sudo mkdir /var/www/SitoA web
>

--------------------------------------------------------------------

## CONTROLLO FILE APACHE2
### tempo d'esecuzione: 2 min
:warning: controllare che vengano gestiti i file dentro apache2

>cat cd /etc/apache2/apache2.conf
>

>
>       <>Directory /var/www/>
>            Options Indexes FollowSymLinks
>            AllowOverride None
>            Require all granted
>       </Directory>
>

---------------------------------------------------------------------

## CREAZIONE FILE DI CONFIGURAZIONE DEL SITO
### tempo d'esecuzione: 10 min
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 001-default.conf
>
>sudo nano 001-deafult.conf
>
>       DocumentRoot /var/www/SitoA
>
>       ServerName sitoa-113.virtual.marconi
>
>       ErrorLog /var/www/SitoA/log/error.log
>
>       CustomLog /var/www/SitoA/log/access.log combined
>

---------------------------------------------------------------------

## CREAZIONE SITO
### tempo d'esecuzione: 15 min

Dopo aver creato un file index.html nella cartella web dello spazio personale
e dopo aver creato la cartella log (var/www/Sito/web e var/www/Sito/log):
>systemctl reload apache2
>

### :white_check_mark: passaggi avvenuti con successo:
>         Authentication is required to reload 'apache2.service'.
>         Authenticating as: adminuser
>         Password:
>         ==== AUTHENTICATION COMPLETE ===
>

>sudo a2ensite 001-default.conf
>

>         Enabling site hm18858-default.
>         To activate the new configuration, you need to run:
>           systemctl reload apache2

### :warning: passaggi non avvenuti con successo

>         [sudo] password for adminuser:
>         Job for apache2.service failed.
>         See "systemctl status apache2.service" and "journalctl -xe" for details.
>

visonare utente che ha generato errore: <br>
>apache2ctl configtest
>

disattivare file dell'utente incriminato:
>sudo a2dissite <fileconf.conf>
>

>         Site hm18858-default disabled.
>         To activate the new configuration, you need to run:
>           systemctl reload apache2

### controllo finale sul raggiungimento del sito <br>
comandi utili:
>digitare sul browser servername
>
>apache2 is not active -> sudo systemctl enable apache2
>
>cat /var/log/syslog
>
>history | grep enable
>
>sudo systemctl restart apache2.service
>
>sudo systemctl reload apache2
>
  
--------------------------------------------------------------------

## INSTALLAZIONE SAMBA
### tempo d'esecuzione: 15 min
Un file server Samba consente la condivisione di file tra diversi sistemi operativi su una rete.<br>
Ti consente di accedere ai file del desktop da un laptop e di condividere file con utenti Windows e macOS.<br>
<bR>
Di cosa avremo bisogno:
- Ubuntu 16.04 LTS
- Una rete locale (LAN) su cui condividere file

>sudo apt update
>
>sudo apt install samba
>
>whereis samba
>

L'output dovrebbe essere il seguente: :white_check_mark:
>     samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
>

>mkdir /home/<username>/sambashare/
>
>sudo nano /etc/samba/smb.conf

File di configurazione: 
>     [sambashare]
>
>         comment = Samba on Ubuntu
>
>         path = /home/username/sambashare
>
>         read only = no
>
>         browsable = yes
>

>sudo service smbd restart
>
>sudo ufw allow samba
>
>sudo smbpasswd -a username
>

Su Ubuntu: apri il file manager predefinito e fai clic su Connetti al server, quindi inserisci:<br>
>smb://ip-address/sambshare
>

Su macOS: nel menu Finder, fai clic su Vai> Connetti al server, quindi inserisci:<br>
>smb://ip-address/sambshare
>

Su Windows, apri File Manager e modifica il percorso del file in:<br>
>\\ip-address\sambashare
>

------------------------------------------------------------

## CONFIGURAZIONE FTP
### tempo d'esecuzione: 10 min
:warning: configurare file vsftpd per usufruire dei comandi FTP da remoto<br>
>sudo nano /etc/vsftpd.conf
>

>
>     listen=yes
>
>     listen_ipv6=NO
>
>     anonymous_enable=NO
>
>     local_enable=YES
>
>     write_enable=YES
>
>     local_umask=022
>
>     dirmessage_enable=YES
>
>     use_localtime=YES
>
>     xferlog_enable=YES
>
>     connect_from_port_20=YES
>
>     xferlog_file=/var/log/vsftpd.log
>
>     xferlog_std_format=YES
>
>     ftpd_banner=Welcome to our  FTP service.
>
>     chroot_local_user=YES
>
>     local_root=/var/www/$USER
>
>     user_sub_token=$USER
>
>     allow_writeable_chroot=YES
>
>     secure_chroot_dir=/var/run/vsftpd/empty
>
>     pam_service_name=vsftpd
>
>     rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
>
>     rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
>
>     ssl_enable=NO
>
>     session_support=YES
>
>     log_ftp_protocol=YES
>

-------------------------------------------------------------

## OPZIONALE
### tempo d'esecuzione: 5 min
:warning: se si usa configurazione DHCP4 e si dispone di Windows su macchina fisica
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
