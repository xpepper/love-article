<p align="center">
  <a target="_blank"><img src="https://angular.io/assets/images/logos/angular/angular_solidBlack.png" width="120" alt="Angular Logo" /></a>
<a target="_blank"><img src="https://spring.io/images/projects/spring-edf462fec682b9d48cf628eaf9e19521.svg" width="100" alt="Spring Logo" /></a>
</p>

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/fedexu/love-article/blob/master/LICENSE)
[![sonar qube](https://sonarcloud.io/api/project_badges/measure?project=fedexu_love-article&metric=alert_status)](https://sonarcloud.io/dashboard?id=fedexu_love-article)

# Love Article

This is a tutorial project, the purpose is to show one of the many possible implementation of the <a target="_blank" rel="noopener noreferrer" href="http://martinfowler.com/microservices/">Microservice Architecture Pattern</a> by using Spring Boot, Spring Cloud and Docker.

<p align="center">
  <a target="_blank"><img src="https://user-images.githubusercontent.com/22296699/74551163-fd12c600-4f52-11ea-8343-a66c0a87759c.gif" alt="angular-pwa-gif" /></a>
</p>


## Functional services

Love Article is created with three core microservices. All of them are developed following the pattern Domain-Driven Design (DDD) and they are independently deployable applications.

<p align="center">
  <img width="868" alt="functional_schema" src="https://user-images.githubusercontent.com/22296699/76226612-65855800-621e-11ea-82ce-4616a1715905.png">
</p>

#### Favorites service
Contains the favorite articles and metadata about them.

//TODO API TABLE

#### Articles service
It's the data service that contains all the articles of the site

//TODO API TABLE

#### Statistics service
Performs calculations about the favorites articles of all the users and the "Hot topic" on the site. It work on the "tag" on the articels and the number of the favorites topic along with the creation date of the article itself.
The data generated it's used to order articles list on the main window.

//TODO API TABLE

## Infrastructure architecture

In this project there is some distributed system patterns implemented. <a target="_blank" rel="noopener noreferrer" href="http://projects.spring.io/spring-cloud/">Spring cloud</a> enhance Spring Boot applications to implement those patterns and it's used in this project along with some Netflix OSS projects.

<p align="center">
  <img width="963" alt="infrastructure_architecture" src="https://user-images.githubusercontent.com/22296699/79771007-7a313180-832e-11ea-9637-a3f3b8305181.png">
</p>

### Spring Cloud Config Server [<a target="_blank" rel="noopener noreferrer" href="https://cloud.spring.io/spring-cloud-config/reference/html/">Documentation</a>]
Spring Cloud Config provides server-side and client-side support for externalized configuration in a distributed system. With the Config Server, you have a central place to manage external properties for applications across all environments.

### Eureka Server [<a target="_blank" rel="noopener noreferrer" href="https://github.com/Netflix/eureka/wiki/Eureka-at-a-glance">Documentation</a>]
Eureka is a REST based service that is primarily used for locating services for the purpose of load balancing and failover of middle-tier servers.
The client also has a built-in load balancer that does basic round-robin load balancing.

### Zuul Gateway [<a target="_blank" rel="noopener noreferrer" href="https://github.com/Netflix/zuul/wiki">Documentation</a>]
Zuul is the front door for all requests from devices and web sites to the backend services.
Is built to enable dynamic routing, monitoring, resiliency and security.

### Zipkin [<a target="_blank" rel="noopener noreferrer" href="https://zipkin.io/">Documentation</a>]
Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures. Features include both the collection and lookup of this data. You can search the transactions by use the Trace ID generated by Spring sleuth in the dashboard and analize the calls directly with the UI.

### Rabbit Mq - Message broker [<a target="_blank" rel="noopener noreferrer" href="https://www.rabbitmq.com/">Documentation</a>]
RabbitMQ is one of the most famous open source message broker.
Why a message broker? Leave ad open door to a message broker service in your architecture help you to go to event-driven programming and solve many problem with this approach instead of REST calling between microservices (that is not a good things...)

### Security
Advanced security is out of the scope for this POC (proof-of-concept) project. We use a simply JWT authentication implemented with filters on spring security level.
To see more of security topic with spring cloud try to see <a target="_blank" rel="noopener noreferrer" href="http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_security">here</a>.

## Local environment run

This project use 9 Spring Boot application, 4 Database instances and RabbitMq.
Ensure that you have at least `4 Gb` RAM available by running it with Docker-compose. A second options is to run the core services (Gateway, Registry, Config, Auth Service and Functional services ) manually.

#### Before you start
- Be sure to clone the project with the command: `git clone --recurse-submodules <url>`
- Install Docker along with Docker Compose.
- Build the project: `mvn clean package -DskipTests`

#### Run the app
To run the app simply type `docker-compose up --build`
All the images will be builded locally and runned. If you want to detach the process add -d in the command.

#### Important endpoints
Run locally with Eclipse/IntelliJ:
- http://localhost:4000 - Gateway
- http://localhost:7777 - Eureka Discovery Dashboard
- http://localhost:15000 - RabbitMq (username/password: guest/guest)

Run with docker-compose:
- http://localhost:8080 - Gateway
- http://localhost:8082 - Eureka Discovery Dashboard
- http://localhost:8083 - RabbitMq (username/password: guest/guest)

To use api with Postman or other tool remember to do a POST on `http://"gateway"/auth/login` with payload `{"username": "dummy", "password": "dummy"}` (or other pre-autorized user created in the Docker scripts), to retrieve a valid JWT toket. Otherwise the Gateway will reject any request except the PWA application static files.

## Notes

If you see some error in the startup process, no worries. The Docker compose V3 have removed the `depends_on` option and the application are resilient to the fail startup.

Discovery Service take a while to register all the applications (default heartbeat is 30s). This service will be the first to start up but you need some time to see all the services register to it.

## Help me to get the app famous!

Love Article is open source and want to give to you the basics knowledge about cloud native application with Spring and java technologies. A star to this project will be appreciate!
