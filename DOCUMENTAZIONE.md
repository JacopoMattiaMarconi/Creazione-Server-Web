# Creazione-Server-Web su macchina virtuale con Linux Ubuntu Server
## apache2

1. INSTALLARE APACHE2, OPENSSH-SERVER, VSTPD (per comandi FTP con FileZilla)
>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server
>
>sudo apt-get install vstpd

---------------------------------------------------------------------

2. controllare che vengano gestiti i file dentro determinate cartelle <Directory /var/www/>
>cd /etc/apache2/apache2.conf
>

---------------------------------------------------------------------

3. modificare impostazioni di rete, scheda di rete in bridge
>cd /etc/netplan/00-installer-config.yaml

>     network:
>
>       version: 2
>
>       renderer: networkd
>
>       ethernets:
>
>         enp0s3:
>
>           dhcp4: yes

>sudo netpaln try
>
>ip address

---------------------------------------------------------------------

4. SITO A
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 001-default.conf
>
>sudo nano 001-deafult.conf
>
>       DocumentRoot /var/www/SitoA/web
>
>       ServerName sitoa-101.virtual.marconi
>
>       ErrorLog /var/www/SitoA/log/error.log
>
>       CustomLog /var/www/SitoA/log/access.log combined
>
>systemctl reload apache2
>
>sudo a2ensite 001-default.conf
>
>cd /var/www/
>
>sudo mkdir SitoA
>
>cd /var/www/SitoA/web/
>
>sudo nano Index.html
>

5. SITO B
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 002-default.conf
>
>sudo nano 002-deafult.conf
>
>       DocumentRoot /var/www/SitoB/web
>
>       ServerName sitob-101.virtual.marconi
>
>       ErrorLog /var/www/SitoB/log/error.log
>
>       CustomLog /var/www/SitoB/log/access.log combined
>
>systemctl reload apache2
>
>sudo a2ensite 002-default.conf
>
>cd /var/www/
>
>sudo mkdir SitoB
>
>cd /var/www/SitoB/web/
>
>sudo nano Index.html
>

6. SITO C
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 003-default.conf
>
>sudo nano 003-deafult.conf
>
>       DocumentRoot /var/www/SitoC/web
>
>       ServerName srv-101.virtual.marconi
>
>       ErrorLog /var/www/SitoC/log/error.log
>
>       CustomLog /var/www/SitoC/log/access.log combined
>
>systemctl reload apache2
>
>sudo a2ensite 003-default.conf
>
>cd /var/www/
>
>sudo mkdir SitoC
>
>cd /var/www/SitoB/web/
>
>sudo nano Index.html
>
--------------------------------------------------------------------

7. Windows
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
