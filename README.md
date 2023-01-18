# Devops-Projects-15--Containerize-Java-Web-App


# Customizing images/ building the artifact and containerizing 
* mysql
* nginx
* tomcat

## Setup Dockerfiles

![image](https://user-images.githubusercontent.com/96833570/213184115-b0cb969f-134d-47a5-81ba-8579261a2297.png)


## App Dockerfile
![image](https://user-images.githubusercontent.com/96833570/213184232-f2a76fcf-8425-4f10-bb8c-4178da30ce4e.png)


</hr>

## Web Dockerfile

![image](https://user-images.githubusercontent.com/96833570/213184443-747c75b2-de49-45b2-94ba-adb8f8067963.png)

```

FROM nginx
LABEL "Project"="Devops-projects-15"
LABEL "Author"="Gulcan"

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY nginvproapp.conf /etc/nginx/conf.d/vproapp.conf
```
### App.conf

```
upstream vproapp {
 server vproapp:8080;
}
server {
  listen 80;
location / {
  proxy_pass http://vproapp;
}
}
```

## db setup

![image](https://user-images.githubusercontent.com/96833570/213186459-1918cd39-c86f-404c-9712-5806c7e6fdf6.png)

### db_backup.sql 

![image](https://user-images.githubusercontent.com/96833570/213186534-581615ce-c5e1-4318-bfec-a97a85b633b8.png)



</hr>

```
apt install openjdk-8-jdk maven -y

```

</hr>

### application.properties

```
#JDBC Configutation for Database Connection
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://vprodb:3306/accounts?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=conve$jdbc.username=root
jdbc.password=vprodbpass

#Memcached Configuration For Active and StandBy Host
#For Active Host
memcached.active.host=vprocache01
memcached.active.port=11211
#For StandBy Host
memcached.standBy.host=vprocache02
memcached.standBy.port=11211

#RabbitMq Configuration
rabbitmq.address=vpromq01
rabbitmq.port=15672
rabbitmq.username=guest
rabbitmq.password=guest

#Elasticesearch Configuration
elasticsearch.host =vprosearch01
elasticsearch.port =9300
elasticsearch.cluster=vprofile
elasticsearch.node=vprofilenode
```


![image](https://user-images.githubusercontent.com/96833570/213188881-7f6adad2-8d91-4189-a382-433829df138e.png)


After ensuring the application conf file and Dockerfiles match run `mvn install`


![image](https://user-images.githubusercontent.com/96833570/213191208-ad683211-7b36-448a-b84c-400c3bbae974.png)


## Building the images

* `cp -r target Docker-files/app/`

* /app# `docker build -t elkakimmie/devops:V.1 .`


![image](https://user-images.githubusercontent.com/96833570/213193458-40f5a4d1-ca73-4221-a131-a2b12310d450.png)

* `docker build -t elkakimmie/db:v1 .`

![image](https://user-images.githubusercontent.com/96833570/213238870-fdac6e05-e092-4332-ad7b-b187cbe184f0.png)


* `docker build -t elkakimmie/web-nginx:v1 .`

![image](https://user-images.githubusercontent.com/96833570/213240331-0d369149-ad57-4217-9744-413dd3480b04.png)

</hr>

# Containerizing rabbitmq and memcached



