## Linux Server Configuration
This project is about server configuration and deploying of an web application previously developped(Items Catalog)

## Server Ip Address
52.43.194.43

## Application Items Catalog link
http://ec2-52-43-194-43.us-west-2.compute.amazonaws.com

## Summary of configuration
- Create new user "grader" and give him sudo permissions
	- sudo adduser grader
	- type superheros as password
	- sudo echo "grader ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/grader
- Enable ssh login for user grader: create ssh key pair, save public key in remote server /home/grader/.ssh/authorized_keys
- Check and update installed paskages
	- sudo apt-get update
	- sudo apt-get upgrade
- Change ssh port : edit file /etc/ssh/sshd_config, on line port 22, change port 22 to 2200
- Disable ssh login for root: edit file /etc/ssh/sshd_config, on line PermitRootLogin without-password, change to PermitRootLogin no
- Restart ssh service : sudo service ssh restart
- Configure firewall UFW
	- sudo ufw allow 2200 
	- sudo ufw allow www 
	- sudo ufw allow ntp 
	- sudo ufw enable
- Configure local timezone to UTC: follow instructions giving by this command <code>sudo dpkg-reconfigure tzdata</code>
- Install Apache and mod_wsgi
	- sudo apt-get install apache2
	- sudo apt-get install libapache2-mod-wsgi
- Install and configure PostgreSQL
	- sudo apt-get install postgresql
	- edit this file /etc/postgresql/POSTGRESQL_VERSION/main/pg_hba.conf to disable remote connections on postgresql 
 	- sudo /etc/init.d/postgresql restart
- Install git
	- sudo apt-get install git

- Create Database and Database User
	- sudo su - postgres
	- psql -c "CREATE USER catalog NOCREATEDB NOSUPERUSER PASSWORD 'catalog'"
	- psql -c "CREATE DATABASE catalog"
	- psql -c "GRANT All ON DATABASE catalog TO catalog"
- Deploye application
	- sudo mkdir /var/www/html/catalog
	- sudo nano /var/www/html/catalog/catalog.wsgi. Add application wsgi settings
	- edit file /etc/apache2/sites-enabled/000-default.conf to add catalog wsgi setting and restart Apache service
	- sudo mkdir /var/www/html/catalog/lib
	- cd /var/www/html/catalog 
	- Installed project dependencies: Flask, SQLAlchemy, google-api-python-client, oauth2client
	- sudo git clone MY-GITHUB-CATALOG-REPOSITERY and final settings


## Private Key 
A private key will be given in review notes. Use this private key to connect to server as user grader

## How to connect to server
- create file /home/USERNAME/.ssh/key.rsa (if .ssh/ folder doesn't exist, create one)
- copy private key(grader private key given in notes) into key.rsa file
- change file persmissions on .ssh/ and key.rsa
	- chmod 600 ~.ssh/key.rsa
- ssh -i ~.ssh/key.rsa -p 2200 grader@52.43.194.43
