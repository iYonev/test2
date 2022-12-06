
# Deploy LEMP with Docker
1.Create our docker-compose yaml file inside it will be defined :
* mysql - will be given the root password and will listen on port 3306
* ngix web server -we will set the nginx server configuration file from our local directory with help of volumes   
using depends_on we make sure the db is already available before starting the web server 
* php container - with fpm module and we will open port 9000
* phpmyadmin - where phpmyadmin host maps to our database,root pass is the same ,and port that phpmyadmin needs to use for conneting to mysql (3306)
```
  mysql:
   image: mariadb
   environment:
    MYSQL_ROOT_PASSWORD: admin
   ports:
    - "3306:3306"
  nginx:
    image: tutum/nginx
    ports:
      - "8084:80"
    volumes:
        - ./default:/etc/nginx/sites-available/default
        - ./default:/etc/nginx/sites-enabled/default
    deploy:
      mode: replicated
      replicas: 2
    depends_on:
       - mysql

  phpfpm:
    image: php:fpm
    ports:
        - "9000:9000"
    volumes:
        - ./public:/usr/share/nginx/html
    deploy:
      mode: replicated
      replicas: 3


  phpmyadmin:
   image: phpmyadmin/phpmyadmin
   ports:
    - 8183:80
   environment:
    - PMA_HOST=mysql
    - MYSQL_ROOT_PASSWORD=admin
    - PMA_PORT=3306
~                
```
2.We deploy our stack
then check if our service is available with <code>docker service ls</code>  
![image](https://user-images.githubusercontent.com/114083012/205907253-cbf9834f-92fd-45b4-a80e-c5171f513992.png)
![image](https://user-images.githubusercontent.com/114083012/205907564-4f339b96-23dc-43ba-a7c7-4000fbcfe739.png)
![image](https://user-images.githubusercontent.com/114083012/205907700-c69bdb0e-5962-4810-a3ab-43a866fc2358.png)
