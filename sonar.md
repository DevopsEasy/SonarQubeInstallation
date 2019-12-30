# SonarQubeInstallation
SonarQube Installation Steps
# Sonarqube Installation Steps for Ubuntu 18

### Update the packages

sudo apt update

### Install Software Properties
sudo apt install software-properties-common -y

### Install Java 11

sudo apt install openjdk-11-jdk -y

### Install Postgress 

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

### Add Key for Postgress

wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

### Now Enter following commands to install Postgress
sudo apt-get update -y
sudo apt-get install postgresql postgresql-contrib

### Now check whether Postgress is running

sudo systemctl status postgresql


### Now add user Sonar 
sudo adduser sonar

### Now create a folder sonar & give permission

sudo mkdir /opt/sonar

sudo chown -R sonar:sonar /opt/sonar

### Go to sonar folder

cd /opt/sonar

### Download SonarQube

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.1.0.31237.zip

### Install unzip package

sudo apt install unzip -y

### Now Unzip the Sonar package

sudo unzip sonarqube-8.1.0.31237.zip

### Now create password for postgres

sudo passwd postgres

### Now switch to postgres user

 su - postgres

 ### Now create another user after switching 

createuser sqube

### Enter below command 

psql

### Now alter user with passwd

ALTER USER sqube WITH ENCRYPTED password 'sqube';

### Create Database 

CREATE DATABASE sqube OWNER sqube;

### Quit session

\q

### Now exit from postgres user

exit

### Now edit sonar properties file

sudo nano sonarqube-8.1.0.31237/conf/sonar.properties

### Modify the following content

# The schema must be created first.
sonar.jdbc.username=sqube
sonar.jdbc.password=sqube

#----- PostgreSQL 9.3 or greater
# By default the schema named "public" is used. It can be overridden with the p$
sonar.jdbc.url=jdbc:postgresql://localhost/sqube

# By default, ports will be used on all IP addresses associated with the server.
sonar.web.host=127.0.0.1



#sonar.ce.javaOpts=-Xmx512m -Xms128m -XX:+HeapDumpOnOutOfMemoryError

# Same as previous property, but allows to not repeat all other settings like -$
sonar.ce.javaAdditionalOpts=-server


#------------------------------------------------------------------------------$
# ELASTICSEARCH

## Exit from Nano Editor & edit sonar.sh file

sudo nano sonarqube-8.1.0.31237/bin/linux-x86-64/sonar.sh

#  the JVM and is not useful in situations where a privileged resource or
#  port needs to be allocated prior to the user being changed.
RUN_AS_USER=sonar


### Now edit the elastic search file 

sudo nano sonarqube-8.1.0.31237/elasticsearch/config/elasticsearch.yml
# Use a descriptive name for the node:
#
node.name:${hostname}
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 0.0.0.0

# come out

### Go to visudo and give SUDO Priviliges to sonar user
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
sonar   ALL=(ALL:ALL) NOPASSWD:ALL

# give folder permission
 sudo chown -R sonar:sonar /opt/sonar

### Start SonarQube

/opt/sonar/sonarqube-8.1.0.31237/bin/linux-x86-64/sonar.sh start

sudo netstat -plnt

# memory issue will occur 
sudo sysctl -w vm.max_map_count=2621444

# start sonaqqube


### Install Apache2

sudo apt install apache2 -y
sudo a2enmod proxy
sudo a2enmod proxy_http

# Go to apache2 conf file & enter following

sudo nano /etc/apache2/sites-available/sonar.conf
















