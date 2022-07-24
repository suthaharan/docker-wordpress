### Basic Wordpress Setup in the Local Environment

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


