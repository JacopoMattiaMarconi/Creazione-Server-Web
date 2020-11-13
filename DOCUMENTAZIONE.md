# Creazione-Server-Web
## apache2

1. installare apache2 + openssh-server
>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server

---------------------------------------------------------------------

2. controllare che vengano gestiti i file dentro determinate cartelle <Directory /var/www/> ... </Directory>
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
>sudo cp 000-deafult.conf 001-deafult.conf
>
>sudo nano 001-deafult.conf
>
>DocumentRoot /var/www/SitoA
>
>systemctl reload apache2
>
>sudo a2ensite 001-deafult.conf
>
>cd /var/www/
>
>sudo mkdir SitoA
>
>cd /var/www/SitoA

5. SITO B
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 002-deafult.conf
>
>sudo nano 002-deafult.conf
>
>DocumentRoot /var/www/SitoB
>
>systemctl reload apache2
>
>sudo a2ensite 002-deafult.conf
>
>cd /var/www/
>
>sudo mkdir SitoB
>
>cd /var/www/SitoB

6. SITO C
>cd /etc/apache2/sites-available
>
>sudo cp 000-deafult.conf 003-deafult.conf
>
>sudo nano 003-deafult.conf
>
>DocumentRoot /var/www/SitoC
>
>systemctl reload apache2
>
>sudo a2ensite 003-deafult.conf
>
>cd /var/www/
>
>sudo mkdir SitoC
>
>cd /var/www/SitoC

--------------------------------------------------------------------

7. Windows
>System32\drivers\etc\hosts
>
>#192.168.1.28  localhost
