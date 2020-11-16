* [SpringCloud 整合 Zookeeper 代替 Eureka](#springcloud-%E6%95%B4%E5%90%88-zookeeper-%E4%BB%A3%E6%9B%BF-eureka)
  * [一、注册中心 Zookeeper](#%E4%B8%80%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83-zookeeper)
    * [1、安装 zookeeper 作为注册中心](#1%E5%AE%89%E8%A3%85-zookeeper-%E4%BD%9C%E4%B8%BA%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83)
      * [1）拉取 zookeeper](#1%E6%8B%89%E5%8F%96-zookeeper)
      * [2）第一次启动 zookeeper](#2%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%90%AF%E5%8A%A8-zookeeper)
      * [3）重启 zookeeper](#3%E9%87%8D%E5%90%AF-zookeeper)
  * [二、服务提供者](#%E4%BA%8C%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85)
    * [1、cloud\-provider\-payment8004](#1cloud-provider-payment8004)
    * [2、pom\.xml](#2pomxml)
    * [3、application\.yml](#3applicationyml)
    * [4、主启动类](#4%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
    * [5、controller](#5controller)
    * [6、验证测试](#6%E9%AA%8C%E8%AF%81%E6%B5%8B%E8%AF%95)
      * [1）访问](#1%E8%AE%BF%E9%97%AE)
      * [2）zookeeper 服务器](#2zookeeper-%E6%9C%8D%E5%8A%A1%E5%99%A8)
  * [三、服务消费者](#%E4%B8%89%E6%9C%8D%E5%8A%A1%E6%B6%88%E8%B4%B9%E8%80%85)
    * [1、cloud\-consumerzk\-order8000](#1cloud-consumerzk-order8000)
    * [2、pom\.xml](#2pomxml-1)
    * [3、application\.yml](#3applicationyml-1)
    * [4、主启动类](#4%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
    * [5、业务类](#5%E4%B8%9A%E5%8A%A1%E7%B1%BB)
      * [1）config](#1config)
      * [2）controller](#2controller)
    * [6、验证测试](#6%E9%AA%8C%E8%AF%81%E6%B5%8B%E8%AF%95-1)
      * [1）访问](#1%E8%AE%BF%E9%97%AE-1)
      * [2）zookeeper 服务器](#2zookeeper-%E6%9C%8D%E5%8A%A1%E5%99%A8-1)

# SpringCloud 整合 Zookeeper 代替 Eureka

## 一、注册中心 Zookeeper

### 1、安装 zookeeper 作为注册中心

#### 1）拉取 zookeeper

```shell
docker pull zookeeper
```

**记得关闭 linux 防火墙**

#### 2）第一次启动 zookeeper

```shell
docker run --name zk01 -p 2181:2181 --restart always -d zookeeper
```

#### 3）重启 zookeeper

```java
docker restart zk01
```

**zookeeper 服务器取代 Eureka 服务器，zookeeper 作为服务注册中心**

## 二、服务提供者

### 1、cloud-provider-payment8004

### 2、pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.lhq.cloud2020</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8004</artifactId>
    <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>




        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

### 3、application.yml

```yaml
server:
  port: 8004

spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.183.101:2181
```

### 4、主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8004 {
    public static void main(String args[]){
        SpringApplication.run(PaymentMain8004.class,args);
    }
}
```

### 5、controller

```java
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;
    @RequestMapping(value = "/payment/zk")
    public String paymentzk(){
        return "spring cloud with zookeeper:"+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

### 6、验证测试

#### 1）访问

http://localhost:8004/payment/zk

#### 2）zookeeper 服务器

![image-20201111155825938](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201116191353.png)

**服务节点为临时节点**

## 三、服务消费者

### 1、cloud-consumerzk-order8000

### 2、pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.lhq.cloud2020</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumerzk-order8000</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>




        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

### 3、application.yml

```yml
server:
  port: 8000

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 192.168.183.101:2181
```

### 4、主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class OrderZKMain8000 {
    public static void main(String args[]){
        SpringApplication.run(OrderZKMain8000.class,args);
    }
}
```

### 5、业务类

#### 1）config

```java
@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

#### 2）controller

```java
package com.lhq.cloud2020.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-11 15:34
 */
@RestController
@Slf4j
public class OrderZKController {

    public static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/zk")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/zk", String.class);
        return result;
    }
}
```

### 6、验证测试

#### 1）访问

http://localhost:8004/payment/zk

http://localhost:8000/consumer/payment/zk

#### 2）zookeeper 服务器

![image-20201111160441210](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201116191359.png)