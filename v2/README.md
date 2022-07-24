* Notice how all three services were placed on the same network called “mynet”. This prevents an annoying error notice from phpMyAdmin where it complains about the PMA_HOST. Also notice how both the “phpmyadmin” and “wordpress” services use/depend on the “db” service.


* Volumes parameter creates the copy of /var/lib/mysql folder from WSL directory in your project under mysql folder volumes: db_data:/var/lib/mysql. Any changes made here will be synced to the container folder



* Start the Docker containers 
```
$ docker-compose up -d
```

* Stop your currently running docker-compose session
```
$ docker-compose stop
```

* Remove the existing container
```
$ docker-compose rm wordpress
```

* To check where Wordpress is installed
```
$ cd ~\wordpress
$ ls wp_html
```

* To access PHPMyAdmin and to create the database
http://127.0.0.1:8081

* To setup Wordpress, add in the database config details to wp-config.php in the wordpress setup folder and then visit http://127.0.0.1:8080 


