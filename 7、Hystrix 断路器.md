* [Hystrix 断路器](#hystrix-%E6%96%AD%E8%B7%AF%E5%99%A8)
  * [一、概述](#%E4%B8%80%E6%A6%82%E8%BF%B0)
    * [1、Hystrix 是什么](#1hystrix-%E6%98%AF%E4%BB%80%E4%B9%88)
    * [2、Hystrix 能干嘛](#2hystrix-%E8%83%BD%E5%B9%B2%E5%98%9B)
  * [二、概念](#%E4%BA%8C%E6%A6%82%E5%BF%B5)
    * [1、服务降级](#1%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7)
    * [2、服务熔断](#2%E6%9C%8D%E5%8A%A1%E7%86%94%E6%96%AD)
    * [3、服务限流](#3%E6%9C%8D%E5%8A%A1%E9%99%90%E6%B5%81)
  * [三、使用步骤](#%E4%B8%89%E4%BD%BF%E7%94%A8%E6%AD%A5%E9%AA%A4)
    * [1、新建 cloud\-provider\-hystrix\-payment8001](#1%E6%96%B0%E5%BB%BA-cloud-provider-hystrix-payment8001)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）业务类](#4%E4%B8%9A%E5%8A%A1%E7%B1%BB)
      * [5）控制类](#5%E6%8E%A7%E5%88%B6%E7%B1%BB)
    * [2、新建 cloud\-consumer\-feign\-hystrix\-order8000](#2%E6%96%B0%E5%BB%BA-cloud-consumer-feign-hystrix-order8000)
      * [1）pom\.xml](#1pomxml-1)
      * [2）application\.yml](#2applicationyml-1)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
      * [4）业务类](#4%E4%B8%9A%E5%8A%A1%E7%B1%BB-1)
      * [5）控制类](#5%E6%8E%A7%E5%88%B6%E7%B1%BB-1)
  * [四、服务降级容错解决的维度要求](#%E5%9B%9B%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7%E5%AE%B9%E9%94%99%E8%A7%A3%E5%86%B3%E7%9A%84%E7%BB%B4%E5%BA%A6%E8%A6%81%E6%B1%82)
    * [1、超时](#1%E8%B6%85%E6%97%B6)
    * [2、出错（宕机或程序运行出错）](#2%E5%87%BA%E9%94%99%E5%AE%95%E6%9C%BA%E6%88%96%E7%A8%8B%E5%BA%8F%E8%BF%90%E8%A1%8C%E5%87%BA%E9%94%99)
    * [3、OK](#3ok)
  * [五、服务降级](#%E4%BA%94%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7)
    * [1、降级配置](#1%E9%99%8D%E7%BA%A7%E9%85%8D%E7%BD%AE)
    * [2、全局服务降级 DefaultProperties](#2%E5%85%A8%E5%B1%80%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7-defaultproperties)
    * [3、通配服务降级 FeignFallBack](#3%E9%80%9A%E9%85%8D%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7-feignfallback)
      * [1）通过实现 PaymentHystrixService 接口](#1%E9%80%9A%E8%BF%87%E5%AE%9E%E7%8E%B0-paymenthystrixservice-%E6%8E%A5%E5%8F%A3)
      * [2）在 PaymentHystrixService 上添加](#2%E5%9C%A8-paymenthystrixservice-%E4%B8%8A%E6%B7%BB%E5%8A%A0)
  * [六、服务熔断](#%E5%85%AD%E6%9C%8D%E5%8A%A1%E7%86%94%E6%96%AD)
    * [1、是什么](#1%E6%98%AF%E4%BB%80%E4%B9%88)
    * [2、案例](#2%E6%A1%88%E4%BE%8B)
      * [1）修改 cloud\-provider\-hystrix\-payment8001 的 service](#1%E4%BF%AE%E6%94%B9-cloud-provider-hystrix-payment8001-%E7%9A%84-service)
      * [2）修改 cloud\-provider\-hystrix\-payment8001 中的 controller 层](#2%E4%BF%AE%E6%94%B9-cloud-provider-hystrix-payment8001-%E4%B8%AD%E7%9A%84-controller-%E5%B1%82)
      * [3）测试](#3%E6%B5%8B%E8%AF%95)
    * [3、总结](#3%E6%80%BB%E7%BB%93)
      * [1）熔断类型](#1%E7%86%94%E6%96%AD%E7%B1%BB%E5%9E%8B)
      * [2）断路器默认开启的条件](#2%E6%96%AD%E8%B7%AF%E5%99%A8%E9%BB%98%E8%AE%A4%E5%BC%80%E5%90%AF%E7%9A%84%E6%9D%A1%E4%BB%B6)
      * [3）熔断器开启式，主逻辑怎么恢复呢？](#3%E7%86%94%E6%96%AD%E5%99%A8%E5%BC%80%E5%90%AF%E5%BC%8F%E4%B8%BB%E9%80%BB%E8%BE%91%E6%80%8E%E4%B9%88%E6%81%A2%E5%A4%8D%E5%91%A2)
  * [七、Hystrix 工作流程](#%E4%B8%83hystrix-%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
  * [八、服务监控 HystrixDashboard](#%E5%85%AB%E6%9C%8D%E5%8A%A1%E7%9B%91%E6%8E%A7-hystrixdashboard)
    * [1、概述](#1%E6%A6%82%E8%BF%B0)
    * [2、新建 cloud\-consumer\-hystrix\-dashboard](#2%E6%96%B0%E5%BB%BA-cloud-consumer-hystrix-dashboard)
      * [1）pom\.xml](#1pomxml-2)
      * [2）application\.yml](#2applicationyml-2)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-2)
      * [4）修改 cloud\-provider\-hystrix\-payment8001 的主启动类](#4%E4%BF%AE%E6%94%B9-cloud-provider-hystrix-payment8001-%E7%9A%84%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [5）启动 9002](#5%E5%90%AF%E5%8A%A8-9002)
      * [6）启动 8001](#6%E5%90%AF%E5%8A%A8-8001)
    * [3、如何看此图](#3%E5%A6%82%E4%BD%95%E7%9C%8B%E6%AD%A4%E5%9B%BE)

# Hystrix 断路器

## 一、概述

### 1、Hystrix 是什么

Hystrix 是一个用于处理分布式系统的延迟和容错的开源库，能保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，提高分布式系统的弹性。

“断路器” 当某个服务出现故障，通过故障监控，向调用方法返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间等待或者抛出调用方无法处理的异常。

### 2、Hystrix 能干嘛

服务降级、服务熔断、接近实时的监控。。。

## 二、概念

### 1、服务降级

> 介绍

服务器忙，请稍后再试，不让客户等待并立即返回一个友好提示，fallback。

> 哪些情况会触发降级？

程序运行异常

超时

服务熔断触发服务降级

线程池/信号量打满也会导致服务降级

### 2、服务熔断

> 介绍

类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示。

> 类似保险丝

服务的降级 -> 进而熔断 -> 恢复调用链路

### 3、服务限流

秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒N个，有序进行。

## 三、使用步骤

> 生产者端服务降级

### 1、新建 cloud-provider-hystrix-payment8001

#### 1）pom.xml

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

    <artifactId>cloud-provider-hystrix-payment8001</artifactId>
    <dependencies>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--引入自定义的api通用包，可用使用Payment支付Entity-->
        <dependency>
            <groupId>com.lhq.cloud2020</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
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

</project>
```

#### 2）application.yml

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-provider-hystrix-payment

eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
#      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      defaultZone: http://localhost:7001/eureka
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaClient
//激活 @HystrixCommand 注解
@EnableCircuitBreaker
public class PaymentHystrixMain8001 {
    public static void main(String[] args){
  		SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
}
```

#### 4）业务类

```java
@Service
public class PaymentService {
    /**
     * 正常访问，肯定OK
     * @param id
     * @return
     */
    public String paymentInfo_OK(Integer id){
        return "线程池：" + Thread.currentThread().getName() + "paymentInfo_OK,id：" + id;
    }
	
    //设置服务降级方法，fallbackMethod 指定 paymentInfo_TimeOutHandler 为降级方法，commandProperties 是一个数组
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",commandProperties = {
            //三秒超时
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public String paymentInfo_TimeOut(Integer id){
        int timeNumber = 13;
        //int age = 10/0;
        try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "线程池：" + Thread.currentThread().getName() + "paymentInfo_TimeOut,id：" + id +"，耗时 " + timeNumber + " 秒钟";
    }
	
    //降级方法
    public String paymentInfo_TimeOutHandler(Integer id){
        return "线程池：" + Thread.currentThread().getName() + "系统繁忙或运行报错，请稍后再试" + id +"\t"+"【兜底】";
    }

}
```

#### 5）控制类

```java
@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String s = paymentService.paymentInfo_OK(id);
        log.info("****result:" + s);
        return s;
    }

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String s = paymentService.paymentInfo_TimeOut(id);
        log.info("****result:" + s);
        return s;
    }

}
```

### 2、新建 cloud-consumer-feign-hystrix-order8000

> 消费者端服务降级

#### 1）pom.xml

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

    <artifactId>cloud-consumer-feign-hystrix-order8000</artifactId>
    <dependencies>
        <!--openfeign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--引入自定义的api通用包，可用使用Payment支付Entity-->
        <dependency>
            <groupId>com.lhq.cloud2020</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
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

</project>
```

#### 2）application.yml

```yaml
server:
  port: 8000
spring:
  application:
    name: cloud-consumer-hystrix-order
eureka:
  client:
    register-with-eureka: true
    service-url:
      defaultZone: http://localhost:7001/eureka #单机版
feign:
  hystrix:
    enabled: true #开启服务降级
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableFeignClients
@EnableHystrix
public class OrderHystrixMain8000 {
    public static void main(String[] args){
        SpringApplication.run(OrderHystrixMain8000.class,args);
    }
}
```

#### 4）业务类

```java
@Component
@FeignClient("CLOUD-PROVIDER-HYSTRIX-PAYMENT")
public interface PaymentHystrixService {

    @GetMapping("/payment/hystrix/ok/{id}")
    String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    String paymentInfo_TimeOut(@PathVariable("id") Integer id);

}
```

#### 5）控制类

```java
@RestController
public class OrderHystrixController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    String paymentInfo_OK(@PathVariable("id") Integer id){
        return paymentHystrixService.paymentInfo_OK(id);
    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",commandProperties = {
            //三秒超时
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        return paymentHystrixService.paymentInfo_TimeOut(id);
    }

    public String paymentInfo_TimeOutHandler(Integer id){
        return "我是消费者8000，对方支付系统繁忙请10秒后再试或者自己运行出错请检查自己";
    }

}
```

## 四、服务降级容错解决的维度要求

### 1、超时

如果服务超时，调用者不能一直卡死等待，必须要有服务降级。

### 2、出错（宕机或程序运行出错）

对方服务宕机，调用者不能一直卡死等待，必须要有服务降级。

### 3、OK

对方服务 OK ，调用者出故障或有自我要求（自己的等待时间小于服务提供者），自己处理降级。

## 五、服务降级

### 1、降级配置

添加 @HystrixCommand 注解

添加 @EnableCircuitBreaker 激活注解

1）8001

8001 设置自身超时时间的峰值，峰值内可以正常运行，超过了需要有兜底的方法处理，作为服务降级 fallback。

paymentInfo_TimeOutHandler 为兜底方案

注意事项：

配置的热部署对 java 代码修改敏感，但是对 @HystrixCommand 内的属性修改不敏感。

2）8000

订单微服务也可以更好的保护自己

在 yml 里配置 hystrix 开启

在主启动类上添加注解 @EnableHystrix

### 2、全局服务降级 DefaultProperties

> 解决代码膨胀问题

编写降级方法

修改 OrderHystrixController

```java
@RestController
/** 定义默认降级兜底方法为 payment_Global_FallbackMethod；
*	只要加上 @HystrixCommand 注解，如果不指定 fallbackMethod
*	默认降级走 payment_Global_FallbackMethod
*/
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")
public class OrderHystrixController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    String paymentInfo_OK(@PathVariable("id") Integer id){
        return paymentHystrixService.paymentInfo_OK(id);
    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    //@HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler",commandProperties = {
    //        //三秒超时
    //        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1500")
    //})
    @HystrixCommand
    String paymentInfo_TimeOut(@PathVariable("id") Integer id){

        int age = 10 / 0;
        return paymentHystrixService.paymentInfo_TimeOut(id);
    }

    public String paymentInfo_TimeOutHandler(Integer id){
        return "我是消费者8000，对方支付系统繁忙请10秒后再试或者自己运行出错请检查自己";
    }

    // 添加降级方法
    public String payment_Global_FallbackMethod(){
        return "Global 异常处理信息，请稍后再试，/(ToT)/~";
    }
    
}
```

### 3、通配服务降级 FeignFallBack

> 解决代码混乱问题

服务降级，客户端去调用服务端，碰到服务端宕机或关闭

本次案例服务降级处理实在客户端 8000 实现的，与服务端 8001 没有关系，只需要为 Feign 客户端定义的接口添加一个服务降级处理的实现类即可实现解耦。

未来我们面对的异常：运行、超时、宕机

#### 1）通过实现 PaymentHystrixService 接口

> 因为原来接口是通过远程调用来进行业务处理的，当被调用的服务宕机后，此类将作为降级处理类，每个方法对应降级的方法。

```java
@Component
public class PaymentFallbackService implements PaymentHystrixService {
    
    @Override
    public String paymentInfo_OK(Integer id) {
        return "-----PaymentFallbackService fall back paymentInfo_OK";
    }

    @Override
    public String paymentInfo_TimeOut(Integer id) {
        return "-----PaymentFallbackService fall back paymentInfo_TimeOut";
    }
    
}
```

#### 2）在 PaymentHystrixService 上添加

```java
// fallback 指定降级的类
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)
```

## 六、服务熔断

### 1、是什么

熔断机制是应对雪崩效应的一种微服务链路保护机制。当扇出链路的某个微服务出错不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的相应信息。**当检测到该节点微服务调用响应正常后，恢复调用链路**。

在 Spring Cloud 框架里，熔断机制通过 Hystrix 实现。Hystrix 会监控微服务间调用的情况，当失败的调用到一定阈值，缺省 5 秒内 20 次调用失败，就会启动熔断机制。熔断机制的注解是 @HystrixCommand。

半开状态：

比如说一个微服能承受100的并发量，某一时刻有500人同时访问。该服务就会崩掉，熔断，直接当掉。

外部此时不能访问了，过了一会，发现没有那么高的并发量了。感觉并发量在我的承受之内了(100)。比如1s/72次，那么就试着这放开这些并发请求。

放着放着发现能够适应当前的并发量了，我再把闸道合上。                

放着放着的状态就是半开状态，然后再把断路器合上变成打开的状态。

![image-20210220093901257](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220093901.png)

### 2、案例

#### 1）修改 cloud-provider-hystrix-payment8001 的 service

添加如下代码

```java
 // ----- 服务熔断
@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
    @HystrixProperty(name = "circuitBreaker.enabled",value = "true"), // 是否开启断路器
    @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"), // 请求次数
    @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"), // 时间窗口期
    @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60") // 失败率达到多少后跳闸
})
public String paymentCircuitBreaker(@PathVariable("id") Integer id) {
    if (id < 0) {
        throw new RuntimeException("*******id 不能负数");
    }
    String serialNumber = IdUtil.simpleUUID();
    return Thread.currentThread().getName() + "\t" + "调用成功，流水号：" + serialNumber;
}

public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id) {
    return "id 不能为负数，请稍后再试， /(ToT)/~~  id:" + id;
}
```

#### 2）修改 cloud-provider-hystrix-payment8001 中的 controller 层

添加以下代码

```java
//===== 服务熔断
@GetMapping("/payment/circuit/{id}")
public String paymentCircuitBreaker(@PathVariable("id") Integer id){
    String result = paymentService.paymentCircuitBreaker(id);
    log.info("****result:" + result);
    return result;
}
```

#### 3）测试

启动 7001 和 8001

访问 http://localhost:8001/payment/circuit/1

![image-20210220102843344](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220102843.png)

访问 http://localhost:8001/payment/circuit/-1

![image-20210220102824340](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220102824.png)

多次访问 http://localhost:8001/payment/circuit/-1 

之后访问 http://localhost:8001/payment/circuit/1

![image-20210220103006946](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220103006.png)

等待一段时间再访问 http://localhost:8001/payment/circuit/1

![image-20210220103047911](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220103047.png)

### 3、总结

#### 1）熔断类型

1、熔断打开

请求不再进行调用当前服务，再有请求调用时将不会调用主逻辑，而是直接调用降级 fallback。实现 了自动地发现错误并将降级逻辑切换为主逻辑，减少响应延迟效果。内部设置时钟一般为 MTTR（平均故障处理时间），当打开熔断时长达到所设时钟则进入半熔断状态。

2、熔断关闭 

熔断关闭后，服务正常开始调用。                

3、熔断半开       

部分请求根据规则调用当前服务，如果请求成功且符合规则则认为当前服务回复正常，关闭熔断。           

#### 2）断路器默认开启的条件            

1、当（失败频率）满足一定的阈值时： > 20次/10s                	2、 当失败率达到一定的时候：10s内50%的失败请求。                

注意：两个条件要都满足，才会开启断路器。

比如10s内出错21次，正确100次。            

满足情况1，不满足情况2.                        

断路器也不会开启。                    

3、一段时间后，默认是5s，这个时候断路器从开启变成半开状态。会放行一些请求，

如果成功，断路器关闭，若失败继续开启。

#### 3）熔断器开启式，主逻辑怎么恢复呢？

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220103634.png)

## 七、Hystrix 工作流程

官网：https://github.com/Netflix/Hystrix/wiki/How-it-Works

官网流程图

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220104400.png)

## 八、服务监控 HystrixDashboard

### 1、概述

除了隔离依赖服务的调用以外，Hystrix 还提供了准实时的调用监控（Hystrix Dashboard），Hystrix 会持续地记录所有通过 Hystrix  发起的请求执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求，多少成功，多少失败等。Netflix 通过 Hystrix-metrics-event-stream 项目实现了对以上指标的监控。Spring Cloud 也提供了 Hystrix Dashboard 的整合，对监控内容转化成可视化界面。

### 2、新建 cloud-consumer-hystrix-dashboard

#### 1）pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
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

```yaml
server:
  port: 9002
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardMain9001 {
    public static void main(String[] args){
        SpringApplication.run(HystrixDashboardMain9001.class);
    }
}
```

#### 4）修改 cloud-provider-hystrix-payment8001 的主启动类

```java
@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker
public class PaymentHystrixMain8001 {

    public static void main(String[] args){
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }

    /**
     * 此配置是为了服务监控而配置，与服务容错本身无关，SpringCloud升级后的坑
     * ServletRegistrationBean因为springboot的默认路径不是“/hystrix.stream”，
     * 只要在自己的项目里配置夏敏的servlet即可。
     * @return
     */
    @Bean
    public ServletRegistrationBean getServlet(){
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        //9002监控8001的监控地址
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}
```

#### 5）启动 9002

访问 http://localhost:9002/hystrix

![image-20210220112936055](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220112936.png)

#### 6）启动 8001

1、测试监控

http://localhost:8001/hystrix.stream

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113140.png)

2、测试地址

http://localhost:8001/payment/circuit/-1

![image-20210220113457396](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113457.png)

http://localhost:8001/payment/circuit/1

![image-20210220113428572](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113505.png)

3、打开监控

![image-20210220113525578](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113525.png)

### 3、如何看此图

![img](C:%5CUsers%5C13014%5CDesktop%5C20210220113550.png)

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113606.png)

实心圆：

颜色变化代表了实例的健康程度，它的健康程度从绿色<黄色<橙色<红色递减。            

实心圆越大说明流量越大，所以通过该实心圆的展示，就可以在大量的实例中快速的发现故障实例和高压力实例。                        曲线/折线：用来记录两分钟内流量的相对变化，可以通过它观察到流量的上升和下降趋势。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210220113704.png)