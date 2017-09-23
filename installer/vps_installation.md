# VPS Installation Guide.
A guide to configure my VPS.

## First Connection (as root)
1. Update system : `$ apt-get update && apt-get upgrade`
2. Change root password : `$ passwd root`
3. Add new user : `adduser [username]`
4. Change default port for ssh connection : `$ nano /etc/ssh/sshd_config`
5. Restart ssh service : `$ /etc/init.d/ssh restart`
6. Exit connection.

## Install & configure Apache server
### Apache Installation
1. Connect as root : `$ su`
2. Install apache2 : `$ apt-get install apache2`
3. Go on your web browser on the url of your server and check if apache work : _It Work!_ webpage
4. Install PHP7 and some other libraries : `$ apt-get install php7.0 libapache2-mod-php7.0 php7.0-mysql php7.0-curl php7.0-json php7.0-gd php7.0-mcrypt php7.0-intl php7.0-sqlite3 php7.0-gmp php7.0-mbstring php7.0-xml php7.0-zip`
5. Add the following code on _/var/www/html/info.php_ : `<?php phpinfo() ?>;`
6. Go on page `http://your_server_ip/info.php` and check the version of PHP.

### Apache Configuration
#### Remove server information when 404 error occurred
1. Connect as root : `$ su`
2. Open file _apache2.conf_ : `$ nano /etc/apache2/apache2.conf`
3. Add the following instructions at the end of the file :
```
# Remove information about server when error HTTP 404 ocurred.
ServerSignature Off
ServerTokens Prod
```
4. Restart Apache : `$ systemctl restart apache2`

## Install MySQL service
1. Connect as root : `$ su`
2. Install Mysql server and Mysql client : `$ apt-get install mysql-server mysql-client`
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
4. Follow the documentation to add new user on mysql : [Adding User Accounts](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html)

## Install Java and Apache Tomcat server
### Java installation
1. Connect as root : `$ su`
2. Install package to add _add-apt-repository_ command : `$ apt-get install software-properties-common`
3. Add package for java : `$ add-apt-repository ppa:webupd8team/java`
4. Update your system : `$ apt-get update`
5. Add Java8 : `$ apt-get install oracle-java8-installer`
6. Set Java8 as default version : `$ apt-get install oracle-java8-set-default`
7. Check the version of java installed : `$ java -version`
8. Add JDK : `$ apt-get install openjdk-8-jdk` 

### Tomcat installation
1. Instantiate global variable JAVA_HOME, JRE_HOME and CATALINA_HOME :
```
$ echo "export CATALINA_HOME="/opt/tomcat9"" >> /etc/environment
$ echo "export JAVA_HOME="/usr/lib/jvm/java-8-oracle"" >> /etc/environment
$ echo "export JRE_HOME="/usr/lib/jvm/java-8-oracle/jre"" >> /etc/environment
```
2. Download Apache Tomcat binary source : `$ wget http://apache.mirrors.ovh.net/ftp.apache.org/dist/tomcat/tomcat-9/v9.0.0.M26/bin/apache-tomcat-9.0.0.M26.tar.gz`
3. Decompress source : `$ tar xvzf apache-tomcat-9.0.0.M26.tar.gz`
4. Rename folder : `$ mv apache-tomcat-9.0.0.M26 tomcat9`
5. Move Tomcat source on _/opt_ : `$ mv tomcat9/ /opt/`
6. Go on `http://your_server_ip:8080` and check if you see the page of Tomcat
7. Add role and user on _CATALINA_HOME/conf/tomcat-users.xml_ : 
```
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
  <role rolename="manager-gui"/>
  <user username="admin_name" password="your_password" roles="manager-gui"/>
  <user username="your_name" password="your_password" roles="manager-gui"/>
</tomcat-users>
```
8. Comment **Valve** attribute on _CATALINA_HOME/webapps/manager/META_INF/context.xml_ : 
```
<?xml version="1.0" encoding="UTF-8"?>
<Context antiResourceLocking="false" privileged="true" >
    <!-- 
    <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> 
    -->
    <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```
9. Go on `http://your_server_ip:8080` and try to connect on Manager App page.

## Install Composer
1. Connect as root : `$ su`
2. Update your system : `$ apt-get update`
3. Install usefull dependencies for Composer : `$ apt-get install curl`
4. Download composer source with curl : `$ curl -s https://getcomposer.org/installer | php`
5. Move _composer.phar_ on _/usr/local/bin/composer_ : `$ mv composer.phar /usr/local/bin/composer`
6. Check if composer is correctly installed with : `$ composer --version`. This command return `Composer version 1.5.1 2017-08-09 16:07:22` with your version and your date.

## Install Unzip
1. Connect as root : `$ su`
2. Update your system : `$ apt-get update`
3. Install unzip : `$ apt-get install unzip`
4. Use unzip to unzip project download from Github : `$ unzip master.zip -d dest_folder` 

# How to ?
- Download Github source : `$ wget https://github.com/username/repository/archive/master.zip`
- Change locale when perl error occurred : `$ local-gen fr_FR.UTF-8 && dpkg-reconfigure locales`.
