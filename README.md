# 3tier-wordpress-deployment
3-Tier Web Application Deployment on AWS using the LAMP Stack (Linux, Apache, MySQL, PHP)
=======================================================================

--> Launch a Amazon Linux 2 image EC2 instance 
--> Create a RDS MySQL Engine (private)
--> Allow protocols in SG , MySQL or anywhere

==================MySQL Client=====================================

--->install MySQL client 
 yum install -y MySQL

---> Export MySQL endpoint as MySQL_HOST Variable,
example : export MYSQL_HOST=mysqldb.cdbmlufjd.ap-south-1.rds.amazon.com

export MYSQL_HOST=<your-RDS-endpoint>

=================Connect to RDS and create DB ========================

--->Connect to RDS MySQL to create required database and users

mysql -h mysqldb.cdbmlufjd.ap-south-1.rds.amazon.com -P 3306 -u admin -p

---> create a database 

CREATE DATABASE wordpress;
CREATE USER 'wpuser' IDENTIFIED BY 'root12345';
GRANT ALL PRIVILEGES ON wordpress.* TO wpuser;
FLUSH PRIVILEGES;
Exit
===================================================================
Now install and configure apache
===============================================================

sudo yum install -y httpd
sudo service httpd start

Download a wordpress template and unzip the wordpress

wget https://wordpress.org/latest.tar.gz

tar -xzf latest.tar.gz

ls

cd wordpress

-->create a wp-config file from sample file already provided

cp wp-config-sample.php wp-config.php

cat wp-config.php

--> edit the wp-config.php file to point to database 

vi wp-config.php

--> Replace the Database_name_here , "username_here" , "password_here" and "rds-endpoint-name" with valid info


// ** MySQL settings - you can get this info from your web host **//

/* The name of the database for wordpress */

define( 'DB_NAME' , 'database_name_here-wordpress' );

/** MySQL database username **/
define ( 'DB_USER' , 'username_here-wordpressuser' );

/** MySQL database password */
define( 'DB_PASSWORD' , 'password_here-root12345' );

/* MySQL hostname */
define( 'DB_HOST' , 'rds-endpoint-name-rds endpoint');

---> now go to below link and it provides some information to update the wp-config file . It looks like below shared one .

https://api.wordpress.org/secret-key/1.1/salt/ --> open this in browser , it will generate few tokens and replace in this file 
===================================================================================================

8 TOKENS WILL BE THERE REPLACE TOKEN !!!
===================================================================================================`
---> Now install dependencies

sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2

---> come back to home directory and copy all context to /var/www/html , then restart the service.

cd /home/ec2-user

sudo cp -r wordpress/* /var/www/html/

sudo service httpd restart

place public ip to the browser : it ask for username and password and install wordpress