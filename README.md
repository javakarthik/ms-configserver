# README #

Spring Cloud Config Server's accompanying source code for blog posts at http://tech.asimio.net/2016/12/09/Centralized-and-Versioned-Configuration-using-Spring-Cloud-Config-Server-and-Git.html and http://tech.asimio.net/2017/02/02/Refreshable-Configuration-using-Spring-Cloud-Config-Server-Spring-Cloud-Bus-RabbitMQ-and-Git.html

### Requirements ###

* Java 8
* Maven 3.3.x
* Docker host or Docker machine
* In case of running Config Server in **registration-first** mode, running instance(s) of Eureka Service as described at https://bitbucket.org/asimio/discoveryserver/ would be needed.
* In case of running Config Server in **config-monitor** mode, a RabbitMQ host is needed with this exchange created:

```
name=springCloudBus
type=topic
durable=true
autoDelete=false
internal=false
```

and a Git repo webhook to notify Config Server of file changes

### Building the artifact ###

```
mvn clean package
```

### Executing service in config-first or standalone mode ###
```
java -jar target/config-server.jar
or
java -Dspring.profiles.active=standalone -jar target/config-server.jar
or
java -DappPort=8101 -jar target/config-server.jar
or multiple instances:
java -DappPort=8101 -jar target/config-server.jar
java -DappPort=8102 -jar target/config-server.jar
```

### Executing service in registration-first mode ###

Multiple instances. Also assumes two instances of Eureka Server as described at https://bitbucket.org/asimio/discoveryserver/
```
java -Dspring.profiles.active=registration-first -DappPort=8101 -DhostName=localhost -Deureka.client.serviceUrl.defaultZone="http://localhost:8001/eureka/,http://localhost:8002/eureka/" -jar target/config-server.jar
java -Dspring.profiles.active=registration-first -DappPort=8102 -DhostName=localhost -Deureka.client.serviceUrl.defaultZone="http://localhost:8001/eureka/,http://localhost:8002/eureka/" -jar target/config-server.jar
```

Open http://localhost:8001 or http://localhost:8002 in a browser

### Executing service in config-first and config-monitor mode ###

```
java -Dspring.active.profiles=config-monitor -DappPort=8101 -Dspring.rabbitmq.host=<rabbitmq host> -Dspring.rabbitmq.port=<rabbitmq port> -Dspring.rabbitmq.virtual-host=<rabbitmq-vhost> -Dspring.rabbitmq.username=<rabbitmq user> -Dspring.rabbitmq.password=<rabbitmq passwd> -jar target/config-server.jar
```

### How do I get set up using Docker? ###

```
sudo docker pull asimio/config-server
```

Multiple containers. Also assumes two instances of Eureka Server as described at https://bitbucket.org/asimio/discoveryserver/
```
sudo docker run -idt -p 8101:8101 -e appPort=8101 -e spring.profiles.active=registration-first -e hostName=localhost -e eureka.client.serviceUrl.defaultZone=http://localhost:8001/eureka/,http://localhost:8002/eureka/ asimio/config-server:latest
sudo docker run -idt -p 8102:8102 -e appPort=8102 -e spring.profiles.active=registration-first -e hostName=localhost -e eureka.client.serviceUrl.defaultZone=http://localhost:8001/eureka/,http://localhost:8002/eureka/ asimio/config-server:latest
```

Open http://localhost:8001 or http://localhost:8002 in a browser

### Who do I talk to? ###

* orlando.otero at asimio dot net
* https://www.linkedin.com/in/ootero