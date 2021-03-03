* [SpringCloud Alibaba Nacos 服务注册和配置中心](#springcloud-alibaba-nacos-%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E5%92%8C%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83)
  * [一、Nacos 简介](#%E4%B8%80nacos-%E7%AE%80%E4%BB%8B)
    * [1、为什么叫 Nacos](#1%E4%B8%BA%E4%BB%80%E4%B9%88%E5%8F%AB-nacos)
    * [2、是什么](#2%E6%98%AF%E4%BB%80%E4%B9%88)
    * [3、能干嘛](#3%E8%83%BD%E5%B9%B2%E5%98%9B)
    * [4、去哪下](#4%E5%8E%BB%E5%93%AA%E4%B8%8B)
      * [官方文档](#%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3)
    * [5、各种注册中心比较](#5%E5%90%84%E7%A7%8D%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83%E6%AF%94%E8%BE%83)
  * [二、安装并运行 Nacos](#%E4%BA%8C%E5%AE%89%E8%A3%85%E5%B9%B6%E8%BF%90%E8%A1%8C-nacos)
    * [1、本地环境](#1%E6%9C%AC%E5%9C%B0%E7%8E%AF%E5%A2%83)
    * [2、下载 Nacos](#2%E4%B8%8B%E8%BD%BD-nacos)
    * [3、解压](#3%E8%A7%A3%E5%8E%8B)
    * [4、访问](#4%E8%AE%BF%E9%97%AE)
    * [5、结果页面](#5%E7%BB%93%E6%9E%9C%E9%A1%B5%E9%9D%A2)
  * [三、Nacos 作为服务注册中心演示](#%E4%B8%89nacos-%E4%BD%9C%E4%B8%BA%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83%E6%BC%94%E7%A4%BA)
    * [1、官方文档](#1%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3)
    * [2、基于 Nacos 的服务提供者](#2%E5%9F%BA%E4%BA%8E-nacos-%E7%9A%84%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85)
      * [1）新建 cloudalibaba\-provider\-payment9001](#1%E6%96%B0%E5%BB%BA-cloudalibaba-provider-payment9001)
        * [1、pom\.xml](#1pomxml)
          * [父 pom](#%E7%88%B6-pom)
          * [本 pom](#%E6%9C%AC-pom)
        * [2、application\.yml](#2applicationyml)
        * [3、主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
        * [4、控制层](#4%E6%8E%A7%E5%88%B6%E5%B1%82)
        * [5、测试](#5%E6%B5%8B%E8%AF%95)
          * [访问](#%E8%AE%BF%E9%97%AE)
          * [nacos 控制台](#nacos-%E6%8E%A7%E5%88%B6%E5%8F%B0)
      * [2）以 9001 为模板创建 9002](#2%E4%BB%A5-9001-%E4%B8%BA%E6%A8%A1%E6%9D%BF%E5%88%9B%E5%BB%BA-9002)
      * [3）新建 cloudalibaba\-consumer\-nacos\-order83](#3%E6%96%B0%E5%BB%BA-cloudalibaba-consumer-nacos-order83)
        * [1、pom\.xml](#1pomxml-1)
        * [2、application\.yml](#2applicationyml-1)
        * [3、主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
        * [4、配置类](#4%E9%85%8D%E7%BD%AE%E7%B1%BB)
        * [5、控制类](#5%E6%8E%A7%E5%88%B6%E7%B1%BB)
      * [4）开启 nacos、9001、9002](#4%E5%BC%80%E5%90%AF-nacos90019002)
        * [1、nacos 控制台](#1nacos-%E6%8E%A7%E5%88%B6%E5%8F%B0)
        * [2、访问](#2%E8%AE%BF%E9%97%AE)
      * [5）服务注册中心对比](#5%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83%E5%AF%B9%E6%AF%94)
        * [1、Nacos 全景图](#1nacos-%E5%85%A8%E6%99%AF%E5%9B%BE)
        * [2、Nacos 和 CAP](#2nacos-%E5%92%8C-cap)
        * [3、切换](#3%E5%88%87%E6%8D%A2)
  * [四、Nacos 作为服务配置中心演示](#%E5%9B%9Bnacos-%E4%BD%9C%E4%B8%BA%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83%E6%BC%94%E7%A4%BA)
    * [1、Nacos 作为配置中心\-基础配置](#1nacos-%E4%BD%9C%E4%B8%BA%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83-%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE)
      * [1）新建 cloudalibaba\-config\-nacos\-client3377](#1%E6%96%B0%E5%BB%BA-cloudalibaba-config-nacos-client3377)
        * [1、pom\.xml](#1pomxml-2)
        * [2、yml](#2yml)
          * [为什么配置两个？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%85%8D%E7%BD%AE%E4%B8%A4%E4%B8%AA)
          * [bootstrap\.yml](#bootstrapyml)
          * [application\.yml](#applicationyml)
        * [3、主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-2)
        * [4、业务类](#4%E4%B8%9A%E5%8A%A1%E7%B1%BB)
        * [5、在 nacos 中添加配置信息](#5%E5%9C%A8-nacos-%E4%B8%AD%E6%B7%BB%E5%8A%A0%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF)
          * [Nacos 中的匹配规则](#nacos-%E4%B8%AD%E7%9A%84%E5%8C%B9%E9%85%8D%E8%A7%84%E5%88%99)
        * [6、测试](#6%E6%B5%8B%E8%AF%95)
          * [启动前](#%E5%90%AF%E5%8A%A8%E5%89%8D)
          * [运行](#%E8%BF%90%E8%A1%8C)
          * [修改配置](#%E4%BF%AE%E6%94%B9%E9%85%8D%E7%BD%AE)
    * [2、Nacos 作为配置中心\-分类配置](#2nacos-%E4%BD%9C%E4%B8%BA%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83-%E5%88%86%E7%B1%BB%E9%85%8D%E7%BD%AE)
      * [1）问题](#1%E9%97%AE%E9%A2%98)
      * [2）Nacos 的图形化管理界面](#2nacos-%E7%9A%84%E5%9B%BE%E5%BD%A2%E5%8C%96%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A2)
        * [1、配置管理](#1%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86)
        * [2、命名空间](#2%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)
        * [3、Namespace \+ Group \+ Data ID 三者关系？为什么这样设计？](#3namespace--group--data-id-%E4%B8%89%E8%80%85%E5%85%B3%E7%B3%BB%E4%B8%BA%E4%BB%80%E4%B9%88%E8%BF%99%E6%A0%B7%E8%AE%BE%E8%AE%A1)
          * [是什么](#%E6%98%AF%E4%BB%80%E4%B9%88)
          * [三者情况](#%E4%B8%89%E8%80%85%E6%83%85%E5%86%B5)
          * [默认情况](#%E9%BB%98%E8%AE%A4%E6%83%85%E5%86%B5)
      * [3）Case](#3case)
        * [1、DataID 方案](#1dataid-%E6%96%B9%E6%A1%88)
          * [新建 dev 配置 DataID](#%E6%96%B0%E5%BB%BA-dev-%E9%85%8D%E7%BD%AE-dataid)
          * [新建 test 配置 DataID](#%E6%96%B0%E5%BB%BA-test-%E9%85%8D%E7%BD%AE-dataid)
        * [2、Group 方案](#2group-%E6%96%B9%E6%A1%88)
        * [3、Namespace 方案](#3namespace-%E6%96%B9%E6%A1%88)
  * [五、Nacos 集群和持久化配置](#%E4%BA%94nacos-%E9%9B%86%E7%BE%A4%E5%92%8C%E6%8C%81%E4%B9%85%E5%8C%96%E9%85%8D%E7%BD%AE)
    * [1、官网说明](#1%E5%AE%98%E7%BD%91%E8%AF%B4%E6%98%8E)
      * [1）官网架构图](#1%E5%AE%98%E7%BD%91%E6%9E%B6%E6%9E%84%E5%9B%BE)
      * [2）解释](#2%E8%A7%A3%E9%87%8A)
      * [3）说明](#3%E8%AF%B4%E6%98%8E)
        * [Nacos支持三种部署模式](#nacos%E6%94%AF%E6%8C%81%E4%B8%89%E7%A7%8D%E9%83%A8%E7%BD%B2%E6%A8%A1%E5%BC%8F)
        * [单机模式支持mysql](#%E5%8D%95%E6%9C%BA%E6%A8%A1%E5%BC%8F%E6%94%AF%E6%8C%81mysql)
    * [2、Nacos 持久化配置解释](#2nacos-%E6%8C%81%E4%B9%85%E5%8C%96%E9%85%8D%E7%BD%AE%E8%A7%A3%E9%87%8A)
      * [1）Nacos 默认数据库](#1nacos-%E9%BB%98%E8%AE%A4%E6%95%B0%E6%8D%AE%E5%BA%93)
      * [2）如何更改数据库为 mysql](#2%E5%A6%82%E4%BD%95%E6%9B%B4%E6%94%B9%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%BA-mysql)
      * [3）测试](#3%E6%B5%8B%E8%AF%95)
    * [3、配置集群](#3%E9%85%8D%E7%BD%AE%E9%9B%86%E7%BE%A4)
      * [1）上传  <a href="https://github\.com/alibaba/nacos/releases/download/1\.4\.1/nacos\-server\-1\.4\.1\.tar\.gz">nacos\-server\-1\.4\.1\.tar\.gz</a> 到服务器](#1%E4%B8%8A%E4%BC%A0--nacos-server-141targz-%E5%88%B0%E6%9C%8D%E5%8A%A1%E5%99%A8)
      * [2）解压此压缩包](#2%E8%A7%A3%E5%8E%8B%E6%AD%A4%E5%8E%8B%E7%BC%A9%E5%8C%85)
      * [3）修改 application\.properties](#3%E4%BF%AE%E6%94%B9-applicationproperties)
      * [4）修改 cluster\.conf](#4%E4%BF%AE%E6%94%B9-clusterconf)
      * [5）编辑 start\.sh 脚本](#5%E7%BC%96%E8%BE%91-startsh-%E8%84%9A%E6%9C%AC)
        * [执行方式](#%E6%89%A7%E8%A1%8C%E6%96%B9%E5%BC%8F)
      * [6）Nginx 配置，由它作为负载均衡器](#6nginx-%E9%85%8D%E7%BD%AE%E7%94%B1%E5%AE%83%E4%BD%9C%E4%B8%BA%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E5%99%A8)
        * [启动 nginx](#%E5%90%AF%E5%8A%A8-nginx)
      * [7）测试](#7%E6%B5%8B%E8%AF%95)
        * [访问](#%E8%AE%BF%E9%97%AE-1)
        * [将微服务启动注册到集群](#%E5%B0%86%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%90%AF%E5%8A%A8%E6%B3%A8%E5%86%8C%E5%88%B0%E9%9B%86%E7%BE%A4)
          * [yml](#yml)
          * [查看注册中心](#%E6%9F%A5%E7%9C%8B%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83)
    * [4、总结](#4%E6%80%BB%E7%BB%93)

# SpringCloud Alibaba Nacos 服务注册和配置中心

## 一、Nacos 简介

### 1、为什么叫 Nacos

前四个字母分别为 Naming 和 Configuration 的前两个字母，最后 s 为 Service

### 2、是什么

一个更易于构建云原生应用的动态服务发现，配置管理和服务管理中心

Nacos：Dynamic Naming and Configuration Service

Nacos 就是注册中心 + 配置中心的组合

Nacos = Eureka + Config + Bus

### 3、能干嘛

替代 Eureka 做服务注册中心

替代 Config 做服务配置中心

### 4、去哪下

https://github.com/alibaba/Nacos

#### 官方文档

https://nacos.io/zh-cn/index.html

### 5、各种注册中心比较

![image-20210301145810189](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301145810.png)

据说 Nacos 在阿里巴巴内部有超过 10 万的实例运行，已经过了类似双十一等各种大型流量的考验

## 二、安装并运行 Nacos

### 1、本地环境

Java 8 + Maven 

### 2、下载 Nacos

https://github.com/alibaba/nacos/releases/download/1.1.4/nacos-server-1.1.4.zip

### 3、解压

直接运行 bin 下的 startup.cmd

### 4、访问

http://localhost:8848/nacos

账号 nacos

密码 nacos

### 5、结果页面

![image-20210301151849798](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301151849.png)

![image-20210301151934267](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301151934.png)

## 三、Nacos 作为服务注册中心演示

### 1、官方文档

https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html

### 2、基于 Nacos 的服务提供者

#### 1）新建 cloudalibaba-provider-payment9001

##### 1、pom.xml

###### 父 pom

```xml
<!--spring cloud alibaba 2.1.0.RELEASE-->
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-alibaba-dependencies</artifactId>
  <version>2.1.0.RELEASE</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```

###### 本 pom

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

    <artifactId>cloudalibaba-provider-payment9001</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
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
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

##### 2、application.yml

```yml
server:
  port: 9001
spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置 Nacos 地址
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

##### 3、主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-01 15:42
 */
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain9001 {
    public static void main(String[] args){
        SpringApplication.run(PaymentMain9001.class,args);
    }
}
```

##### 4、控制层

```java
package com.lhq.cloud2020.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-01 15:43
 */
@RestController
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id) {
        return "nacos registry，serverPort：" + serverPort + "\t id" + id;
    }
}
```

##### 5、测试

###### 访问

http://localhost:9001/payment/nacos/1

###### nacos 控制台

![image-20210301171102465](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301171102.png)

#### 2）以 9001 为模板创建 9002

#### 3）新建 cloudalibaba-consumer-nacos-order83

##### 1、pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
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
```

##### 2、application.yml

```yml
server:
  port: 83

spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848

# 消费者将要去访问的微服务名称（注册成功进 nacos 的微服务提供者）
service-url:
  nacos-user-service: http://nacos-payment-provider
```

##### 3、主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-02 10:10
 */
@EnableDiscoveryClient
@SpringBootApplication
public class OrderNacosMain83 {

    public static void main(String[] args){
        SpringApplication.run(OrderNacosMain83.class,args);
    }

}
```

##### 4、配置类

```java
package com.lhq.cloud2020.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-02 10:11
 */
@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

##### 5、控制类

```java
package com.lhq.cloud2020.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-02 10:12
 */
@RestController
@Slf4j
public class OrderNacosController {

    @Resource
    private RestTemplate restTemplate;

    @Value("${service-url.nacos-user-service}")
    private String serverUrl;

    @GetMapping(value = "/consumer/payment/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Long id) {
        return restTemplate.getForObject(serverUrl + "/payment/nacos/" + id,String.class);
    }
}
```

#### 4）开启 nacos、9001、9002

##### 1、nacos 控制台

![image-20210302103747758](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302103747.png)

##### 2、访问

多次访问

http://localhost:83/consumer/payment/nacos/13

观察

![image-20210302103838147](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302103838.png)

![image-20210302103850804](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302103904.png)

#### 5）服务注册中心对比

##### 1、Nacos 全景图

![nacos_landscape.png](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302104322.png)

##### 2、Nacos 和 CAP

##### 3、切换

**C 是所有节点在同一时间看到的数据是一致的；而 A 的定义是所有的请求都会收到响应**

何时选择使用何种模式？

一般来说，如果不需要存储服务级别的信息且服务实例是通过 nacos-client 注册，并且能够保持心跳上报，那么就可以选择 AP 模式。

当前主流的服务如 Spring Cloud 和 Dubbo 服务，都适用于 AP 模式，AP 模式为了服务的可能性而减弱了一致性，因此 AP 模式下只支持注册临时实例。

如果需要在服务级别编辑或者存储配置信息，那么 CP 是必须，K8S 服务和 DNS 服务则适用于 CP 模式。

CP 模式下则支持注册中心持久化实例，此时则是以 Raft 协议为集群运行模式，该模式下注册实例之前必须先注册服务，如果服务不存在，则会返回错误。

curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'

## 四、Nacos 作为服务配置中心演示

### 1、Nacos 作为配置中心-基础配置

#### 1）新建 cloudalibaba-config-nacos-client3377

##### 1、pom.xml

```xml
<dependencies>
<!--        nacos-config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-config</artifactId>
        </dependency>
<!--        nacos-discovery-cloud-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
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
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

##### 2、yml

bootstrap.yml

applicaiton.yml

###### 为什么配置两个？

Nacos 同 springcloud-config 一样，在项目初始化时，要保证先从配置中心进行配置拉取，拉取配置之后，才能保证项目的正常启动

SpringBoot 中配置文件的加载是存在优先级顺序的，bootstrap 优先级高于 application

###### bootstrap.yml

```yml
server:
  port: 3377

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # nacos 服务注册中心地址
      config:
        server-addr: localhost:8848 # nacos 作为配置中心地址
        file-extension: yaml #指定 yaml 格式的配置
```

###### application.yml

```yml
spring:
  profiles:
    active: dev # 表示开发环境
```

##### 3、主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-02 11:08
 */
@EnableDiscoveryClient
@SpringBootApplication
public class NacosConfigClientMain3377 {

    public static void main(String[] args){
        SpringApplication.run(NacosConfigClientMain3377.class,args);
    }

}
```

##### 4、业务类

```java
package com.lhq.cloud2020.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-03-02 11:09
 */
@RestController
@RefreshScope // 支持 nacos 的动态刷新
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/config/info")
    public String getConfigInfo(){
        return configInfo;
    }

}
```

`@RefreshScope`

实现配置自动更新

##### 5、在 nacos 中添加配置信息

###### Nacos 中的匹配规则

**理论**

Nacos 中的 dataid 的组成格式与 SpringBoot 配置文件的匹配规则

在 Nacos Spring Cloud 中，`dataId` 的完整格式如下：

```plain
${prefix}-${spring.profiles.active}.${file-extension}
```

- `prefix` 默认为 `spring.application.name` 的值，也可以通过配置项 `spring.cloud.nacos.config.prefix`来配置。
- `spring.profiles.active` 即为当前环境对应的 profile，详情可以参考 [Spring Boot文档](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html#boot-features-profiles)。 **注意：当 `spring.profiles.active` 为空时，对应的连接符 `-` 也将不存在，dataId 的拼接格式变成 `${prefix}.${file-extension}`**
- `file-exetension` 为配置内容的数据格式，可以通过配置项 `spring.cloud.nacos.config.file-extension` 来配置。目前只支持 `properties` 和 `yaml` 类型。

**实操**

Nacos 界面配置对应

![image-20210302113615976](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302113616.png)

**设置 DataId**

公式

${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}

总结

![image-20210302113931016](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302113931.png)

##### 6、测试

###### 启动前

启动前需要在 nacos 客户端-配置管理-配置列表栏目下查看有无对应的 yaml 配置文件

###### 运行

调用 http://localhost:3377/config/info

![image-20210302114412304](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302114412.png)

###### 修改配置

自带动态刷新

![image-20210302114512180](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302114512.png)

![image-20210302114521832](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302114521.png)



### 2、Nacos 作为配置中心-分类配置

#### 1）问题

多环境多项目管理

![image-20210302143555003](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302143555.png)

![image-20210302143606820](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302143606.png)

#### 2）Nacos 的图形化管理界面

##### 1、配置管理

![image-20210302143722509](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302143722.png)

##### 2、命名空间

![image-20210302143749777](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302143749.png)

##### 3、Namespace + Group + Data ID 三者关系？为什么这样设计？

###### 是什么

类似 Java 里面的 package 名和类名

最外层的 namespace 是可以用于区分部署环境的，Group 和 DataID 逻辑上区分两个目标对象

###### 三者情况

![image-20210302144048896](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302144048.png)

###### 默认情况

**Namespace = public，Group = DEFAULT_GROUP，默认 Cluster 是 DEFAULT**

Nacos 默认的命名空间是 public，Namespace 主要用来实现隔离

比方说我们现在有三个环境：开发、测试、生产环境，我们就可以创建三个 Namespace，不同的 Namespace 之间是隔离的

Group 默认是 DEFAULT_GROUP，Group 可以把不同的微服务划分到同一个分组里面去

Service 就是微服务；一个微服务可以包含多个 Cluster（集群），Nacos 默认 Cluster 是 DEFAULT，Cluster 是对指定微服务的一个虚拟划分

![image-20210302144622967](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302144623.png)

最后是 Instance，就是微服务的实例

#### 3）Case

##### 1、DataID 方案

指定 spring.profile.active 和配置文件的 DataID 来使不同环境下读取不同的配置

默认空间 + 默认分组 + 新建 dev 和 test 两个 DataID

###### 新建 dev 配置 DataID

![image-20210302150328677](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150328.png)

###### 新建 test 配置 DataID

![image-20210302150405145](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150405.png)

通过 spring.profile.active 属性就能进行多环境下配置文件的读取

![image-20210302150517824](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150517.png)

测试

http://localhost:3377/config/info

##### 2、Group 方案

通过 Group 实现环境区分

新建 Group

![image-20210302150749453](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150749.png)

在 nacos 图形界面控制台上面新建配置文件 DataID

![image-20210302150844884](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150844.png)

bootstrap + application

![image-20210302150917069](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302150917.png)



##### 3、Namespace 方案

新建 dev/test 的 Namespace

![image-20210302151021745](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302151021.png)

回到服务管理-服务列表查看

![image-20210302151051029](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302151051.png)

按照域名配置填写

![image-20210302151116667](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210302151116.png)

YML

bootstrap.yml

```yml
server:
  port: 3377

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # nacos 服务注册中心地址
      config:
        server-addr: localhost:8848 # nacos 作为配置中心地址
        file-extension: yaml #指定 yaml 格式的配置
        group: DEV_GROUP
        namespace: 60f1eadd-6994-414b-81ac-8f07f11986b7
```

applicaiton.yml

```yml
spring:
  profiles:
#    active: info
#    active: test
    active: dev # 表示开发环境
```

## 五、Nacos 集群和持久化配置

### 1、官网说明

https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html

#### 1）官网架构图

**集群部署架构图**

因此开源的时候推荐用户把所有服务列表放到一个vip下面，然后挂到一个域名下面

[http://ip1](http://ip1/):port/openAPI 直连ip模式，机器挂则需要修改ip才可以使用。

[http://SLB](http://slb/):port/openAPI 挂载SLB模式(内网SLB，不可暴露到公网，以免带来安全风险)，直连SLB即可，下面挂server真实ip，可读性不好。

[http://nacos.com](http://nacos.com/):port/openAPI 域名 + SLB模式(内网SLB，不可暴露到公网，以免带来安全风险)，可读性好，而且换ip方便，推荐模式

![deployDnsVipMode.jpg](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303145309.jpeg)

#### 2）解释        

​                               ![image-20210303145402157](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303145402.png)

​                                                               ![image-20210303145422935](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303145422.png)  

#### 3）说明

按照上图，我们需要一个 mysql 数据库

##### Nacos支持三种部署模式

- 单机模式 - 用于测试和单机试用。
- 集群模式 - 用于生产环境，确保高可用。
- 多集群模式 - 用于多数据中心场景。

##### 单机模式支持mysql

在0.7版本之前，在单机模式时nacos使用嵌入式数据库实现数据的存储，不方便观察数据存储的基本情况。

0.7版本增加了支持mysql数据源能力，具体的操作步骤：

- 1.安装数据库，版本要求：5.6.5+
- 2.初始化mysql数据库，数据库初始化文件：nacos-mysql.sql
- 3.修改conf/application.properties文件，增加支持mysql数据源配置（目前只支持mysql），添加mysql数据源的url、用户名和密码。

```properties
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://11.162.196.16:3306/nacos_devtest?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=nacos_devtest
db.password=youdontknow
```

再以单机模式启动nacos，nacos所有写嵌入式数据库的数据都写到了mysql

### 2、Nacos 持久化配置解释

#### 1）Nacos 默认数据库 

derby

#### 2）如何更改数据库为 mysql

- 在解压后的 nacos/conf 目录下找到 nacos-mysql.sql 脚本

  在 mysql 数据库中执行此脚本

- 在 nacos/conf 目录下找到 application.properties

  添加配置

  ```properties
  spring.datasource.platform=mysql
  
  db.num=1
  db.url.0=jdbc:mysql://11.162.196.16:3306/nacos_devtest?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
  db.user=nacos_devtest
  db.password=youdontknow
  ```

  如果数据库是 mysql8.0 的话，尽量选择新版 nacos，连接时需要加上时区

#### 3）测试

添加配置后，配置信息存入 mysql 中

![image-20210303152250265](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303152250.png)

### 3、配置集群

#### 1）上传  [nacos-server-1.4.1.tar.gz](https://github.com/alibaba/nacos/releases/download/1.4.1/nacos-server-1.4.1.tar.gz) 到服务器

路径 /opt/Software/

使用 xftp 或者 rz 命令

#### 2）解压此压缩包

```shell
tar -zxvf nacos-server-1.4.1.tar.gz
```

出现 nacos 文件夹

![image-20210303104056557](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303152629.png)

#### 3）修改 application.properties

nacos/conf 文件夹

在文档末添加

```properties
spring.datasource.platform=mysql

db.num=1

db.url.0=jdbc:mysql://172.23.15.52:3306/nacos-server?useSSL=false&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&characterEncoding=utf8&serverTimezone=UTC

db.user=root

db.password=root
```

完整版

```properties
#
# Copyright 1999-2018 Alibaba Group Holding Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

#*************** Spring Boot Related Configurations ***************#
### Default web context path:
server.servlet.contextPath=/nacos
### Default web server port:
server.port=8848

#*************** Network Related Configurations ***************#
### If prefer hostname over ip for Nacos server addresses in cluster.conf:
# nacos.inetutils.prefer-hostname-over-ip=false

### Specify local server's IP:
# nacos.inetutils.ip-address=


#*************** Config Module Related Configurations ***************#
### If use MySQL as datasource:
# spring.datasource.platform=mysql

### Count of DB:
# db.num=1

### Connect URL of DB:
# db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
# db.user.0=nacos
# db.password.0=nacos

### Connection pool configuration: hikariCP
db.pool.config.connectionTimeout=30000
db.pool.config.validationTimeout=10000
db.pool.config.maximumPoolSize=20
db.pool.config.minimumIdle=2

#*************** Naming Module Related Configurations ***************#
### Data dispatch task execution period in milliseconds:
# nacos.naming.distro.taskDispatchPeriod=200

### Data count of batch sync task:
# nacos.naming.distro.batchSyncKeyCount=1000

### Retry delay in milliseconds if sync task failed:
# nacos.naming.distro.syncRetryDelay=5000

### If enable data warmup. If set to false, the server would accept request without local data preparation:
# nacos.naming.data.warmup=true

### If enable the instance auto expiration, kind like of health check of instance:
# nacos.naming.expireInstance=true

nacos.naming.empty-service.auto-clean=true
nacos.naming.empty-service.clean.initial-delay-ms=50000
nacos.naming.empty-service.clean.period-time-ms=30000


#*************** CMDB Module Related Configurations ***************#
### The interval to dump external CMDB in seconds:
# nacos.cmdb.dumpTaskInterval=3600

### The interval of polling data change event in seconds:
# nacos.cmdb.eventTaskInterval=10

### The interval of loading labels in seconds:
# nacos.cmdb.labelTaskInterval=300

### If turn on data loading task:
# nacos.cmdb.loadDataAtStart=false


#*************** Metrics Related Configurations ***************#
### Metrics for prometheus
#management.endpoints.web.exposure.include=*

### Metrics for elastic search
management.metrics.export.elastic.enabled=false
#management.metrics.export.elastic.host=http://localhost:9200

### Metrics for influx
management.metrics.export.influx.enabled=false
#management.metrics.export.influx.db=springboot
#management.metrics.export.influx.uri=http://localhost:8086
#management.metrics.export.influx.auto-create-db=true
#management.metrics.export.influx.consistency=one
#management.metrics.export.influx.compressed=true


#*************** Access Log Related Configurations ***************#
### If turn on the access log:
server.tomcat.accesslog.enabled=true

### The access log pattern:
server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i %{Request-Source}i

### The directory of access log:
server.tomcat.basedir=


#*************** Access Control Related Configurations ***************#
### If enable spring security, this option is deprecated in 1.2.0:
#spring.security.enabled=false

### The ignore urls of auth, is deprecated in 1.2.0:
nacos.security.ignore.urls=/,/error,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-ui/public/**,/v1/auth/**,/v1/console/health/**,/actuator/**,/v1/console/server/**

### The auth system to use, currently only 'nacos' is supported:
nacos.core.auth.system.type=nacos

### If turn on auth system:
nacos.core.auth.enabled=false

### The token expiration in seconds:
nacos.core.auth.default.token.expire.seconds=18000

### The default token:
nacos.core.auth.default.token.secret.key=SecretKey012345678901234567890123456789012345678901234567890123456789

### Turn on/off caching of auth information. By turning on this switch, the update of auth information would have a 15 seconds delay.
nacos.core.auth.caching.enabled=true

### Since 1.4.1, Turn on/off white auth for user-agent: nacos-server, only for upgrade from old version.
nacos.core.auth.enable.userAgentAuthWhite=true

### Since 1.4.1, worked when nacos.core.auth.enabled=true and nacos.core.auth.enable.userAgentAuthWhite=false.
### The two properties is the white list for auth and used by identity the request from other server.
nacos.core.auth.server.identity.key=
nacos.core.auth.server.identity.value=

#*************** Istio Related Configurations ***************#
### If turn on the MCP server:
nacos.istio.mcp.server.enabled=false



###*************** Add from 1.3.0 ***************###


#*************** Core Related Configurations ***************#

### set the WorkerID manually
# nacos.core.snowflake.worker-id=

### Member-MetaData
# nacos.core.member.meta.site=
# nacos.core.member.meta.adweight=
# nacos.core.member.meta.weight=

### MemberLookup
### Addressing pattern category, If set, the priority is highest
# nacos.core.member.lookup.type=[file,address-server]
## Set the cluster list with a configuration file or command-line argument
# nacos.member.list=192.168.16.101:8847?raft_port=8807,192.168.16.101?raft_port=8808,192.168.16.101:8849?raft_port=8809
## for AddressServerMemberLookup
# Maximum number of retries to query the address server upon initialization
# nacos.core.address-server.retry=5
## Server domain name address of [address-server] mode
# address.server.domain=jmenv.tbsite.net
## Server port of [address-server] mode
# address.server.port=8080
## Request address of [address-server] mode
# address.server.url=/nacos/serverlist

#*************** JRaft Related Configurations ***************#

### Sets the Raft cluster election timeout, default value is 5 second
# nacos.core.protocol.raft.data.election_timeout_ms=5000
### Sets the amount of time the Raft snapshot will execute periodically, default is 30 minute
# nacos.core.protocol.raft.data.snapshot_interval_secs=30
### raft internal worker threads
# nacos.core.protocol.raft.data.core_thread_num=8
### Number of threads required for raft business request processing
# nacos.core.protocol.raft.data.cli_service_thread_num=4
### raft linear read strategy. Safe linear reads are used by default, that is, the Leader tenure is confirmed by heartbeat
# nacos.core.protocol.raft.data.read_index_type=ReadOnlySafe
### rpc request timeout, default 5 seconds
# nacos.core.protocol.raft.data.rpc_request_timeout_ms=5000

spring.datasource.platform=mysql

db.num=1

db.url.0=jdbc:mysql://192.168.183.101:3306/nacos-server?useSSL=false&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&characterEncoding=utf8&serverTimezone=UTC

db.user=root

db.password=root
```

#### 4）修改 cluster.conf

nacos/conf 下

由于是使用单机配置，所以用端口号区分不同的实例

添加如下信息

192.168.183.101:3333

192.168.183.101:4444

192.168.183.101:5555

#### 5）编辑 start.sh 脚本

nacos/bin 下

```shell
[root@localhost sbin]# vim startup.sh 
```

![image-20210303153457199](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303153457.png)

![image-20210303153528649](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303153528.png)

修改这三处，为了让启动命令能传入端口号，接受不同的启动端

##### 执行方式

```shell
[root@localhost bin]# ./startup.sh -h 3333
[root@localhost bin]# ./startup.sh -h 4444
[root@localhost bin]# ./startup.sh -h 5555
```

#### 6）Nginx 配置，由它作为负载均衡器

修改 nginx 配置文件 /usr/local/nginx/conf

编辑 nginx.conf

![image-20210303154303466](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303154303.png)

修改这三处配置

```shell
upstream cluster{
    server 192.168.183.101:3333;
    server 192.168.183.101:4444;
    server 192.168.183.101:5555;
}
```

```shell
listen       1111;
server_name  localhost;

#charset koi8-r;

#access_log  logs/host.access.log  main;

location / {
    #root   html;
    #index  index.html index.htm;
	proxy_pass http://cluster;
}

```

##### 启动 nginx

```shell
[root@localhost sbin]# ./nginx -c /usr/local/nginx/conf/nginx.conf
```

#### 7）测试

##### 访问 

http://192.168.183.101:1111/nacos

![image-20210303154726768](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303154726.png)

##### 将微服务启动注册到集群

###### yml

![image-20210303154950124](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303154950.png)

###### 查看注册中心

![image-20210303155024995](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303155025.png)

### 4、总结

![image-20210303155250812](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210303155250.png)