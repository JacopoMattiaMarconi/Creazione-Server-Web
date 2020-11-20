# Creazione-Server-Web su macchina virtuale con Linux Ubuntu Server :moon:
## apache2

1. INSTALLARE APACHE2, OPENSSH-SERVER, VSFTPD (per comandi FTP con FileZilla)
>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server
>
>sudo apt-get install vsftpd

---------------------------------------------------------------------

2. modificare impostazioni di rete, scheda di rete in bridge
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
>ip address

---------------------------------------------------------------------

3. CREARE UN UTENTE PER ACCESSO UNICO ALLA CARTELLA SitoA o SitoB o SitoC
>sudo useradd -s /bin/bash -d /var/www/SitoX -m usersitoX
>
>sudo passwd usersitoX
>
>sudo mkdir /var/www/ SitoA
>
>sudo mkdir /var/www/ SitoB
>
>sudo mkdir /var/www/ SitoC
>

--------------------------------------------------------------------

4. controllare che vengano gestiti i file dentro determinate cartelle <Directory /var/www/>
>cd /etc/apache2/apache2.conf
>

---------------------------------------------------------------------

5. SITO A
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
>sudo mkdir /var/www/SitoA log
>
>sudo mkdir /var/www/SitoA web
>
>sudo nano /var/www/SitoA/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 001-default.conf
>

6. SITO B
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
>sudo mkdir /var/www/SitoB log
>
>sudo mkdir /var/www/SitoB web
>
>sudo nano /var/www/SitoB/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 002-default.conf
>

7. SITO C
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
>sudo mkdir /var/www/SitoC log
>
>sudo mkdir /var/www/SitoC web
>
>sudo nano /var/www/SitoC/index.html
>
>sudo systemctl reload apache2
>
>sudo a2ensite 003-default.conf
>

--------------------------------------------------------------------

8. CONFIGURARE FILE VSFTP
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

9. Windows (SE SI USA CONFIGURAZIONE DHCP4)
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
