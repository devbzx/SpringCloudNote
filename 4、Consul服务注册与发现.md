* [Consul 服务注册与发现](#consul-%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%8E%E5%8F%91%E7%8E%B0)
  * [一、consul 简介](#%E4%B8%80consul-%E7%AE%80%E4%BB%8B)
    * [1、什么是 consul](#1%E4%BB%80%E4%B9%88%E6%98%AF-consul)
    * [2、consul 能干嘛](#2consul-%E8%83%BD%E5%B9%B2%E5%98%9B)
      * [1）服务发现](#1%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
      * [2）健康监测](#2%E5%81%A5%E5%BA%B7%E7%9B%91%E6%B5%8B)
      * [3）K、V 存储](#3kv-%E5%AD%98%E5%82%A8)
      * [4）多数据中心](#4%E5%A4%9A%E6%95%B0%E6%8D%AE%E4%B8%AD%E5%BF%83)
      * [5）可视化 web 界面](#5%E5%8F%AF%E8%A7%86%E5%8C%96-web-%E7%95%8C%E9%9D%A2)
    * [3、官网](#3%E5%AE%98%E7%BD%91)
      * [2）docker 安装](#2docker-%E5%AE%89%E8%A3%85)
  * [二、安装并运行 consul](#%E4%BA%8C%E5%AE%89%E8%A3%85%E5%B9%B6%E8%BF%90%E8%A1%8C-consul)
    * [1、consul\.exe 安装](#1consulexe-%E5%AE%89%E8%A3%85)
      * [1）官网安装说明](#1%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E)
      * [2）下载地址](#2%E4%B8%8B%E8%BD%BD%E5%9C%B0%E5%9D%80)
      * [3）运行](#3%E8%BF%90%E8%A1%8C)
      * [4）查看信息](#4%E6%9F%A5%E7%9C%8B%E4%BF%A1%E6%81%AF)
    * [2、docker 安装](#2docker-%E5%AE%89%E8%A3%85-1)
      * [1）拉取镜像](#1%E6%8B%89%E5%8F%96%E9%95%9C%E5%83%8F)
      * [2）启动 consul](#2%E5%90%AF%E5%8A%A8-consul)
      * [3）访问 consul 首页](#3%E8%AE%BF%E9%97%AE-consul-%E9%A6%96%E9%A1%B5)
  * [三、编写代码](#%E4%B8%89%E7%BC%96%E5%86%99%E4%BB%A3%E7%A0%81)
    * [1、服务提供者](#1%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85)
      * [1）新建 module 支付服务 provider8006](#1%E6%96%B0%E5%BB%BA-module-%E6%94%AF%E4%BB%98%E6%9C%8D%E5%8A%A1-provider8006)
      * [2）pom\.xml](#2pomxml)
      * [3）application\.yml](#3applicationyml)
      * [4）主启动类](#4%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [5）业务类 Controller](#5%E4%B8%9A%E5%8A%A1%E7%B1%BB-controller)
      * [6）验证](#6%E9%AA%8C%E8%AF%81)
    * [2、服务消费者](#2%E6%9C%8D%E5%8A%A1%E6%B6%88%E8%B4%B9%E8%80%85)
      * [1）新建 module 服务 order8000](#1%E6%96%B0%E5%BB%BA-module-%E6%9C%8D%E5%8A%A1-order8000)
      * [2）pom\.xml](#2pomxml-1)
      * [3）application\.yml](#3applicationyml-1)
      * [4）主启动类](#4%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
      * [5）config](#5config)
      * [6）controller](#6controller)
      * [7）验证测试](#7%E9%AA%8C%E8%AF%81%E6%B5%8B%E8%AF%95)
  * [四、三个注册中心异同点](#%E5%9B%9B%E4%B8%89%E4%B8%AA%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83%E5%BC%82%E5%90%8C%E7%82%B9)
    * [1、CAP](#1cap)
    * [2、Eureka](#2eureka)
    * [3、zookeeper、consul](#3zookeeperconsul)

# Consul 服务注册与发现

## 一、consul 简介

### 1、什么是 consul

*Consul is a service mesh solution providing a full featured control plane with service discovery, configuration, and segmentation functionality. Each of these features can be used individually as needed, or they can be used together to build a full service mesh. Consul requires a data plane and supports both a proxy and native integration model. Consul ships with a simple built-in proxy so that everything works out of the box, but also supports 3rd party proxy integrations such as Envoy.*

Consul 是一种服务网格解决方案，提供具有服务发现、配置和分段功能的全功能控制平面。

每个功能都可以根据需要单独使用，也可以一起使用以构建完整的服务网格。

Consul 需要数据平面，同时支持代理和本机集成模型。

### 2、consul 能干嘛

#### 1）服务发现

提供 HTTP 和 DNS 两种发现方式。

#### 2）健康监测

支持多种协议，HTTP、TCP、Docker、Shell 脚本定制化。

#### 3）K、V 存储

key , Value的存储方式

#### 4）多数据中心

Consul支持多数据中心

#### 5）可视化 web 界面

![image-20201116095245143](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201116095245143.png)

### 3、官网

https://www.consul.io

#### 2）docker 安装

```shell
docker pull consul
```

## 二、安装并运行 consul

### 1、consul.exe 安装

#### 1）官网安装说明

https://learn.hashicorp.com/consul/getting-started/install.html

#### 2）下载地址

https://www.consul.io/downloads.html

#### 3）运行

下载完成后有一个 consul.exe 文件，双击运行即可

#### 4）查看信息

查看版本信息

```shell
consul --version
```

使用开发模式启动

```shell
consul agent -dev
```

访问 consul 首页

http://localhost:8500

### 2、docker 安装

#### 1）拉取镜像

```shell
docker pull consul
```

#### 2）启动 consul

```shell
docker run --name consul -p 8500:8500 -d consul
```

#### 3）访问 consul 首页

http://192.168.183.101:8500

## 三、编写代码

### 1、服务提供者

#### 1）新建 module 支付服务 provider8006

cloud-providerconsul-payment8006

#### 2）pom.xml

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

    <artifactId>cloud-providerconsul-payment8006</artifactId>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-consul-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.lhq.cloud2020</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
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

#### 3）application.yml

```yaml
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment
  cloud:
    consul:
      host: 192.168.183.101
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

#### 4）主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-13 11:03
 */
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8006 {
    public static void main(String[] args){
        SpringApplication.run(PaymentMain8006.class,args);
    }
}
```

#### 5）业务类 Controller

```java
package com.lhq.cloud2020.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-13 11:03
 */
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;
    @RequestMapping(value = "/payment/consul")
    public String paymentConsul(){
        return "spring cloud with zookeeper:"+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

#### 6）验证

访问 http://localhost:8006/payment/consul

![image-20201116101546723](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201116101546723.png)

访问 http://192.168.183.101:8500

![image-20201116101632164](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201116101632164.png)

### 2、服务消费者

#### 1）新建 module 服务 order8000

cloud-consumerconsul-order8000

#### 2）pom.xml

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

    <artifactId>cloud-consumerconsul-order8000</artifactId>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-consul-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>com.lhq.cloud2020</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
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

#### 3）application.yml

```yaml
server:
  port: 8000

spring:
  application:
    name: consul-consumer-order
  cloud:
    consul:
      host: 192.168.183.101
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

#### 4）主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-13 11:11
 */
@SpringBootApplication
@EnableDiscoveryClient
public class OrderConsulMain8000 {
    public static void main(String[] args) {
        SpringApplication.run(OrderConsulMain8000.class,args);
    }
}
```

#### 5）config

```java
package com.lhq.cloud2020.config;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-13 11:13
 */

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

#### 6）controller

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
 * @since 2020-11-13 11:15
 */
@RestController
@Slf4j
public class OrderConsulController {

    public static final String INVOME_URL  = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/consul")
    public String payment(){
        String result = restTemplate.getForObject(INVOME_URL + "/payment/consul", String.class);
        return result;
    }

}
```

#### 7）验证测试

访问 consul 地址

http://localhost:8006/payment/consul

![image-20201116102256848](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201116102256848.png)

访问测试地址

http://localhost:8000/consumer/payment/consul

![image-20201116102435699](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201116102435699.png)

## 四、三个注册中心异同点

### 1、CAP

C：Consistency（强一致性）

A：Availability（可用性）

P：Partition tolerance（分区容错）

CAP 理论关注粒度是数据，而不是整体系统设计的策略

### 2、Eureka

AP 强调可用性和容错性

### 3、zookeeper、consul

CP 强调强一致性和容错性

