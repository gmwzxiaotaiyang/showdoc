### 创建服务提供者

添加依赖
```xml
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
```

配置 application.yml
```yml

spring:
  application:
    name: server1
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.9.129:8848

```

通过 Spring Cloud 原生注解 @EnableDiscoveryClient 开启服务注册发现功能：

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Server1Application {

    public static void main(String[] args) {
        SpringApplication.run(Server1Application.class, args);
    }
}
```

### 配置服务消费者

添加依赖
```xml

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
```

配置 application.yml
```yml

spring:
  application:
    name: server2
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.9.129:8848

```

通过 Spring Cloud 原生注解 @EnableDiscoveryClient 开启服务注册发现功能  EnableFeignClients 启用 Feign

```java
@SpringBootApplication
@EnableFeignClients
@EnableDiscoveryClient
public class Server2Application {
    public static void main(String[] args) {
        SpringApplication.run(Server2Application.class, args);
    }

}
```

创建 FeignClient
```java
@FeignClient(value = "server1")
public interface Server1Client {
    @GetMapping("/server1/hello/{name}")
    String hello(@PathVariable("name") String name);
}

```

调用提供者服务
```java 
  @RestController
    public class TestController {

        @Resource
        private Server1Client server1Client;

        @RequestMapping(value = "/hello/{str}", method = RequestMethod.GET)
        public String echo(@PathVariable String str) {
            return server1Client.hello(str);
        }
    }
```