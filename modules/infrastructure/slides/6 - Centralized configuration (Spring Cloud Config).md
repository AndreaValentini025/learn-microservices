# Centralized Configuration

## Introduction to Spring Cloud Config Server

When it comes to setting up a config server, there are a number of options to consider:
* **Selecting a storage type** for the configuration repository
* **Deciding on the initial client connection**, either to the config server or to the discovery server
* **Securing the configuration**, both against unauthorized access to the API and by avoiding storing sensitive information in plain text in the configuration repository

### Storage type
Spring Cloud Config server supports the storing of configuration files in a number of different backends:
* Git repository
* Local filesystem
* HashiCorp Vault
* JDBC database

See the [reference documentation](https://docs.spring.io/spring-cloud-config/reference/server.html) for the full list. In this chapter, we will use a local filesystem.

### Initial client connection
**By default, a client connects first to the config server to retrieve its configuration**. Based on the configuration, it connects to the discovery server, to register itself. With this approach, it is possible to store the configuration of the discovery server in the config server. **One concern with connecting to the config server first is that the config server can become a single point of failure (i.e., Spring Config Server is not a distributed application!).**

It is also possible the other way around, that is, the client first connects to the discovery server to find a config server instance and then connects to the config server to get its configuration. If the clients connect first to a discovery server there can be multiple config server instances registered so that a single point of failure can be avoided. 

If you prefer to use the discovery server to locate the config server, you can do so by setting _spring.cloud.config.discovery.enabled=true_ (the default is false). The net result of doing so is that client applications only need the appropriate discovery configuration. For example, you need to define the Eureka server address (_eureka.client.serviceUrl.defaultZone_). The price for using this option is an extra network round trip on startup, to locate the service registration. The benefit is that, as long as the discovery server is a fixed point, the config server can change its coordinates.

See the [reference documentation](https://docs.spring.io/spring-cloud-config/reference/client.html#discovery-first-bootstrap) for more details.

### Securing the configuration
When the configuration information is asked for by a microservice, or anyone using the API of the config server, it will be protected against eavesdropping by the edge server since it already uses **HTTPS**. To ensure that the API user is a known client, we will use **HTTP basic authentication**. 

To avoid a situation where someone with access to the configuration repository can steal sensitive information, such as passwords, the config server supports the **encryption of configuration information when stored on disk**. The config server supports the use of both symmetric and asymmetric keys. Asymmetric keys are more secure but harder to manage.

### The Config Server API
The config server exposes a REST API that can be used by its clients to retrieve their configuration. We will use the following endpoints in the API:
* _/actuator_: The standard actuator endpoint exposed by all microservices. As always, these should be used with care. They are very useful during development but must be locked down before being used in production.
* _/encrypt_ and _/decrypt_: Endpoints for encrypting and decrypting sensitive information. These must also be locked down before being used in production.
* _/{microservice}/{profile}_: Returns the configuration for the specified microservice and the specified Spring profile.
We will see some sample uses for the API when we try out the config server.

## Setting up Spring Cloud Config Server

### Maven dependencies
To include a Config Server service in our ecosystem, create an empty service and add the *spring-cloud-starter-netflix-eureka-client*, *spring-cloud-config-server*, *spring-boot-starter-security* dependencies.

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
```

### Configuration

```
server.port: 8888
spring.application.name: config-server

spring.security.user:
  name: ${CONFIG_SERVER_USR}
  password: ${CONFIG_SERVER_PWD}

spring.profiles.active: native
spring.cloud.config.server.native.searchLocations: file:/home/nicolab/IdeaProjects/learn-microservices/code/spring-cloud-config-end/config-repo
encrypt.key: ${CONFIG_SERVER_ENCRYPT_KEY}

eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
    initialInstanceInfoReplicationIntervalSeconds: 5
    registryFetchIntervalSeconds: 5
  instance:
    leaseRenewalIntervalInSeconds: 5
    leaseExpirationDurationInSeconds: 5

# WARNING: Exposing all management endpoints over http should only be used during development, must be locked down in production!
management.endpoint.health.show-details: "ALWAYS"
management.endpoints.web.exposure.include: "*"

logging:
  level:
    root: info

---
spring.config.activate.on-profile: docker
eureka.client.serviceUrl.defaultZone: http://eureka:8761/eureka/
spring.cloud.config.server.native.searchLocations: file:/config-repo
```

After moving the configuration files from each client’s source code to the configuration repository, we will have some common configuration in many of the configuration files, for example, for the configuration of actuator endpoints and how to connect to Eureka. 
* the common parts have to be placed in a common configuration file _application.yml_
* this file is shared by all clients

The configuration repository can be found in _/config-repo_.

```
config-repo/
├── application.yml
├── composite-service.yml
├── date-service.yml
├── time-service.yml
└── gateway-service.yml
```

The most of these files are simple and similar to each because all common parts have been included in _application.yml_. Below you can see the content of _date-service.yml_.

```
server.port: 9001
spring.application.name: date-service

---
spring.config.activate.on-profile: docker
server.port: 8080
```

### Server code

### Docker configuration

```
  config:
    build: config-server-end
    mem_limit: 512m
    environment:
      - SPRING_PROFILES_ACTIVE=docker,native
      - ENCRYPT_KEY=${CONFIG_SERVER_ENCRYPT_KEY}
      - SPRING_SECURITY_USER_NAME=${CONFIG_SERVER_USR}
      - SPRING_SECURITY_USER_PASSWORD=${CONFIG_SERVER_PWD}
    volumes:
      - /home/nicolab/IdeaProjects/learn-microservices/code/spring-cloud-config-end/config-repo:/config-repo

```

Here are the explanations for the preceding code:
* The Spring profile _native_ is added to signal to the config server that the config repository is based on local files.
* The environment variable _ENCRYPT_KEY_ is used to specify the symmetric encryption key that will be used by the config server to encrypt and decrypt sensitive configuration information.
* The environment variables _CONFIG_SERVER_USR_ and _CONFIG_SERVER_PWD_ are used to specify the credentials to be used for protecting the APIs using basic HTTP authentication.
* The volumes declaration will make the config-repo folder accessible in the Docker container at /config-repo.

The values of the three preceding environment variables, marked in the Docker Compose file with ${...}, are fetched by Docker Compose from the .env file:

```
CONFIG_SERVER_ENCRYPT_KEY=ninna-nanna-ninna-0h
CONFIG_SERVER_USR=user
CONFIG_SERVER_PWD=secret
```

These environmental variables can be injected in IntelliJ Configurations using third party plugins such as [EnvFile](https://github.com/ashald/EnvFile).


### Trying out Spring Cloud Config Server

**Configuration retrieval** Configurations can be retrieved using the */service/profile* endpoint exposed by the config server.
For example, you can use the following command to retrieve the _time-service_ configuration for the docker profile. You can test all the other combinations by changing the name either of the *service* or of the *profile*.

```
$ curl http://user:secret@localhost:8888/time-service/docker | jq

{
  "name": "time-service",
  "profiles": [
    "docker"
  ],
  "label": null,
  "version": null,
  "state": null,
  "propertySources": [
    {
      "name": "Config resource 'file [/Users/nicola/IdeaProjects/learn-spring-boot/code/learn-spring-m6/spring-cloud-config-end/config-repo/time-service.yml]' via location 'file:/Users/nicola/IdeaProjects/learn-spring-boot/code/learn-spring-m6/spring-cloud-config-end/config-repo/' (document #1)",
      "source": {
        "spring.config.activate.on-profile": "docker",
        "server.port": 8080
      }
    },
    ...
```

The response contains properties from a number of property sources, one per property file and Spring profile that matched the API request. The property sources are returned in priority order; if a property is specified in multiple property sources, the first property in the response takes precedence.

**Encryption** Information can be encrypted and decrypted using the /encrypt and /decrypt endpoints exposed by the config server. 

```
curl http://user:secret@localhost:8888/encrypt -d my-super-secure-password
4d28a7cb6eb9976dbeae0eb1cc0cb05672f01789140394fe4d93069d3622ab10a25ba9cf2b4ae3fcbc566dfb6cf13403%   
```

```
curl http://user:secret@localhost:8888/decrypt -d 4d28a7cb6eb9976dbeae0eb1cc0cb05672f01789140394fe4d93069d3622ab10a25ba9cf2b4ae3fcbc566dfb6cf13403
my-super-secure-password%    
```

If you want to use an encrypted value in a configuration file, you need to prefix it with {cipher} and wrap it in ''. For example, to store the encrypted version of 'my-super-secure-password', add the following line in a YAML-based configuration file:
```
secret-password: '{cipher}4d28a7cb6eb9976dbeae0eb1cc0cb05672f01789140394fe4d93069d3622ab10a25ba9cf2b4ae3fcbc566dfb6cf13403'
```

When the config server detects values in the format '{cipher}...', it tries to decrypt them using its encryption key before sending them to a client.

## Setting up Spring Cloud Config Clients

### Maven dependencies
```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-client</artifactId>
    </dependency>
```

### Configuration
* Move the configuration file, _application.yml_, to the config repository and rename it with the name of the client as specified by the property _spring.application.name_. 
* Add a new _application.yml_ file to the _src/main/resources_ folder. This file will be used to hold the configuration required to connect to the config server. 

```
spring:
  application:
    name: time-service
  config:
    import: optional:configserver:http://${CONFIG_SERVER_HOST}:${CONFIG_SERVER_PORT}
  cloud.config:
    failFast: true
    retry:
      initialInterval: 3000
      multiplier: 1.3
      maxInterval: 10000
      maxAttempts: 20
    username: ${CONFIG_SERVER_USR}
    password: ${CONFIG_SERVER_PWD}
```

This configuration will make the client do the following:
* Connect to the config server using the http://localhost:8888 URL when it runs outside Docker, and using the http://config:8888 URL when running in a Docker container
* Use HTTP Basic authentication, based on the value of the CONFIG_SERVER_USR and CONFIG_SERVER_PWD properties, as the client’s username and password
* Try to reconnect to the config server during startup up to 20 times, if required. If the connection attempt fails, the client will initially wait for 3 seconds before trying to reconnect. The wait time for subsequent retries will increase by a factor of 1.3. The maximum wait time between connection attempts will be 10 seconds. If the client can’t connect to the config server after 20 attempts, its startup will fail
   
This configuration is generally good for resilience against temporary connectivity problems with the config server. It is especially useful when the whole landscape of microservices and its config server are started up at once, for example, when using the docker-compose up command. In this scenario, many of the clients will be trying to connect to the config server before it is ready, and the retry logic will make the clients connect to the config server successfully once it is up and running.

## Setting up alternative backends
TODO

## Resources
- Microservices with Spring Boot 3 and Spring Cloud (Chapter 12)
- https://docs.spring.io/spring-cloud-config/reference/server.html


