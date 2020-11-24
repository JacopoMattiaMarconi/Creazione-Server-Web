# Creazione-Server-Web su macchina virtuale con Linux Ubuntu Server :moon:

## apache2

[INSTALLAZIONE PACCHETTI NECESSARI](#INSTALLARE-APACHE2-+-OPENSSH-+-SERVER-VSFTPD)<br>
[CONFIGURAZIONE DI RETE](#CONFIGURAZIONE-DI-RETE)<br>
[CREAZIONE UTENTI](#CREAZIONE-UTENTI)<br>
[CREAZIONE SPAZIO SITI](4.-CREAZIONE-SPAZIO-SITI)<br>
[CONTROLLO FILE APACHE2](#5.-CONTROLLO-FILE-APACHE2)<br>
[CREAZIONE SITO A](#6.-CREAZIONE-SITO-A)<br>
[FTP: CONFIGURARE FILE VSFTP](#9.-FTP:-CONFIGURARE-FILE-VSFTP)<br>
[OPZIONALE: DHCP4 + WINDOWS](#10.-OPZ:-WINDOWS-(SE-SI-USA-CONFIGURAZIONE-DHCP4))<br>

## INSTALLARE APACHE2 + OPENSSH-SERVER + VSFTPD
:warning: pacchetto VSFTPD per comandi FTP con FileZilla

>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server
>
>sudo apt-get install vsftpd

---------------------------------------------------------------------
## CONFIGURAZIONE DI RETE<br>
:exclamation: In caso di virtualizzazione è necessario porre scheda in bridge

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

>sudo netpaln try

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

>ip address

---------------------------------------------------------------------

## CREAZIONE UTENTI
>sudo useradd -s /bin/bash -d /var/www/SitoX -m usersitoX
>
>sudo passwd usersitoX
>

---------------------------------------------------------------------

## CREAZIONE SPAZIO SITI<br>
:exclamation: Lo spazio dei siti può essere creato dall'utente creato precedentemente da root
>sudo mkdir /var/www/ SitoA
>
>sudo mkdir /var/www/SitoA log
>
>sudo mkdir /var/www/SitoA web
>

--------------------------------------------------------------------

## CONTROLLO FILE APACHE2<br>
:exclamation: controllare che vengano gestiti i file dentro determinate cartelle <Directory /var/www/>

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

## CREAZIONE SITO A
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
>sudo nano /var/www/SitoA/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 001-default.conf
>

### SITO B
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 002-default.conf
>
>sudo nano 002-deafult.conf
>
>       DocumentRoot /var/www/SitoB
>
>       ServerName sitob-113.virtual.marconi
>
>       ErrorLog /var/www/SitoB/log/error.log
>
>       CustomLog /var/www/SitoB/log/access.log combined 
>
>sudo nano /var/www/SitoB/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 002-default.conf
>

### SITO C
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 003-default.conf
>
>sudo nano 003-deafult.conf
>
>       DocumentRoot /var/www/SitoC
>
>       ServerName ubuntu-srv113.virtual.marconi
>
>       ErrorLog /var/www/SitoC/log/error.log
>
>       CustomLog /var/www/SitoC/log/access.log combined
>
>sudo nano /var/www/SitoC/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 003-default.conf
>

--------------------------------------------------------------------

## FTP: CONFIGURARE FILE VSFTP
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

## OPZ: WINDOWS (SE SI USA CONFIGURAZIONE DHCP4)
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
