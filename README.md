# Spring-Boot-Docker
Dockerising the Spring-boot-microservice

Please refer to this link for Docker installation on Windows 10 HOME edition

https://itnext.io/install-docker-on-windows-10-home-d8e621997c1d

#####TO run MySQL Container:
``
docker run --detach --name ec-mysql -p 6604:3306 -e MYSQL_ROOT_PASSWORD=password -e MYSQL_USER=user -e MYSQL_PASSWORD=password -d mysql
``
#####To open MySQL Bash:
``
docker exec -it ec-mysql bash
mysql -u root -p
``

#####Run Docker container with docker profile set in Dockerfile and migration scripts on host

``
docker run --name ec-app -p 8080:8080 -v C:/Users/omrfr/Desktop/db/migration:/var/migration -e server=ec-mysql -e port=3306 -e dbuser=user -e dbpassword=pass --link ec-mysql:mysql -d explorecali
``

#####we can use docker.maven.plugin from spotify to run the app in docker
``
mvn package -DskipTests docker:build 
``
###### container with default property set in Dockerfile
``
docker run --name ec-app-default -p 8080:8080  -d explorecali-default
``
##### Build jar, image, set mysql profile
``
mvn package -DskipTests docker:build -D ec-profile=mysql
``
##### Run Docker container with mysql profile
``
docker run    --name ec-app-mysql -p 8181:8080  --link ec-mysql:mysql -d explorecali-mysql
``
##### Build jar, image, set docker profile
``
mvn package -DskipTests docker:build -D ec-profile=docker
``
##### Run Docker container with docker profile set in Dockerfile and migration scripts on host
``
docker run --name ec-app-docker -p 8282:8080 -v ~/db/migration:/var/migration -e server=ec-mysql -e port=3306 -e dbuser=cali_user -e dbpassword=cali_pass --link ec-mysql:mysql -d explorecali-docker
``
##### Enter Docker container
``
docker exec -t -i ec-app /bin/bash
``
#####
##### Push image to Docker hub
######Login to Docker hub locally
``docker login``
###### Upload image
``
docker tag <image id> <docker hub repository>/explorecali-default:latest
docker tag b94a80fbe703 mathanpointer/explorecali-default:latest
``
###### Download image
``
docker pull <docker hub repository>/explorecali-default
``