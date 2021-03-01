* [SpringCloud Sleuth 分布式请求链路跟踪](#springcloud-sleuth-%E5%88%86%E5%B8%83%E5%BC%8F%E8%AF%B7%E6%B1%82%E9%93%BE%E8%B7%AF%E8%B7%9F%E8%B8%AA)
  * [一、概述](#%E4%B8%80%E6%A6%82%E8%BF%B0)
    * [1、为什么会出现这个技术？需要解决哪些问题？](#1%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BC%9A%E5%87%BA%E7%8E%B0%E8%BF%99%E4%B8%AA%E6%8A%80%E6%9C%AF%E9%9C%80%E8%A6%81%E8%A7%A3%E5%86%B3%E5%93%AA%E4%BA%9B%E9%97%AE%E9%A2%98)
    * [2、是什么](#2%E6%98%AF%E4%BB%80%E4%B9%88)
  * [二、搭建链路监控步骤](#%E4%BA%8C%E6%90%AD%E5%BB%BA%E9%93%BE%E8%B7%AF%E7%9B%91%E6%8E%A7%E6%AD%A5%E9%AA%A4)
    * [1、zipkin](#1zipkin)
      * [1）下载](#1%E4%B8%8B%E8%BD%BD)
      * [2）运行 jar](#2%E8%BF%90%E8%A1%8C-jar)
      * [3）运行控制台](#3%E8%BF%90%E8%A1%8C%E6%8E%A7%E5%88%B6%E5%8F%B0)
        * [完整的调用链路](#%E5%AE%8C%E6%95%B4%E7%9A%84%E8%B0%83%E7%94%A8%E9%93%BE%E8%B7%AF)
        * [名词解释](#%E5%90%8D%E8%AF%8D%E8%A7%A3%E9%87%8A)
    * [2、服务提供者](#2%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）PaymentController](#3paymentcontroller)
    * [3、服务消费者（调用方）](#3%E6%9C%8D%E5%8A%A1%E6%B6%88%E8%B4%B9%E8%80%85%E8%B0%83%E7%94%A8%E6%96%B9)
      * [1）pom\.xml](#1pomxml-1)
      * [2）application\.yml](#2applicationyml-1)
      * [3）OrderController](#3ordercontroller)
    * [4、测试](#4%E6%B5%8B%E8%AF%95)
      * [1）启动](#1%E5%90%AF%E5%8A%A8)
      * [2）访问](#2%E8%AE%BF%E9%97%AE)
        * [页面](#%E9%A1%B5%E9%9D%A2)
        * [查看依赖关系](#%E6%9F%A5%E7%9C%8B%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB)
        * [原理图](#%E5%8E%9F%E7%90%86%E5%9B%BE)

# SpringCloud Sleuth 分布式请求链路跟踪

## 一、概述

### 1、为什么会出现这个技术？需要解决哪些问题？

在微服务架构中，一个客户端发起的请求在后端系统中会经过多个不同的服务节点调用来协同产生最后的请求结果，每一个前段请求都会形成一条复杂的分布式服务调用链路，链路中的任何一环出现高延时或者错误都会引起整个请求最后的失败。

![image-20210301111243281](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301111243.png)

![image-20210301111259808](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301111259.png)

### 2、是什么

https://github.com/spring-cloud/spring-cloud-sleuth

Spring Cloud Sleuth 提供了一套完整的服务跟踪的解决方案

在分布式系统中提供追踪解决方案并且兼容支持了 zipkin

## 二、搭建链路监控步骤

### 1、zipkin

#### 1）下载

下载地址

https://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/

Spring Cloud 从 F 版起已不需要自己构建 Zipkin Server 了，只需要调用 jar 包即可

#### 2）运行 jar

```shell
java -jar zipkin-server-2.12.9-exec.jar
```

![image-20210301094834585](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301094834.png)

#### 3）运行控制台

http://localhost:9411/zipkin/

##### 完整的调用链路

标识一请求链路，一条链路通过 Trace Id 唯一标识，Span 标识发起的请求信息，各 span 通过 parent id 关联起来

![image-20210301095559714](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301095559.png)

上图解释

一条链路通过 Trace Id 唯一标识，Span 标识发起的请求信息，各 span 通过 parent id 关联起来

![image-20210301095734306](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301095734.png)

整个链路的依赖关系如下：

![image-20210301095810471](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301095810.png)

##### 名词解释

Trace：类似于树结构的 Span 集合，表示一条调用链路，存在唯一标识

Span：表示调用链路来源，通俗的理解 span 就是一次请求信息

### 2、服务提供者

修改 cloud-provider-payment8001

#### 1）pom.xml

增加 zipkin 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

```xml
<dependencies>
    <!--        包含了 sleuth + zipkin-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zipkin</artifactId>
    </dependency>

    <dependency>
        <groupId>com.lhq.cloud2020</groupId>
        <artifactId>cloud-api-commons</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

    <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.1.10</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
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

#### 2）application.yml

增加 zipkin 的配置

```yml
zipkin:
  base-url: http://localhost:9411
sleuth:
  sampler:
    # 采样率值介于 0 到 1 之间，1表示全部采集
    probability: 1
```

```yml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      # 采样率值介于 0 到 1 之间，1表示全部采集
      probability: 1
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.183.101:3306/db2020?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: root


eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
#      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      defaultZone: http://localhost:7001/eureka  #单机版
  instance:
    instance-id: payment8001
    prefer-ip-address: true #访问路径显示ip
    #Eureka 客户端向服务器发送心跳的时间间隔，单位是秒（默认30秒）
    lease-renewal-interval-in-seconds: 1
    #Eureka 服务端在收到最后一次心跳后等待时间上限，单位为秒（默认90秒），超时将剔除服务
    lease-expiration-duration-in-seconds: 2
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

#### 3）PaymentController

增加 

```java
@GetMapping(value = "/payment/zipkin")
public String zipkin(){
    return "hi,i'm paymentzipkin server fall back";
}
```

```java
package com.lhq.cloud2020.controller;

import com.lhq.cloud2020.entities.CommonResult;
import com.lhq.cloud2020.entities.Payment;
import com.lhq.cloud2020.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.util.ObjectUtils;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;
import java.util.List;
import java.util.concurrent.TimeUnit;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @Autowired
    private DiscoveryClient discoveryClient;

    @Value("${server.port}")
    private String serverPort;
    @PostMapping("/payment/create")
    public CommonResult<Payment> create(@RequestBody Payment payment){
        log.info("绑定结果："+payment);
        int result = paymentService.create(payment);
        log.info("*****插入结果:"+result);

        if (result > 0){
            return new CommonResult(200,"插入数据库成功,serverPort:"+serverPort,result);
        } else {
            return new CommonResult(444,"插入数据库失败",null);
        }
    }
    @GetMapping("/payment/get/{id}")
    public CommonResult get(@PathVariable("id") Long id){
        Payment result = paymentService.getPaymentById(id);
        log.info("*****查找结果:"+result);
        if (!ObjectUtils.isEmpty(result)){
            return new CommonResult(200,"查找成功,serverPort:"+serverPort,result);
        } else {
            return new CommonResult(444,"查找失败，对应ID："+id,null);
        }
    }
    @GetMapping(value = "/payment/discovery")
    public Object discover(){
        List<String> services = discoveryClient.getServices();
        for (String element : services){
            log.info("****element:"+element);
        }
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance : instances){
            log.info(instance.getServiceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
        }
        return this.discoveryClient;
    }
    @GetMapping(value = "/payment/lb")
    public String getPaymentLB(){
        return serverPort;
    }

    @GetMapping(value = "/payment/feign/timeout")
    public String paymentFeignTimeout(){
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return serverPort;
    }

    @GetMapping(value = "/payment/zipkin")
    public String zipkin(){
        return "hi,i'm paymentzipkin server fall back";
    }
}
```

### 3、服务消费者（调用方）

修改 cloud-consumer-order8000

#### 1）pom.xml

增加

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zipkin</artifactId>
    </dependency>
    <dependency>
        <groupId>com.lhq.cloud2020</groupId>
        <artifactId>cloud-api-commons</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web  -->
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

#### 2）application.yml

增加 

```yml
zipkin:
  base-url: http://localhost:9411
sleuth:
  sampler:
    # 采样率值介于 0 到 1 之间，1表示全部采集
    probability: 1
```

```yml
server:
  port: 8000
spring:
  application:
    name: cloud-order-service
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      # 采样率值介于 0 到 1 之间，1表示全部采集
      probability: 1
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka #单机版
#      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
```

#### 3）OrderController

增加

```java
@GetMapping("/consumer/payment/zipkin")
public String paymentZipkin(){
    String result = restTemplate.getForObject("http://localhost:8001" + "/payment/zipkin/", String.class);
    return result;
}
```

```java
package com.lhq.cloud2020.controller;

import com.lhq.cloud2020.entities.CommonResult;
import com.lhq.cloud2020.entities.Payment;
import com.lhq.cloud2020.lb.LoadBalancer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;
import java.net.URI;
import java.util.List;

@RestController
public class OrderController {

    //public static final String PROVIDER_URL = "http://localhost:8001";
    public static final String PROVIDER_URL = "http://CLOUD-PAYMENT-SERVICE/";

    @Resource
    RestTemplate restTemplate;

    @Autowired
    private LoadBalancer loadBalancer;

    @Autowired
    private DiscoveryClient discoveryClient;
    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment){
        return restTemplate.postForObject(PROVIDER_URL+"payment/create",payment,CommonResult.class);
    }

    @GetMapping("/consumer/payment/{id}")
    public CommonResult<Payment> getPayment1(@PathVariable("id") Long id){
        return restTemplate.getForObject(PROVIDER_URL+"payment/get/"+id,CommonResult.class);
    }
    @GetMapping("/consumer/payment/getForEntity/{id}")
    public CommonResult<Payment> getPayment2(@PathVariable("id") Long id){
        ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PROVIDER_URL+"payment/get/"+id,CommonResult.class);
        if (entity.getStatusCode().is2xxSuccessful()){
            return entity.getBody();
        } else {
            return new CommonResult<>(444,"操作失败");
        }
    }

    @GetMapping(value = "/consumer/payment/lb")
    public String getPaymentLB(){
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        if (instances == null || instances.size() <= 0){
            return null;
        }
        ServiceInstance serviceInstance = loadBalancer.instances(instances);
        URI uri = serviceInstance.getUri();
        return restTemplate.getForObject(uri + "/payment/lb",String.class);
    }

    @GetMapping("/consumer/payment/zipkin")
    public String paymentZipkin(){
        String result = restTemplate.getForObject("http://localhost:8001" + "/payment/zipkin/", String.class);
        return result;
    }

}
```

### 4、测试

#### 1）启动

eureka7001

payment8001

order8000

使用 8000 调用 8001

#### 2）访问

打开浏览器访问：http://localhost:9411

##### 页面

![image-20210301105125531](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301105125.png)

![image-20210301105204800](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301105204.png)

##### 查看依赖关系

![image-20210301105249693](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301105249.png)

##### 原理图

![image-20210301105330176](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210301105330.png)