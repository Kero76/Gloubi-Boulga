# VPS Installation Guide.
A guide to configure my VPS.

## First Connection (as root)
1. Update system : `$ apt-get update && apt-get upgrade`
2. Change root password : `$ passwd root`
3. Add new user : `adduser [username]`
4. Change default port for ssh connection : `$ nano /etc/ssh/sshd_config`
5. Restart ssh service : `$ /etc/init.d/ssh restart`
6. Exit connection.

## Install Apache2 server (as yourself)
1. Connect as root : `$ su`
2. Install apache2 : `$ apt-get install apache2`
3. Go on your web browser on the url of your server and check if apache work : _It Work!_ webpage
4. Install PHP7 and some other libraries : `$ apt-get install php7.0 libapache2-mod-php7.0 php7.0-mysql php7.0-curl php7.0-json php7.0-gd php7.0-mcrypt php7.0-intl php7.0-sqlite3 php7.0-gmp php7.0-mbstring php7.0-xml php7.0-zip`
5. Add the following code on _/var/www/html/info.php_ : `<?php phpinfo() ?>;`
6. Go on page `http://your_server_ip/info.php` and check the version of PHP.

## Install MySQL service (as yourself)
1. Connect as root : `$ su`
2. Install apache2 : `$ apt-get install mysql-server mysql-client`
3. Execute `$ mysql` and type `SHOW DATABASES;` and see the result : 
```
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.00 sec)

```
