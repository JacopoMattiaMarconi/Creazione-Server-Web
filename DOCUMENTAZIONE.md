# Creazione-Server-Web su macchina virtuale con Linux Ubuntu Server :moon:

## apache2

[INSTALLAZIONE PACCHETTI NECESSARI](#INSTALLAZIONE-PACCHETTI)<br>
[CONFIGURAZIONE DI RETE](#CONFIGURAZIONE-DI-RETE)<br>
[CREAZIONE UTENTI](#CREAZIONE-UTENTI)<br>
[CREAZIONE SPAZIO SITI](#CREAZIONE-SPAZIO-SITI)<br>
[CONTROLLO FILE APACHE2](#CONTROLLO-FILE-APACHE2)<br>
[CREAZIONE FILE DI CONFIGURAZIONE DEL SITO](#CREAZIONE-FILE-DI-CONFIGURAZIONE-DEL-SITO)<br>
[CREAZIONE SITO](#CREAZIONE-SITO)<br>
[CONFIGURAZIONE FTP](#CONFIGURAZIONE-FTP)<br>
[OPZIONALE: DHCP4 + WINDOWS](#OPZIONALE)<br>

## INSTALLAZIONE PACCHETTI
:warning: pacchetto APACHE2 per scaricare server APACHE2 con FileZilla<br>
:warning: pacchetto OPENSSH-SERVER per comandi FTP con FileZilla<br>
:warning: pacchetto VSFTPD per comandi FTP con FileZilla

>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server
>
>sudo apt-get install vsftpd

:white_check_mark: <br>
controllare connettività dopo aver installato APACHE2
>digitare ip su barra di ricerca nel browser
>

---------------------------------------------------------------------

## CONFIGURAZIONE DI RETE<br>
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

Per controllare corretta configurazione di rete: <br>
>ip address

:check:
---------------------------------------------------------------------

## CREAZIONE UTENTI
>sudo useradd -s /bin/bash -d /var/www/SitoX -m usersitoX
>
>sudo passwd usersitoX
>

---------------------------------------------------------------------

## CREAZIONE SPAZIO SITI<br>
:warning: Lo spazio dei siti può essere creato dall'utente creato precedentemente da root
>sudo mkdir /var/www/ SitoA
>
>sudo mkdir /var/www/SitoA log
>
>sudo mkdir /var/www/SitoA web
>

--------------------------------------------------------------------

## CONTROLLO FILE APACHE2<br>
:warning: controllare che vengano gestiti i file dentro determinate cartelle <Directory /var/www/>

>cd /etc/apache2/apache2.conf
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

### SITO A
>sudo nano /var/www/SitoA/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 001-default.conf
>

se comando a2ensite da errore di compilazione:<br>
>sudo a2dissite 001-default.conf
>

--------------------------------------------------------------------

## CONFIGURAZIONE FTP
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
:warning: se si usa configurazione DHCP4 e si dispone di Windows su macchina fisica
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
