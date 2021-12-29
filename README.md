# docker-lab-practice
~~~
Jenkins lauch on Docker environment.

I have run this command direct attached IP on virtual machine, 
Floating IP attached with private IP does not worked/tested or not sure.

docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11


##docker run with name: 

docker run -p 6003:6379 --name redis -d redis:4.0


Troubleshooting
****************************
docker logs redis

docker exec -it redis /bin/bash

Create Network
*******************************
docker network create mongo
 
 
#COMMAND

##Create docker network
docker network create mongo

##start mongodb
 
docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--net mongo \
--name mongodb \
mongo

 
##start mongo-express
docker run -d \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_MONGODB_SERVER=mongdb \
--net mongo \
--name mongo-express \
mongo-express


###Docker-compose

[root@docker docker-compose]# cat mongo.yaml
version: '3'
services:
        mongodb:
                image: mongo
                ports:
                        - 27017:27017
                environment:
                        - MONGO_INITDB_ROOT_USERNAME=admin
                        - MONGO_INITDB_ROOT_PASSWORD=password
        mongo-express:
                image: mongo-express
                ports:
                        - 8080:8081
                environment:
                        - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
                        - ME_CONFIG_MONGODB_ADMINPASSWORD=password
                        - ME_CONFIG_MONGODB_SERVER=mongodb


### run docker-compose 
docker-compose -f mongo.yaml up -d
docker-compose -f mongo.yaml down 





### Dockerfile

###Docker Volume 

3 Volumes Type

1. Host Volumes
2. Anonymous Volumes 
3. Named Volumes 

# Host Volumes
docker run -v /home/mount/data:/var/lib/mysql/data
              "      HOST    ":"     Container     "
			  
			  
#Anonymous Volumes
/var/lib/docker/volumes/random-hash/_data            ;automatically created by Docker

#Named Volumes
docker -v name:/var/lib/mysql/data

## Docker Compose Run with Named Volumes:

[root@docker docker-compose]# cat mongo-persistence.yaml
version: '3'
services:
        mongodb:
                image: mongo
                ports:
                        - 27017:27017
                environment:
                        - MONGO_INITDB_ROOT_USERNAME=admin
                        - MONGO_INITDB_ROOT_PASSWORD=password
                volumes:
                        - mongo-data:/data/db
        mongo-express:
                image: mongo-express
                ports:
                        - 8080:8081
                environment:
                        - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
                        - ME_CONFIG_MONGODB_ADMINPASSWORD=password
                        - ME_CONFIG_MONGODB_SERVER=mongodb

volumes:
        mongo-data:
                driver: local


			  



~~~
