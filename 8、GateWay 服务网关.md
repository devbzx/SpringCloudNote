* [Gateway 服务网关](#gateway-%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3)
  * [一、概述简介](#%E4%B8%80%E6%A6%82%E8%BF%B0%E7%AE%80%E4%BB%8B)
    * [1、背景](#1%E8%83%8C%E6%99%AF)
    * [2、概述](#2%E6%A6%82%E8%BF%B0)
    * [3、能干嘛](#3%E8%83%BD%E5%B9%B2%E5%98%9B)
    * [4、微服务架构中网关在哪里](#4%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E4%B8%AD%E7%BD%91%E5%85%B3%E5%9C%A8%E5%93%AA%E9%87%8C)
    * [5、为什么选择 GateWay](#5%E4%B8%BA%E4%BB%80%E4%B9%88%E9%80%89%E6%8B%A9-gateway)
      * [1）SpringCloud Gateway具有如下特性](#1springcloud-gateway%E5%85%B7%E6%9C%89%E5%A6%82%E4%B8%8B%E7%89%B9%E6%80%A7)
      * [2）SpringCloud和Zuul的区别](#2springcloud%E5%92%8Czuul%E7%9A%84%E5%8C%BA%E5%88%AB)
      * [3）Zuul 1\.x 的模型](#3zuul-1x-%E7%9A%84%E6%A8%A1%E5%9E%8B)
      * [4）GateWay 模型](#4gateway-%E6%A8%A1%E5%9E%8B)
  * [二、三大核心概念](#%E4%BA%8C%E4%B8%89%E5%A4%A7%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5)
    * [1、Route（路由）](#1route%E8%B7%AF%E7%94%B1)
    * [2、Predicate（断言）](#2predicate%E6%96%AD%E8%A8%80)
    * [3、Filter（过滤）](#3filter%E8%BF%87%E6%BB%A4)
  * [三、Gateway 工作流程](#%E4%B8%89gateway-%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
  * [四、入门配置](#%E5%9B%9B%E5%85%A5%E9%97%A8%E9%85%8D%E7%BD%AE)
    * [1、新建 cloud\-gateway\-gateway9527](#1%E6%96%B0%E5%BB%BA-cloud-gateway-gateway9527)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）如何做路由映射呢](#4%E5%A6%82%E4%BD%95%E5%81%9A%E8%B7%AF%E7%94%B1%E6%98%A0%E5%B0%84%E5%91%A2)
        * [1、我们以前访问 cloud\-provider\-payment8001 中的 controller 方法是通过](#1%E6%88%91%E4%BB%AC%E4%BB%A5%E5%89%8D%E8%AE%BF%E9%97%AE-cloud-provider-payment8001-%E4%B8%AD%E7%9A%84-controller-%E6%96%B9%E6%B3%95%E6%98%AF%E9%80%9A%E8%BF%87)
        * [2、我们不想暴漏 8001 端口，希望通过 9527 进行访问](#2%E6%88%91%E4%BB%AC%E4%B8%8D%E6%83%B3%E6%9A%B4%E6%BC%8F-8001-%E7%AB%AF%E5%8F%A3%E5%B8%8C%E6%9C%9B%E9%80%9A%E8%BF%87-9527-%E8%BF%9B%E8%A1%8C%E8%AE%BF%E9%97%AE)
        * [3、将 application\.yml 修改为](#3%E5%B0%86-applicationyml-%E4%BF%AE%E6%94%B9%E4%B8%BA)
        * [4、看图](#4%E7%9C%8B%E5%9B%BE)
        * [5、测试](#5%E6%B5%8B%E8%AF%95)
      * [5）另一种方式配置 Gateway 网关路由](#5%E5%8F%A6%E4%B8%80%E7%A7%8D%E6%96%B9%E5%BC%8F%E9%85%8D%E7%BD%AE-gateway-%E7%BD%91%E5%85%B3%E8%B7%AF%E7%94%B1)
        * [1、代码配置](#1%E4%BB%A3%E7%A0%81%E9%85%8D%E7%BD%AE)
  * [五、通过微服务名实现动态路由](#%E4%BA%94%E9%80%9A%E8%BF%87%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%90%8D%E5%AE%9E%E7%8E%B0%E5%8A%A8%E6%80%81%E8%B7%AF%E7%94%B1)
    * [1、原来存在的问题](#1%E5%8E%9F%E6%9D%A5%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98)
    * [2、修改代码](#2%E4%BF%AE%E6%94%B9%E4%BB%A3%E7%A0%81)
    * [3、测试](#3%E6%B5%8B%E8%AF%95)
  * [六、Predicate 的使用](#%E5%85%ADpredicate-%E7%9A%84%E4%BD%BF%E7%94%A8)
  * [七、Filter 的使用](#%E4%B8%83filter-%E7%9A%84%E4%BD%BF%E7%94%A8)

# Gateway 服务网关

## 一、概述简介

### 1、背景

cloud全家桶中有个很重要的组件就是网关，在1.x版本中都是采用的zuul网关 

但在2.x版本中，zuul的升级一直跳票，SpringCloud最后自己研发了一个网关替代zuul： 

即springcloud gateway一句话：gateway是原zuul1.x版的替代。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220141609.png)

### 2、概述

Gateway是在Spring生态系统上构建的API网关服务，基于Spring5，SpringBoot2和Project Reactor等技术。 

Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的过滤功能，例如：熔断，限流，重试。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220142015.png)

底层替代了zuul，基于WebFlux框架，WebFlux框架使用了Reactor模式通信框架Netty 

一句话：SpringCoud Gateway使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架。

### 3、能干嘛

反向代理、鉴权、流量控制、熔断、日志监控......

### 4、微服务架构中网关在哪里

![image-20210220142324900](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220142324.png)

### 5、为什么选择 GateWay

#### 1）SpringCloud Gateway具有如下特性

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220142611.png)

#### 2）SpringCloud和Zuul的区别

![img](https://gitee.com/xue--dong/blog_images/raw/master/images/20210129213000.png)

#### 3）Zuul 1.x 的模型

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220142751.png)

#### 4）GateWay 模型

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220142824.png)

## 二、三大核心概念

### 1、Route（路由）

> 构建网关的基本模块，由 ID，目标 URI，一系列的断言和过滤器组成，如果断言为 true 则匹配该路由。

### 2、Predicate（断言）

> 参考的是 Java8 的 java.util.function.Predicate 开发人员可以匹配HTTP请求中的所有内容（例如请求头或请求参数），**如果请求与断言相匹配则进行路由**。

### 3、Filter（过滤）

> 指的是 Spring 框架中 GatewayFilter 的实例，使用过滤器，可以在请求被路由前或者之后对请求进行修改。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220143534.png)

## 三、Gateway 工作流程

> 核心逻辑：路由转发+执行过滤器链

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220143701.png)

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220143714.png)

## 四、入门配置

### 1、新建 cloud-gateway-gateway9527

#### 1）pom.xml

pom 里不能有 spring-boot-starter-web

```xml
<dependencies>
    <!--引入网关gateway-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>

    <!--引入我们自定义的公共api jar包-->
    <dependency>
        <groupId>com.lhq.cloud2020</groupId>
        <artifactId>cloud-api-commons</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <!--eureka client-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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
  port: 9527

spring:
  application:
    name: cloud-gateway
    
eureka:
  instance:
    hostname: cloud-gateway-service
    prefer-ip-address: true
    instance-id: cloud-gateway9527
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      #      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      defaultZone: http://localhost:7001/eureka  #单机版
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
public class GateWayMain9527 {
    public static void main(String[] args){
        SpringApplication.run(GateWayMain9527.class,args);
    }
}
```

#### 4）如何做路由映射呢

##### 1、我们以前访问 cloud-provider-payment8001 中的 controller 方法是通过

http://localhost:8001/payment/get/1

http://localhost:8001/payment/lb

就能访问到响应的方法

##### 2、我们不想暴漏 8001 端口，希望通过 9527 进行访问

##### 3、将 application.yml 修改为

```yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:   # 可以为controller中的所有rest接口做路由
        - id: payment_routh           # 路由id：payment_route，没有固定规则，建议配合服务名
          uri: http://localhost:8001  # 匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**    # 断言：路径相匹配的进行路由

        - id: payment_routh2
          uri: http://localhost:8001
          predicates:
            - Path=/payment/lb/**

eureka:
  instance:
    hostname: cloud-gateway-service
    prefer-ip-address: true
    instance-id: cloud-gateway9527
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      #      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      defaultZone: http://localhost:7001/eureka  #单机版
```

```yml
routes:    
        
        #可以指定多个路由，为controller中的所有rest接口做路由
    
    routes:  
        - id: payment_routh           
          uri: http://localhost:8001  # 匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**    # 断言：路径相匹配的进行路由
            
        #uri和predicates拼接：//localhost:8001/payment/get/**就是具体的接口请求路径。
        
        #predicates：我断言http://localhost:8001下面有一个/payment/get/**这样的地址。
            #找到了这个地址就返回true，可以用9527端口访问，进行端口的适配。
            #找不到就返回false，不能用9527这个端口适配。
            
            #慢慢的就不在暴露微服务本来的接口8001.转而使用统一网关9527.
            
        #为什么是/get/**：因为后面会传参数：/get/{id}
```

##### 4、看图

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220160935.png)

9527 网关就挡在了这两个方法前面，那么当你想要访问里面的这个 http://localhost:9527/payment/get/1，就要套一个网关。

##### 5、测试

![image-20210220161139916](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220161139.png)

我们通过 9527 端口也可以访问到 /payment/get/1

#### 5）另一种方式配置 Gateway 网关路由

##### 1、代码配置

代码中注入RouteLocator的Bean

业务需求：通过 9527 网关访问到百度新闻的网址：

http://news.baidu.com/guonei

在config包下创建一个配置类    

路由规则是：我现在访问 /guonei，将会转发到

http://news.baidu.com/guonei

同理也可以配置“国际”以及其他的页面

```java
@Configuration
public class GateWayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder){
        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
        routes.route("path_route_lhq",
                r -> r.path("/guonei")
                        .uri("http://news.baidu.com/"))
                .route("path_route_lhq2"
                        ,r->r.path("/guoji")
                        .uri("http://news.baidu.com/"))
                .route("path_route_lhq3"
                        ,r->r.path("/mil")
                                .uri("http://news.baidu.com/"))
                .build();
        return routes.build();
    }

}
```

![image-20210220161558513](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220161558.png)

## 五、通过微服务名实现动态路由

### 1、原来存在的问题

![image-20210220170723102](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220170723.png)

面临问题：

我们一个路由规则只对应一个接口方法，即我们把请求地址写死了。

试想一下：在分布式集群的情况下，会有多少个主机，多少个端口，多少个接口？       

难道我们要为每一个接口都定义一个路由规则吗？

解决思路：

我们前面用80调用8001和8002中的接口时，只认微服务名。访问接口时没有指定哪个端口。

那么我们在定义路由规则时也可以通过微服务名实现动态路由和负载均衡。

默认情况下Gateway会根据注册中心注册的服务列表，以注册中心上微服务名为路径创建动态路由进行转发，从而实现动态路由的功能。

### 2、修改代码

我们将 yml 改为

```yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true # 开启从注册中心动态创建路由的功能，利用为服务名进行路由。
      routes:   # 可以为controller中的所有rest接口做路由
        - id: payment_routh           # 路由id：payment_route，没有固定规则，建议配合服务名
#          uri: http://localhost:8001  # 匹配后提供服务的路由地址
          uri: lb://cloud-payment-service  # lb://开头代表从注册中心中获取服务，后面接的就是你需要转发到的服务名称
          predicates:
            - Path=/payment/get/**    # 断言：路径相匹配的进行路由

        - id: payment_routh2
#          uri: http://localhost:8001
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/**
        - id: path_route_lhq4
          uri: http://news.baidu.com/
          predicates:
            - Path=/ent

eureka:
  instance:
    hostname: cloud-gateway-service
    prefer-ip-address: true
    instance-id: cloud-gateway9527
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      #      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      defaultZone: http://localhost:7001/eureka  #单机版
```

注意：

uri: lb://cloud-payment-service        

lb://开头代表从注册中心中获取服务，后面接的就是你需要转发到的服务名称。

而且找到的服务我还要实现负载均衡。

配置好这个 yml 后，就会去注册中心找 cloud-payment-service 微服务，然后根据默认的负载均衡算法。 选择一个微服务去调用里面的接口。

SpringCloudNetFlixRibbon 会在定义 lb 前缀的目标 URL 上实现负载均衡。

我们之前是通过 8000 访问 8001/8002 ，在 8000 中实现了负载均衡。

配置好后的路由规则：

发送localhost:9527/xx/xxx请求，会去找cloud-payment-service服务名中对应微服务实例。

再根据具体的路径，找的具体的方法接口，并且开启了负载均衡。

### 3、测试

启动 9527

访问 http://localhost:9527/payment/lb

![image-20210220171252216](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220171252.png)

![image-20210220171304796](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220171304.png)

访问 http://localhost:9527/payment/get/1

![image-20210220171338328](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220171338.png)

![image-20210220171359051](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220171359.png)

## 六、Predicate 的使用

### 1、是什么

启动 GateWay9527 我们发现使用 PredicateFactory 加载了红框包围的内容。

![image-20210222101842286](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222101842.png)

每一个断言 Predicate 都有它独特的规则，多个 Predicate 断言是一个与 & 组合。

### 2、使用其他断言

#### 1）After Route Predicate Factory

> 之后的时间能访问

##### 1、获得当前时区的时间

```java
public class T2 {
    public static void main(String[] args) {
        ZonedDateTime now = ZonedDateTime.now();
        System.out.println(now);
    }
}
```

![image-20210222103001662](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222103001.png)

##### 2、配 yml

![image-20210222103842924](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222103843.png)

在原来的基础上加上红线划的部分

##### 3、测试

如果时间没到，则直接404

![image-20210222104004748](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222104004.png)

时间到了正常访问

![image-20210222104119152](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222104119.png)

#### 2）Before Route Predicate

> 之前的时间能访问

#### 3）Between Route Predicate

> 之间的时间能访问

> Cookie Route Predicate 需要两个参数，一个是 Cookie name，一个是正则表达式。
>
> 路由规则会通过获取对应的 Cookie name 值和正则表达式去匹配，如果匹配上就会执行路由，如果没有匹配就不执行

#### 4）Cookie Route Predicate

##### 1、配 yml

![image-20210222105234904](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222105234.png)

加上红线划的部分

逗号前为键，逗号后为值

##### 2、测试

> 使用 curl 测试

未携带 cookie

![image-20210222105422565](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222105422.png)

携带正确的 cookie

curl http://localhost:9527/payment/lb --cookie "username=zzyy"

![image-20210222105459695](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222105459.png)

#### 5）Header Route Predicate Factory

> 两个参数：一个是属性名称和一个正则表达式，这个属性值和正则表达式匹配则执行。

##### 1、配 yml

![image-20210222110742868](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222110742.png)

加上红线划的部分

##### 2、测试

curl http://localhost:9527/payment/lb -H "X-Request-Id:123"

![image-20210222111201812](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222111201.png)

#### 6）Host Route Predicate

添加 host 过滤

```yaml
- Host=**.lhq.com
```

#### 7）Method route Predicate

```yaml
- Method=GET
```

#### 8）Path Route Predicate

```yaml
- Path=/payment/lb/**
```

#### 9）Query Route Predicate

```yaml
- Query=username, \d+ # 要有参数名 username 并且值还要是整数才能路由
```

http://localhost:9527/payment/lb?username=31

http://localhost:9527/payment/lb?username=-31

### 3、总结

> 断言就是为了实现一组匹配规则，让请求过来找到对应的 Route 进行处理。

全部配置

```yaml
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true # 开启从注册中心动态创建路由的功能，利用为服务名进行路由。
      routes:   # 可以为controller中的所有rest接口做路由
        - id: payment_routh           # 路由id：payment_route，没有固定规则，建议配合服务名
#          uri: http://localhost:8001  # 匹配后提供服务的路由地址
          uri: lb://cloud-payment-service  # lb://开头代表从注册中心中获取服务，后面接的就是你需要转发到的服务名称
          predicates:
            - Path=/payment/get/**    # 断言：路径相匹配的进行路由

        - id: payment_routh2
#          uri: http://localhost:8001
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/**
            - After=2021-02-22T10:28:20.777+08:00[Asia/Shanghai]
            - Before=2021-02-22T10:28:20.777+08:00[Asia/Shanghai]
            - Between=2021-02-22T10:28:20.777+08:00[Asia/Shanghai],2021-02-22T11:28:20.777+08:00[Asia/Shanghai]
            - Cookie=username,zzyy
            - Header=X-Request-Id, \d+ # 请求头要有 X-Request-Id 属性，并且值为整数的正则表达式
            - Host=**.lhq.com
            - Method=GET
            - Query=username, \d+ # 要有参数名 username 并且值还要是整数才能路由
	
```



## 七、Filter 的使用

### 1、是什么

路由过滤器可用于修改进入的 HTTP 请求和返回的 HTTP 响应，路由过滤器只能指定路由进行使用。

Spring Cloud Gateway 内置了多种路由过滤器，他们都由 GatewayFilter 的工厂类来产生。

### 2、Spring Cloud Gateway 的 Filter

#### 1）生命周期

pre（业务逻辑之前），post（业务逻辑之后）

#### 2）种类

GatewayFilter（单一的），Global Filter（全局的）

#### 3）常用的 GatewayFilter

例子 

```yml
filters:
        - AddRequestHeader=X-Request-red, blue
```

https://docs.spring.io/spring-cloud-gateway/docs/2.2.7.RELEASE/reference/html/#gatewayfilter-factories

#### 4）自定义过滤器

两个主要接口介绍：implements GlobalFilter, Ordered

##### 1、定义一个全局过滤器

```java
@Component
@Slf4j
public class MyLogGateWayFilter implements GlobalFilter, Ordered {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("**************come in MyLogGateWayFilter:" + new Date());
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if (uname == null){
            log.info("******用户名为null，非法用户，o(T_T)o");
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return 0;
    }

}
```

##### 2、测试

成功链接

http://localhost:9527/payment/get/1?uname=55

失败链接

http://localhost:9527/payment/get/1