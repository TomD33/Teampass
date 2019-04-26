Pré requis:
Apache v2.0 ou supérieur
MySQL v5.1 ou supérieur
PHP v5.5.0 ou supérieur (nous php 5.6)
Machine Redhat/CentOs


PHP module à avoir:

mcrypt
mbstring
openssl
gd
bcmath
iconv
xml
LDAP Si on utilise LDAP identification


Installation de Php 5.6

$ yum install php (de ton choix) mais pas en dessous de 5.5.0 
$ vim /etc/httpd/conf.d/httpd.conf

Dans ce fichier qui est le php.ini (httpd.conf) on configure le mbstring

[mbstring] 
mbstring.language = all 
mbstring.internal_encoding = UTF-8 
mbstring.http_input = auto 
mbstring.http_output = UTF-8 
mbstring.encoding_translation = On 
mbstring.detect_order = UTF-8 
mbstring.substitute_character = none; 
mbstring.func_overload = 0 
mbstring.strict_encoding = Off

Après l'avoir modifier et enregistrer on redemarre apache.

$ systemctl restart httpd


Obtenir les extensions PHP conforment pour le Projet Teampass

Installation mysqli :

$ yum install mysqli

Installation mcrypt :

$ yum install libmcrypt

Installation bcmath :

$ yum install php-bcmath

Installation gd : 
 $ yum install gd

Petite astuce : si on voit qu'il ne veut pas le prendre on force les paquets d'extentions

Exemple : yum install gd --skip-broken

Pour rattacher Teampass au ldap installer le paquet php-ldap

Une fois tous les paquets installés redémarrer la VM.

$ reboot


Un autre pré-requis de Teampass, ajuster une valeur dans le fichier php.ini (httpd.conf) pour éviter l’erreur
PHP “Maximum execution time” is set to 120 seconds. 

Il faut donc alors éditer le fichier php.ini et modifier la valeur

$ vi /etc/httpd/conf/httpd.conf
max_execution_time = 120


Pour corriger une autre erreur au lancement d’Apache, il faut déterminer un nom ou une ip pour le serveur.

Faut creer un ficher dans conf.d

$ vi /etc/httpd/conf.d/teampass.conf
<VirtualHost *:443>
DocumentRoot "/opt/teampass/"
ServerName teampass.domaine.tld
ServerAdmin mail@domaine.tld
</VirtualHost>


Démarrer le serveur apache et mettre le service en auto

$ systemctl start httpd
$ systemctl enable httpd


Démarrer le serveur MariaDB et mettre le service en auto

$ systemctl start mariadb
$ systemctl enable mariadb
Configuration et création de la base/user Mysql

changer le mots de passe root

$ /usr/bin/mysqladmin -u root password 'new-password'


Configuration alternative

$ /usr/bin/mysql_secure_installation


Créer base et user mariadb.

$ mysql -u root -p
CREATE DATABASE teampass_db COLLATE UTF8_general_ci;

CREATE USER teampass_admin identified by 'mot_de_passe';

GRANT ALL PRIVILEGES ON teampass_db.* to teampass_admin@localhost identified by 'mot_de_passe';

Pour le téléchargement de la source 

On fait un gitclone du fichier avec CMD/Powershell pour Windows ou bien Linux Terminal.
Pour récuperer le git clone prenez le lien en bas çi-dessus:
$ git clone https://github.com/nilsteampassnet/TeamPass/archive/master.zip

dézipper

$ unzip Teampass.master.zip

On pousse le fichier sur le Serveur Virtuel avec Git ou Winscp ou dossier partager ou un cloud ou si vous etes un galerian par email #lol.

On le renomme en "teampass" (pas obligé on peut mettre n'importe quel nom) & Ensuite on le déplace 

$ mv teampass/ /var/www/html/


On modifie les droits

$ chown -R apache:apache /var/www/html/teampass
$ chmod 755 /var/www/html/teampass

Ensuite on entre le lien de teampass que vous avez mis soit une IP soit un .conf (nom)
Après vous allez arriver a une page de finiliasation pour votre Teampass.






