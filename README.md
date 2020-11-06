# Creazione-Server-Web
##apache2

sudo apt-get install apache2
per installare apache2

cd /etc/apache2adminuser ls -l
controllare che vengano gestiti i file dentro determinate cartelle
--> <Directory /var/www/> ... </Directory>

cd /etc/netplan/00-installer-config.yaml
modificare impostazioni di rete
--> mettere la scheda di rete in bridge
sudo netpaln try 
per salvare le impostazioni

sudo apt-get install openssh-server
per installare pacchetto ssh per connessione remota

sudo apt install net-tools

sudo nano /etc/apache2/sites-available/000-deafult.conf
cd /etc/apache2/sites-available
sudo cp 000-deafult.conf 001-deafult.conf
sudo nano 001-deafult.conf
DocumentRoot /var/www/SitoA

systemctl reload apache2
sudo a2ensite 001-deafult.conf
