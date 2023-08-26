## Containerization of Java project using Docker
##### WHY do we have to use Docker container?

### STATISTICS

![Container](https://github.com/aleem632/docker/blob/03ce80a6b5ac0400fcc7d5cba940b22a32d43178/Diagram/Container.png)

## Current Scenario 

Different methods are being used by companies for running business critical Multi tier Web applications on VMware, Physical servers and Cloud providers GCP, AWS and Azure so as operation team or Devops team they have manage many services for frontend and backend services,according to recently scenario companies have agile environment which required frequent code build & test and deploy to servers

## Problems

Procusing all these resources required CapEX and OpEX as in agile environment we will have frequent build & deployment but more importantly the resources are being underuse as over the period of long time it can incease high OpEx.

Having different environment for testing & pre-production and production with alternative version can cause problem for deployment 
which consume alot of time and will not be in sync.

## solution

Solution for this problem is to Containerize multi tier application because it will consume less resouces, reduce the cost and in current scenario microservice architect is being used for many companies. 

Deployment process will be much quick as deployment will be done using container images and we can use same image to deploy in different environments, this process will be repeatable and usable and it will eliminate the problem "it works on my machine" 


### BENEFITS
- Deployment via images 
- same container images across enviroment
- Reusable & Repeatable 

### FLOW OF EXECUTION

![DOCKER COMPOSE](https://github.com/aleem632/docker/blob/9c89a5adfd039d2881912037645af7bf89d59ee9/Diagram/Docker-Compose.png)

# TOOLS 

Docker 

Docker composer 

# Execution Steps

Find the right base image from Docker Hub
write docker composer.yml file to run multi docker images
Build & Run and Test the image later push the images to docker hub with right tag of your Accountname/repositoryname 

1. Find the docker engine Installation steps because steps are being updated frequently, after the installation try to run any image
in order to make sure docker engine is installed properly if you are running docker command without root user then add that user 
to docker group, use this commands

<pre>
usermod -aG docker username
</pre>

2. Go to docker hub and find mysql image and read documentation, for this particular project engine version:8.0 is required, so the image
I will be using is mysql:8.0.33 which has environment variable "mysql_root_password" and mysql_database= which will be used for the creation of database and its password for root user

This docker image has an entry point which is at /docker-entrypoint-initdb.db so once it will start that docker image will run sql file which is stored at my host machine

3. Next step will be to create multi-stage docker file which has 2 base image 
FROM openjdk-11
FROM tomcat9:jre11
First image will be use to build the artifact and Second image will be used to run Java Web app, in order to build artifacts we need 
git & maven , git will be used to clone the repository from GitHub and maven will be use to build the artifacts 
Second Image, once the artifact is ready to deploy we will use COPY command to copy our artifact build by first image to second image at the location of /usr/local/tomcat/webapp/ROOT.war but first we need to remove default file then copy

4. At Docker hub we need to search for nginx image according to project requirement we can use latest image, docker file for this image is going to be simple because we need to search where is the configuration for nginx service so we can copy our conf file because our nginx container is going to be a proxy service listen at port:80 and forward traffic to tomcat image at port:8080

5. For memcached image we can use latest as well, in order to use that memcached image it should have open port of 11211
   
6. Rabbitmq image has of variables which will be used for setting the dafault username and password for tesing purpose we can use 
guest username and guest password but we can change for security, 15672 port should be available to connect to rabbitmq

7. final step will be to create docker compose file which will used to build all the docker images and then we can use docker compose 
command to run all the docker images 
 
