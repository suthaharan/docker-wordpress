# Multisite PHP or Wordpress website with Single Apache host file

## Features
- Easily configurable
- Single default apache conf file
- Single Wordpress image
- Multiple sites can be configured from localhost 

## Files to edit
* Add website folder in the local machine and map it to the container volume
* Make the necessary virtual host configuration inside sites-available apache configuration setting
* Map /etc/hosts from the local linux or mac workstation to have the virtual host name
* Add the virtual host names under environment "Virtual Hosts" section


Edit docker-compose.yml
```sh
version: '3.9'
 
networks:
  haranet:
 
volumes:
  db_data: {}
 
services:
  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: "${DB_ROOT_PASSWORD}"
      MYSQL_DATABASE: "${DB_NAME}"
      MYSQL_USER: "${DB_USER}"
      MYSQL_PASSWORD: "${DB_PASSWORD}"
    networks:
      - haranet
 
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
    networks:
      - haranet
    depends_on:
      - db
  

site1:
    image: wordpress:latest
    volumes:
      - ./docker/php.ini:/usr/local/etc/php/conf.d/wp-php.ini
      - ./site1:/var/www/html/site1
      - ./site2:/var/www/html/site2
      - ./site3:/var/www/html/site3
      - ./conf/000-default.conf:/etc/apache2/sites-available/000-default.conf
      - /etc/hosts:/etc/hosts
    environment:
      - VIRTUAL_HOSTS=site1,site2,site3
    ports:
      - "80:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: "${DB_USER}"
      WORDPRESS_DB_PASSWORD: "${DB_PASSWORD}"
      WORDPRESS_DB_NAME: "${DB_NAME1}"
    networks:
      - haranet
    depends_on:
      - db
```

Create an apache conf file in current directory under conf folder as per below:
vi ./conf/000-default.conf
```sh
NameVirtualHost *:80
<VirtualHost *:80>

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/site1
	ServerName site1

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

<VirtualHost *:80>

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/site2
    ServerName site2

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

<VirtualHost *:80>

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/site3
    ServerName site3

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```