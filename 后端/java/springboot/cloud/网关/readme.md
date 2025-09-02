
引入依赖
```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
```

application.yml 

```yml
server:
  port: 10009

spring:
  application:
    name: getway
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.9.129:8848
    gateway:
      routes:
        - id: server1
          uri: lb://server1
          predicates:
            - Path=/server1/**
        - id: server2
          uri: lb://server2
          predicates:
            - Path=/server2/**
```