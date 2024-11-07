
引入依赖

```xml

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
        </dependency>

```

创建 bootstrap.yml
```yml

spring:
  application:
    name: config
  cloud:
    nacos:
      config:
        server-addr: 192.168.9.129:8848
        file-extension: yaml

```