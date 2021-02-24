* [SpringCloud Stream 消息驱动](#springcloud-stream-%E6%B6%88%E6%81%AF%E9%A9%B1%E5%8A%A8)
  * [一、消息驱动概述](#%E4%B8%80%E6%B6%88%E6%81%AF%E9%A9%B1%E5%8A%A8%E6%A6%82%E8%BF%B0)
    * [1、是什么](#1%E6%98%AF%E4%BB%80%E4%B9%88)
    * [2、设计思想](#2%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3)
      * [1）传统消息中间件的流程](#1%E4%BC%A0%E7%BB%9F%E6%B6%88%E6%81%AF%E4%B8%AD%E9%97%B4%E4%BB%B6%E7%9A%84%E6%B5%81%E7%A8%8B)
      * [2）为什么使用 SpringCloud Stream](#2%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8-springcloud-stream)
      * [3）Stream 是怎么统一底层差异的](#3stream-%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%9F%E4%B8%80%E5%BA%95%E5%B1%82%E5%B7%AE%E5%BC%82%E7%9A%84)
      * [4）官方架构图](#4%E5%AE%98%E6%96%B9%E6%9E%B6%E6%9E%84%E5%9B%BE)
      * [5）Stream 中的消息通信方式遵循了发布\-订阅模式](#5stream-%E4%B8%AD%E7%9A%84%E6%B6%88%E6%81%AF%E9%80%9A%E4%BF%A1%E6%96%B9%E5%BC%8F%E9%81%B5%E5%BE%AA%E4%BA%86%E5%8F%91%E5%B8%83-%E8%AE%A2%E9%98%85%E6%A8%A1%E5%BC%8F)
    * [3、Spring Cloud Stream 标准流程套路](#3spring-cloud-stream-%E6%A0%87%E5%87%86%E6%B5%81%E7%A8%8B%E5%A5%97%E8%B7%AF)
    * [4、编码 API 常用注解](#4%E7%BC%96%E7%A0%81-api-%E5%B8%B8%E7%94%A8%E6%B3%A8%E8%A7%A3)
  * [二、案例说明](#%E4%BA%8C%E6%A1%88%E4%BE%8B%E8%AF%B4%E6%98%8E)
    * [1、Rabbit MQ 环境](#1rabbit-mq-%E7%8E%AF%E5%A2%83)
    * [2、三个子模块](#2%E4%B8%89%E4%B8%AA%E5%AD%90%E6%A8%A1%E5%9D%97)
  * [三、消息驱动之生产者](#%E4%B8%89%E6%B6%88%E6%81%AF%E9%A9%B1%E5%8A%A8%E4%B9%8B%E7%94%9F%E4%BA%A7%E8%80%85)
    * [1、新建 cloud\-stream\-rabbitmq\-provider8801](#1%E6%96%B0%E5%BB%BA-cloud-stream-rabbitmq-provider8801)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）业务类](#4%E4%B8%9A%E5%8A%A1%E7%B1%BB)
        * [1、发送消息接口](#1%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF%E6%8E%A5%E5%8F%A3)
        * [2、发送消息接口实现类](#2%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E7%B1%BB)
        * [3、Controller](#3controller)
    * [2、测试](#2%E6%B5%8B%E8%AF%95)
      * [1）启动注册中心 7001](#1%E5%90%AF%E5%8A%A8%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83-7001)
      * [2）启动 rabbitmq](#2%E5%90%AF%E5%8A%A8-rabbitmq)
      * [3）启动 8801](#3%E5%90%AF%E5%8A%A8-8801)
      * [4）访问](#4%E8%AE%BF%E9%97%AE)
      * [5）结果](#5%E7%BB%93%E6%9E%9C)
        * [1、控制台](#1%E6%8E%A7%E5%88%B6%E5%8F%B0)
        * [2、rabbitmq](#2rabbitmq)
  * [四、消息驱动之消费者](#%E5%9B%9B%E6%B6%88%E6%81%AF%E9%A9%B1%E5%8A%A8%E4%B9%8B%E6%B6%88%E8%B4%B9%E8%80%85)
    * [1、新建 cloud\-stream\-rabbitmq\-consumer8802](#1%E6%96%B0%E5%BB%BA-cloud-stream-rabbitmq-consumer8802)
      * [1）pom\.xml](#1pomxml-1)
      * [2）application\.yml](#2applicationyml-1)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
      * [4）监听器](#4%E7%9B%91%E5%90%AC%E5%99%A8)
    * [2、测试](#2%E6%B5%8B%E8%AF%95-1)
      * [1）启动注册中心 7001](#1%E5%90%AF%E5%8A%A8%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83-7001-1)
      * [2）启动 rabbitmq](#2%E5%90%AF%E5%8A%A8-rabbitmq-1)
      * [3）启动 8801、8802](#3%E5%90%AF%E5%8A%A8-88018802)
      * [4）访问](#4%E8%AE%BF%E9%97%AE-1)
      * [5）结果](#5%E7%BB%93%E6%9E%9C-1)
        * [1、8801 控制台](#18801-%E6%8E%A7%E5%88%B6%E5%8F%B0)
        * [2、8802 控制台](#28802-%E6%8E%A7%E5%88%B6%E5%8F%B0)
        * [3、rabbitmq](#3rabbitmq)
  * [五、分组消费与持久化](#%E4%BA%94%E5%88%86%E7%BB%84%E6%B6%88%E8%B4%B9%E4%B8%8E%E6%8C%81%E4%B9%85%E5%8C%96)
    * [1、重复消费问题](#1%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9%E9%97%AE%E9%A2%98)
      * [1）新建 8803，依照 8802](#1%E6%96%B0%E5%BB%BA-8803%E4%BE%9D%E7%85%A7-8802)
      * [2）启动](#2%E5%90%AF%E5%8A%A8)
      * [3）运行后两个问题](#3%E8%BF%90%E8%A1%8C%E5%90%8E%E4%B8%A4%E4%B8%AA%E9%97%AE%E9%A2%98)
        * [重复消费](#%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9)
        * [解决方法（重要）](#%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E9%87%8D%E8%A6%81)
        * [生产实际案例](#%E7%94%9F%E4%BA%A7%E5%AE%9E%E9%99%85%E6%A1%88%E4%BE%8B)
    * [2、分组](#2%E5%88%86%E7%BB%84)
      * [1）8802、8803 改 yml](#188028803-%E6%94%B9-yml)
      * [2）仍然重复消费](#2%E4%BB%8D%E7%84%B6%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9)
      * [3）将两个消费者改为同一组](#3%E5%B0%86%E4%B8%A4%E4%B8%AA%E6%B6%88%E8%B4%B9%E8%80%85%E6%94%B9%E4%B8%BA%E5%90%8C%E4%B8%80%E7%BB%84)
    * [3、持久化](#3%E6%8C%81%E4%B9%85%E5%8C%96)
      * [1）停止 8802/8803 并去掉了 8802 的分组](#1%E5%81%9C%E6%AD%A2-88028803-%E5%B9%B6%E5%8E%BB%E6%8E%89%E4%BA%86-8802-%E7%9A%84%E5%88%86%E7%BB%84)
      * [2）8801 发送 4 条信息到 rabbitmq](#28801-%E5%8F%91%E9%80%81-4-%E6%9D%A1%E4%BF%A1%E6%81%AF%E5%88%B0-rabbitmq)
      * [3）启动 8802，未分组，后台没有打出来消息](#3%E5%90%AF%E5%8A%A8-8802%E6%9C%AA%E5%88%86%E7%BB%84%E5%90%8E%E5%8F%B0%E6%B2%A1%E6%9C%89%E6%89%93%E5%87%BA%E6%9D%A5%E6%B6%88%E6%81%AF)
      * [4）启动 8803，有分组，后台打出来了 rabbitmq 的消息](#4%E5%90%AF%E5%8A%A8-8803%E6%9C%89%E5%88%86%E7%BB%84%E5%90%8E%E5%8F%B0%E6%89%93%E5%87%BA%E6%9D%A5%E4%BA%86-rabbitmq-%E7%9A%84%E6%B6%88%E6%81%AF)

# SpringCloud Stream 消息驱动

## 一、消息驱动概述

### 1、是什么

> 屏蔽底层消息中间件的差异，降低切换成本，统一消息的编程模型

官方定义 Spring Cloud Stream 是一个构建消息驱动微服务的框架。

应用程序通过 inputs 或者 outputs 来与 Spring Cloud Stream 中的 binder 对象交互。

通过我们配置来 binding（绑定），而 Spring Cloud Stream 的 binder 对象负责与消息中间件交互。

所以，我们只需要搞清楚如何与 Spring Cloud Stream 交互就可以方便使用消息驱动的方式。

通过使用 Spring Integration 来连接消息代理中间件以实现消息时间驱动。

Spring Cloud Stream 为一些供应商的消息中间件提供了个性化的自动化配置实现，引用了发布-订阅、消费组、分区的三个核心概念。

目前仅支持 RabbitMQ、Kafka。

### 2、设计思想

#### 1）传统消息中间件的流程

生产者/消费者之间靠消息媒介传递信息内容（Message）

消息必须走特定的通道（消息通道MessageChannel）

消息通道里的消息如何被消费，谁负责收发处理（消息通道 MessageChannel 的子接口 SubscribableChannel，由 MessageHandler 消息处理器所订阅）

#### 2）为什么使用 SpringCloud Stream

当我们同时用到了 RabbitMQ 和 Kafka，由于这两个消息中间件的架构上的不同，整合和切换就会有很大的成本。

比如 RabbitMQ 有 exchange，Kafka 有 Topic 和 Partion 分区。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224101556.png)

这些中间件的差异性在我们实际项目开发中造成了一定的困扰，我们如果用了两个消息队列的其中一种，后面也无需要，我们想往另外一种消息队列进行迁移，就会有一大堆东西都要推倒重新做，这时候无疑是灾难性的。

因为消息中间件已经和我们的系统耦合了，这时候 SpringCloudStream 给我们提供了一种解耦合的方式。

#### 3）Stream 是怎么统一底层差异的

在没有绑定器这个概念的情况下，我们的 SpringBoot 应用要直接与消息中间件进行信息交互的时候，由于各种消息中间件构建的初衷不同，它们的实现细节上会有较大的差异性。

通过定义绑定器作为中间层，完美地实现了应用程序与消息中间件细节之间的隔离。

通过向应用程序暴露统一的 Channel 通道，使得应用程序不需要再考虑各种不同的消息中间件实现。

通过定义绑定器 Binder 作为中间层，实现了应用程序与消息中间件细节之间的隔离。

Binder：1、input 对应于消费者。2、output 对应于生产者。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224102412.png)

#### 4）官方架构图

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224102504.png)

#### 5）Stream 中的消息通信方式遵循了发布-订阅模式

Topic 主题进行广播：

在 RabbitMQ 中就是 Exchange

在 Kafka 中就是 Topic

### 3、Spring Cloud Stream 标准流程套路

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224102625.png)

Binder：屏蔽消息中间件的连接中间件（连接中间件和生产者/消费者的）

Channel：通道，是队列 Queue 的一种抽象，在消息通讯中就是实现存储和转发的媒介，通过 Channel 对队列进行配置。

Source 和 Sink：简单的可理解为参照对象是 SpringCloudStream 自身，从 Stream 发布消息就是输出，接收消息就是输入。

### 4、编码 API 常用注解

| 组成            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| Middleware      | 中间件，目前只支持 RabbitMQ 和 Kafka                         |
| Binder          | Binder 是应用与消息中间件之间的封装，目前实行了 Kafka 和 RabbitMQ 的 Binder，通过 Binder 可以很方便的连接中间件，可以动态的改变消息类型（对应于 Kafka 的 topic，RabbitMQ 的 exchange），这些都可以通过配置文件来实现 |
| @Input          | 注解标识输入通道，通过该输入通道接收到的消息进入应用程序     |
| @Output         | 注解标识输出通道，发布的消息将通过该通道离开应用程序         |
| @StreamListener | 监听队列，用于消费者的队列的消息接收                         |
| @EnableBinding  | 指信道 channel 和 exchange 绑定在一起                        |

## 二、案例说明

### 1、Rabbit MQ 环境

### 2、三个子模块

cloud-stream-rabbitmq-provider8801，作为生产者进行发消息模块

cloud-stream-rabbitmq-consumer8802，作为消息接收模块

cloud-stream-rabbitmq-consumer8803，作为消息接收模块

## 三、消息驱动之生产者

### 1、新建 cloud-stream-rabbitmq-provider8801

#### 1）pom.xml

```xml
<dependencies>
    <!--rabbitMQ-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
    </dependency>

    <!--eureka client-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

    <!--热部署-->
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

#### 2）application.yml

```yml
server:
  port: 8801

spring:
  application:
    name: cloud-stream-provider
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
      bindings: # 服务的整合处理
        output: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: send-8801.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

![image-20210224111355679](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224111355.png)

这里爆红不必惊慌

#### 3）主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 09:39
 */
@SpringBootApplication
public class StreamMQMain {

    public static void main(String[] args){
        SpringApplication.run(StreamMQMain.class,args);
    }

}

```

#### 4）业务类

##### 1、发送消息接口

```java
package com.lhq.cloud2020.service;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 09:40
 */
public interface IMessageProvider {

    String send();

}
```

##### 2、发送消息接口实现类

```java
package com.lhq.cloud2020.service.impl;

import com.lhq.cloud2020.service.IMessageProvider;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.messaging.MessageChannel;
import org.springframework.messaging.support.MessageBuilder;

import javax.annotation.Resource;
import java.util.UUID;


/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 09:41
 */
@EnableBinding(Source.class)  //定义消息的推送管道
public class IMessageProviderImpl implements IMessageProvider {

    @Resource
    private MessageChannel output;  //消息发送管道

    @Override
    public String send() {
        String serial = UUID.randomUUID().toString();
        output.send(MessageBuilder.withPayload(serial).build());
        System.out.println("****serial:" + serial);
        return null;
    }

}
```

##### 3、Controller

```java
package com.lhq.cloud2020.controller;

import com.lhq.cloud2020.service.IMessageProvider;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 09:48
 */
@RestController
public class SendMessageController {

    @Resource
    private IMessageProvider messageProvider;

    @GetMapping(value = "sendMessage")
    public String sendMessage(){
        return messageProvider.send();
    }

}
```

### 2、测试

#### 1）启动注册中心 7001

#### 2）启动 rabbitmq

http://192.168.183.101:15672/

#### 3）启动 8801

#### 4）访问

多次访问

http://localhost:8801/sendMessage

#### 5）结果

##### 1、控制台

![image-20210224111117926](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224111117.png)

##### 2、rabbitmq

http://192.168.183.101:15672/#/

![image-20210224111214757](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224111214.png)

## 四、消息驱动之消费者

### 1、新建 cloud-stream-rabbitmq-consumer8802

#### 1）pom.xml

```xml
<dependencies>
    <!--rabbitMQ-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
    </dependency>

    <!--eureka client-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

    <!--热部署-->
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

#### 2）application.yml

```yml
server:
  port: 8802

spring:
  application:
    name: cloud-stream-provider
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8802.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

#### 3）主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 14:47
 */
@SpringBootApplication
public class StreamMQMain8802 {

    public static void main(String[] args){
        SpringApplication.run(StreamMQMain8802.class,args);
    }

}
```

#### 4）监听器

```java
package com.lhq.cloud2020.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2021-02-24 14:49
 */
@Component
@EnableBinding(Sink.class)
public class ReceiveMessageListenerController {

    @Value("${server.port}")
    private String serverPort;

    @StreamListener(Sink.INPUT)
    public void input(Message<String> message){
        System.out.println("消费者 1 号，---->接收到的消息：" + message.getPayload() + "\t port：" + serverPort);
    }

}
```

### 2、测试

#### 1）启动注册中心 7001

#### 2）启动 rabbitmq

http://192.168.183.101:15672/

#### 3）启动 8801、8802

#### 4）访问

http://localhost:8801/sendMessage

#### 5）结果

##### 1、8801 控制台

![image-20210224150445763](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224150445.png)

##### 2、8802 控制台

![image-20210224150514628](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224150514.png)

##### 3、rabbitmq

![image-20210224150656619](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224150656.png)

## 五、分组消费与持久化

### 1、重复消费问题

#### 1）新建 8803，依照 8802

#### 2）启动

RabbitMQ、7001、8801、8802、8803

#### 3）运行后两个问题

##### 重复消费

我们发送两次 http://localhost:8801/sendMessage 请求

![屏幕截图 2021-02-24 154158](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224154930.png)

两个消费者的控制台

![屏幕截图 2021-02-24 153934](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224154959.png)![屏幕截图 2021-02-24 153947](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224154954.png)

8802、8803 同时都收到了，存在重复消费问题

##### 解决方法（重要）

分组和持久化属性 group

##### 生产实际案例

比如在如下场景中，订单系统我们做集群部署，都会从 Rabbit MQ 中获取订单信息，那如果同一个订单同时被两个服务获取到，那么就会造成数据错误，我们得避免这种情况。

这时我们就可以使用 stream 中的消息分组来解决。

![image-20210224155519796](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224155519.png)

注意在 stream 中处于同一个 group 中的多个消费者是竞争关系，就能够保证消息只会被一个应用消费一次。

不同组是可以全面消费的（重复消费）

同一组内会发生竞争关系，只有其中一个可以消费。

### 2、分组

> 微服务应用放置于同一个 group 中，就能保证消息只会被其中一个应用消费一次。不同的组是可以消费的，同一个组内会发生竞争关系，只有其中一个可以消费。

group：lhqA、lhqB

#### 1）8802、8803 改 yml

增加 

```yml
group: lhqA
```

```yml
group: lhqB
```



```yml
server:
  port: 8802

spring:
  application:
    name: cloud-stream-consumer
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置
          group: lhqA

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8802.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

```yml
server:
  port: 8803

spring:
  application:
    name: cloud-stream-consumer
  rabbitmq:
    host: 192.168.183.101
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置
          group: lhqB

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8803.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

可以看到 Rabbit MQ 上的 Binding

![屏幕截图 2021-02-24 153916](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224162925.png)

分布式微服务应用为了实现高可用和负载均衡，实际上都会部署多个实例，本例启动了两个消费微服务（8802/8803）

多数情况，生产者发送消息给某个具体微服务时只希望被消费一次，按照上面我们启动两个应用的例子，虽然它们同属于一个应用，但是这个消息出现了被重复消费两次的情况。

为了解决这个问题，在 Spring Cloud Stream 中提供了消费组的概念。

#### 2）仍然重复消费

因为两个消费者属于不同组，所以出现重复消费

#### 3）将两个消费者改为同一组

就避免了重复消费

8802/8803 实现了轮询分组，每次只有一个消费者

8801 模块的发的消息只能被 8802 或 8803 其中一个接收到，这样避免了重复消费

http://localhost:8801/sendMessage

![屏幕截图 2021-02-24 154318](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224163640.png)

![屏幕截图 2021-02-24 154337](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224163646.png)

![屏幕截图 2021-02-24 154353](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224163653.png)

同一个组的多个微服务实例，每次只会有一个拿到

### 3、持久化

#### 1）停止 8802/8803 并去掉了 8802 的分组

#### 2）8801 发送 4 条信息到 rabbitmq

![image-20210224164424471](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224164424.png)

#### 3）启动 8802，未分组，后台没有打出来消息

#### 4）启动 8803，有分组，后台打出来了 rabbitmq 的消息

![image-20210224164438321](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210224164438.png)