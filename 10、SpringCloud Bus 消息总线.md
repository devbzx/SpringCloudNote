* [SpringCloud Bus 消息总线](#springcloud-bus-%E6%B6%88%E6%81%AF%E6%80%BB%E7%BA%BF)
  * [一、概述](#%E4%B8%80%E6%A6%82%E8%BF%B0)
    * [1、是什么](#1%E6%98%AF%E4%BB%80%E4%B9%88)
    * [2、能干嘛](#2%E8%83%BD%E5%B9%B2%E5%98%9B)
    * [3、为何被称为总线](#3%E4%B8%BA%E4%BD%95%E8%A2%AB%E7%A7%B0%E4%B8%BA%E6%80%BB%E7%BA%BF)
      * [1）什么是总线](#1%E4%BB%80%E4%B9%88%E6%98%AF%E6%80%BB%E7%BA%BF)
      * [2）基本原理](#2%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86)
  * [二、RabbitMQ 环境配置](#%E4%BA%8Crabbitmq-%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE)
    * [1、拉取镜像](#1%E6%8B%89%E5%8F%96%E9%95%9C%E5%83%8F)
    * [2、端口映射，启动](#2%E7%AB%AF%E5%8F%A3%E6%98%A0%E5%B0%84%E5%90%AF%E5%8A%A8)
    * [3、访问15672端口进入管理页面](#3%E8%AE%BF%E9%97%AE15672%E7%AB%AF%E5%8F%A3%E8%BF%9B%E5%85%A5%E7%AE%A1%E7%90%86%E9%A1%B5%E9%9D%A2)
  * [三、SpringCloud Bus 动态刷新全局广播](#%E4%B8%89springcloud-bus-%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0%E5%85%A8%E5%B1%80%E5%B9%BF%E6%92%AD)
    * [1、前置条件](#1%E5%89%8D%E7%BD%AE%E6%9D%A1%E4%BB%B6)
    * [2、新建 3366](#2%E6%96%B0%E5%BB%BA-3366)
      * [1）pom 文件](#1pom-%E6%96%87%E4%BB%B6)
      * [2）yml 文件](#2yml-%E6%96%87%E4%BB%B6)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）控制类](#4%E6%8E%A7%E5%88%B6%E7%B1%BB)
    * [3、设计思想](#3%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3)
      * [1）利用消息总线触发一个客户端 /bus/refresh，而刷新所有客户端的配置](#1%E5%88%A9%E7%94%A8%E6%B6%88%E6%81%AF%E6%80%BB%E7%BA%BF%E8%A7%A6%E5%8F%91%E4%B8%80%E4%B8%AA%E5%AE%A2%E6%88%B7%E7%AB%AF-busrefresh%E8%80%8C%E5%88%B7%E6%96%B0%E6%89%80%E6%9C%89%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%9A%84%E9%85%8D%E7%BD%AE)
      * [2）利用消息总线触发一个服务端 ConfigServer 的 /bus/refresh 端点，而刷新所有客户端（更加推荐）](#2%E5%88%A9%E7%94%A8%E6%B6%88%E6%81%AF%E6%80%BB%E7%BA%BF%E8%A7%A6%E5%8F%91%E4%B8%80%E4%B8%AA%E6%9C%8D%E5%8A%A1%E7%AB%AF-configserver-%E7%9A%84-busrefresh-%E7%AB%AF%E7%82%B9%E8%80%8C%E5%88%B7%E6%96%B0%E6%89%80%E6%9C%89%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%9B%B4%E5%8A%A0%E6%8E%A8%E8%8D%90)
      * [3）优劣](#3%E4%BC%98%E5%8A%A3)
    * [4、配置 3344](#4%E9%85%8D%E7%BD%AE-3344)
      * [1）pom\.xml](#1pomxml)
      * [2）yml](#2yml)
    * [5、测试](#5%E6%B5%8B%E8%AF%95)
      * [1）修改 git 上的文件](#1%E4%BF%AE%E6%94%B9-git-%E4%B8%8A%E7%9A%84%E6%96%87%E4%BB%B6)
      * [2）发送 post 请求](#2%E5%8F%91%E9%80%81-post-%E8%AF%B7%E6%B1%82)
      * [3）一次发送，处处生效](#3%E4%B8%80%E6%AC%A1%E5%8F%91%E9%80%81%E5%A4%84%E5%A4%84%E7%94%9F%E6%95%88)
  * [四、SpringCloud Bus 动态刷新定点通知](#%E5%9B%9Bspringcloud-bus-%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0%E5%AE%9A%E7%82%B9%E9%80%9A%E7%9F%A5)
    * [1、大纲](#1%E5%A4%A7%E7%BA%B2)
    * [2、案例](#2%E6%A1%88%E4%BE%8B)
    * [3、总结](#3%E6%80%BB%E7%BB%93)

# SpringCloud Bus 消息总线

## 一、概述

### 1、是什么

![下载](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223102713.png)

Bus 支持两种消息代理：RabbitMQ 和 Kafka

### 2、能干嘛

![下载 (1)](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223102854.png)

### 3、为何被称为总线

#### 1）什么是总线

在微服务架构的系统中，通常会使用轻量级的消息代理来构建一个共用的消息主题，并让系统中所有微服务实例都连接上来。

由于该主题中产生的消息会被所有实例监听和消费，所以称它为消息总线。

在总线上的各个实例，都可以方便地广播一些需要让其他连接在该主题上的实例都知道的消息。

#### 2）基本原理

ConfigClient 实例都会监听 MQ 中同一个 topic（默认是 SpringCloudBus）。

当一个服务刷新数据的时候，它会把这个信息放入到 Topic 中，这样其他监听同一 Topic 的服务就能得到通知，然后去更新自身的配置。

## 二、RabbitMQ 环境配置

### 1、拉取镜像

```bash
docker pull rabbitmq:3.7.26-management
```

### 2、端口映射，启动

```shell
docker run -d -p 5672:5672 -p 15672:15672 --name myrabbitmq f6ea23d4f7e4 
```

### 3、访问15672端口进入管理页面

http://192.168.183.101:15672

账号：guest

密码：guest

![image-20210223104551877](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223104551.png)

## 三、SpringCloud Bus 动态刷新全局广播

### 1、前置条件

必须具备 RabbitMQ 环境

### 2、新建 3366

复刻 3355，两个东西一致

#### 1）pom 文件

```xml
<dependencies>
    <!--        添加消息总线支持-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-bus-amqp</artifactId>
    </dependency>
    <!--eureka server-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>

    <!--添加config-client依赖-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>

    <!--web/actuator这两个一般一起使用，写在一起-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>
            org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <!--一般通用配置-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
```

#### 2）yml 文件

```yml
server:
  port: 3366

spring:
  application:
    name: config-client
  cloud:
#    config 客户端配置
    config:
      label: main   # 分支名称
      name: config    # 配置文件名称
      profile: dev    # 读取后缀名称
      uri: http://config-3344.com:3344 # 配置中心服务器地址
#      discovery:  # 对应eureka中的配置中心，默认不写是找config-server
#        service-id: cloud-config-center
#        enabled: true # 开启读取配置中心的配置，默认是false

  # rabbitmq 的相关配置
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
#服务注册到 eureka
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
#    instance:
#      #instance-id: springcloud-config-client01
#      prefer-ip-address: true

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class ConfigClientMain3355 {
    public static void main(String[] args){
        SpringApplication.run(ConfigClientMain3355.class,args);
    }
}
```

#### 4）控制类

```java
@RestController
@RefreshScope
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }

}
```

### 3、设计思想

#### 1）利用消息总线触发一个客户端 /bus/refresh，而刷新所有客户端的配置

![image-20210223140612393](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223140612.png)

#### 2）利用消息总线触发一个服务端 ConfigServer 的 /bus/refresh 端点，而刷新所有客户端（更加推荐）

![image-20210223141020660](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223141020.png)

#### 3）优劣

> 图二更加合适，图一不合适原因如下

- 打破了微服务的职责单一性，因为微服务本身是业务模块，它本不应该承担配置刷新职责

- 破坏了微服务各节点的对等性
- 有一定局限性。例如，微服务在迁移时，它的网络地址常常会发生变化，如果想要做到自动刷新，那就会增加更多的修改。

### 4、配置 3344

#### 1）pom.xml

```xml
<dependencies>
    <!--        添加消息总线支持-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-bus-amqp</artifactId>
    </dependency>
    <!--eureka server-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>

    <!--添加config-server依赖-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>

    <!--web/actuator这两个一般一起使用，写在一起-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>
            org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <!--一般通用配置-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
```

#### 2）yml

```yml
server:
  port: 3344

spring:
  application:
    name: cloud-config-center # 注册进eureka服务器的微服务名
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/li_hao_qi/springcloud-config.git
          # 搜索目录
          search-paths:
            - springcloud-config
      # 读取分支
      label: main
  # rabbitmq 的相关配置
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
# 服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
#  instance:
#    instance-id: cloud-config-server
#    prefer-ip-address: true

# rabbitmq 相关配置，暴露 bus 刷新配置的端点
management:
  endpoints:  # 暴露 bus 刷新配置的端点
    web:
      exposure:
        include: 'bus-refresh'
```

### 5、测试

#### 1）修改 git 上的文件

![image-20210223141925423](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223141925.png)

#### 2）发送 post 请求

curl -X POST "http://localhost:3344/actuator/bus-refresh"

我们可以看到 rabbitmq 的界面发生了变化

![image-20210223142851365](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223142851.png)

#### 3）一次发送，处处生效

可以用 3355、3366 访问，信息全部改变

http://config-3344.com:3344/main/config-dev.yml

![image-20210223142717181](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223142717.png)

http://localhost:3355/configInfo

![image-20210223142524420](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223142524.png)

http://localhost:3366/configInfo

![image-20210223142545314](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223142545.png)

## 四、SpringCloud Bus 动态刷新定点通知

### 1、大纲

> 通知某一个实例生效而不是全部

公式：http://localhost:配置中心端口号/actuator/bus-refresh/{destination}

/bus/refresh 请求不再发送到具体的服务实例上，而是发给 config server 并通过 destination 参数类指定需要更新配置的服务或实例。

### 2、案例

发送 curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"

我们发现 3355 随着 3344 变了

![image-20210223152228525](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223152228.png)

![image-20210223152239216](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223152239.png)

3366 未改变

![image-20210223152304549](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223152304.png)

### 3、总结

![下载 (1)](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223152416.png)