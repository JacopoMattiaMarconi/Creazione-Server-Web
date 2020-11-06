# Creazione-Server-Web
## apache2

1. installare apache2 + openssh-server
>sudo apt update
>
>sudo apt-get install apache2
>
>sudo apt install openssh-server

---------------------------------------------------------------------

2. controllare che vengano gestiti i file dentro determinate cartelle
>cd /etc/apache2/apache2.conf
>
--> <Directory /var/www/> ... </Directory>

---------------------------------------------------------------------

3. modificare impostazioni di rete
>cd /etc/netplan/00-installer-config.yaml
>
--> mettere la scheda di rete in bridge
>
> 'network:'
>
> 'version: 2'
>
> 'renderer: networkd'
>
> 'ethernets:'
>
> '  enp0s3:'
>
> '    dhcp4: yes'
>
>sudo netpaln try
>
>ip address

---------------------------------------------------------------------

>sudo apt-get install openssh-server

per installare pacchetto ssh per connessione remota


>sudo apt install net-tools
>
>cd /etc/apache2/sites-available


4. SITO A

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
>cd /etc/var/
>
>nano SitoA --> html

5. SITO B

>sudo cp 000-deafult.conf 002-deafult.conf
>
>sudo nano 002-deafult.conf
>
>DocumentRoot /var/www/SitoA
>
>systemctl reload apache2
>
>sudo a2ensite 002-deafult.conf


6.SITO C

>sudo cp 000-deafult.conf 003-deafult.conf
>
>sudo nano 003-deafult.conf
>
>DocumentRoot /var/www/SitoC
>
>systemctl reload apache2
>
>sudo a2ensite 003-deafult.conf
