# 3tier-wordpress-deployment
3-Tier Web Application Deployment on AWS using the LAMP Stack (Linux, Apache, MySQL, PHP)

1. Launch a Amazon Linux 2 image EC2 instance 
![]()
2. Create a RDS MySQL Engine (private)
   Allow protocols in SG , MySQL or anywhere
![]()

3. Login to the EC2 instance and install MySQL client
  
```bash
 yum install -y MySQL
```
Export MySQL endpoint as MySQL_HOST Variable
```bash
export MYSQL_HOST=backend-db.cxi20mqqq3c5.ap-south-1.rds.amazonaws.com
```

4. Connect to RDS and create Database

Connect to RDS MySQL to create required database and users
```bash
mysql -h backend-db.cxi20mqqq3c5.ap-south-1.rds.amazonaws.com -P 3306 -u admin -p
```
create a database
```bash
CREATE DATABASE wordpress;
CREATE USER 'wpuser' IDENTIFIED BY 'root12345';
GRANT ALL PRIVILEGES ON wordpress.* TO wpuser;
FLUSH PRIVILEGES;
Exit
```

5. Now install and configure apache server to deploy the wordpress application

```bash
yum install -y httpd
service httpd start
```

Download a wordpress template and unzip the wordpress

```bash
wget https://wordpress.org/latest.tar.gz

tar -xzf latest.tar.gz

ls

cd wordpress
```
![]()

Create a wp-config file from sample file already provided.
```bash
touch wp-config.php

cp wp-config-sample.php wp-config.php

cat wp-config.php
```

6. Edit the wp-config.php file to point to backend-db database.
```bash
vi wp-config.php
```

Replace the Database_name_here , "username_here" , "password_here" and "rds-endpoint-name" with valid information

/* The name of the database for wordpress */
```bash
define( 'DB_NAME' , 'wordpress' );

/** MySQL database username **/
define ( 'DB_USER' , 'wpuser' );

/** MySQL database password */
define( 'DB_PASSWORD' , 'root12345' );

/* MySQL hostname */
define( 'DB_HOST' , 'backend-db.cxi20mqqq3c5.ap-south-1.rds.amazonaws.com');
```

7. Now go to below link and it provides some information to update the wp-config file .
    It looks like below shared one .
```bash
https://api.wordpress.org/secret-key/1.1/salt/ --> open this in browser , it will generate few tokens and replace in this file.
```
8. Now install dependencies for the Amazon-linux-2 machine.

```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```

9. Come back to home directory and copy all context to /var/www/html , then restart the service.

```bash
cd /home/ec2-user

cp -r wordpress/* /var/www/html/

service httpd restart
```

10. place the EC2 public ip to the browser : it ask for username and password and install wordpress.

![]()

![]()

