# Ribbon 负载均衡服务调用

## 一、概述

### 1、什么是 ribbon

Spring Cloud Ribbon 是基于 Netflix Ribbon 实现的一套客户端**负载均衡工具**

Ribbon 是 Netflix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。

Ribbon 客户端组件提供一系列完善的配置项如连接超时，重试等。

就是在配置文件中列出 LoadBalance （简称 LB）后面所有的机器，Ribbon 会自动的帮你基于某种规则（如简单轮询，随机连接等）去连接这些机器。

我们很容易使用 Ribbon 实现自定义的负载均衡算法。

### 2、官网资料

https://github.com/Netflix/ribbon/wiki/Getting-Started

Ribbon 目前也进入维护模式

### 3、能干嘛

#### 1）LB（负载均衡）

**LB 负载均衡（LoadBalance）是什么**

将用户请求平摊的分配到多个服务上，从而达到系统的 HA（高可用）。

常见的负载均衡有软件 Nginx、LVS、硬件 F5 等。

**Ribbon 本地负载均衡客户端与 Nginx 服务端负载均衡的区别**

Nginx 是服务器负载均衡，客户端所有请求都会交给 nginx，然后由 nginx 实现转发请求。即负载均衡是由服务端实现的。

Ribbon 本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到 JVM 本地，从而在本地实现 RPC 远程调用技术。

##### 集中式 LB

即在服务的消费方和提供方之间使用独立的 LB 设施（可以是硬件，如 F5，也可以是软件，如 nginx），由该设施负责把访问请求通过某种策略转发至服务的提供方。

##### 进程内 LB

将 LB 逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择一个合适的服务器。

Ribbon 就属于进程内 LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。

#### 2）OrderMain8000 通过轮询负载访问 8001/8002

#### 3）一句话概括

负载均衡 + RestTemplate 调用。

## 二、Ribbon 负载均衡演示

### 1、架构说明

![image-20201116172347528](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201116191122.png)

Ribbon 是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和 eureka 结合只是其中的一个实例。

### 2、pom

![image-20201117155922746](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117155922.png)

![image-20201117155932570](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117155932.png)

### 3、Rest Template 的使用

#### 1）官网

https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html

![image-20201117160024635](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117160024.png)

#### 2）GET请求方法

![image-20201117160108435](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117160108.png)

#### 3）POST请求方法

## ![image-20201117160216332](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117160216.png)三、Ribbon 核心组件 IRule

### 1、IRule 算法

根据特定算法从服务列表中选取一个要访问的服务

![image-20201117160427317](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117160427.png)

#### 1）com.netflix.loadbalancer.RoundRobinRule

轮询。

#### 2）com.netflix.loadbalancer.RandomRule

随机。

#### 3）com.netflix.loadbalancer.RetryRule

先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试。

#### 4）WeightedResponseTimeRule 

对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择

#### 5）BestAvailableRule 

会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务。

#### 6）AvailabilityFilteringRule 

先过滤掉故障实例，再选择并发较小的实例。

#### 7）ZoneAvoidanceRule

默认规则，复合判断server所在区域的性能和server的可用性选择服务器。

### 2、如何替换规则

#### **修改 cloud-consumer-order8000**

##### 1）注意配置细节

![image-20201117161407200](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117161407.png)

![image-20201117161353138](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117161353.png)

##### 2）新建 package

![image-20201117161621966](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117161622.png)

##### 3）创建规则类

```java
package com.lhq.myrule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-16 15:17
 */
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule(){
        //定义为随机
        return new RandomRule();
    }
}
```

##### 4）修改主启动类（主启动类添加@RibbonClient）

```java
package com.lhq.cloud2020;

import com.lhq.myrule.MySelfRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@EnableEurekaClient
//主启动类添加@RibbonClient
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
public class OrderMain8000 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain8000.class);
    }
}
```

##### 5）测试

http://localhost:8000/consumer/payment/get/1

## 四、Ribbon 负载均衡算法

### 1、原理

![image-20201117162149478](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201117162149.png)

### 2、手写本地负载均衡器

#### 1）启动 eureka 集群（7001/7002）

7001/7002集群启动

#### 2）服务提供者改造（8001/8002）

在 PaymentController 中添加方法

```java
@GetMapping(value = "/payment/lb")
public String getPaymentLB(){
    return serverPort;
}
```

#### 3）服务消费者改造（cloud-consumer-order8000）

1. ApplicationContextConfig 去掉 @LoadBalanced

2. 在 lb 包下新建 LoadBalancer 接口

   ```java
   package com.lhq.cloud2020.lb;
   
   import org.springframework.cloud.client.ServiceInstance;
   
   import java.util.List;
   
   /**
    * TODO
    *
    * @author 李浩琦
    * @since 2020-11-16 16:13
    */
   public interface LoadBalancer {
   
       ServiceInstance instances(List<ServiceInstance> serviceInstances);
   
   }
   ```

3. 在 lb 包下新建 MyLB 类

   ```java
   package com.lhq.cloud2020.lb;
   
   import org.springframework.cloud.client.ServiceInstance;
   import org.springframework.stereotype.Component;
   
   import java.util.List;
   import java.util.concurrent.atomic.AtomicInteger;
   
   /**
    * TODO
    *
    * @author 李浩琦
    * @since 2020-11-16 16:14
    */
   @Component
   public class MyLB implements LoadBalancer {
   
       private AtomicInteger atomicInteger = new AtomicInteger(0);
   
       public final int getAndIncrement(){
           int current;
           int next;
           do {
               current = this.atomicInteger.get();
               next = current >= 2147483647 ? 0 : current + 1;
           } while (!this.atomicInteger.compareAndSet(current,next));
           System.out.println("*****第几次访问，次数：next:" + next);
           return next;
       }
   
       @Override
       public ServiceInstance instances(List<ServiceInstance> serviceInstances) {
           int index = getAndIncrement() % serviceInstances.size();
           return serviceInstances.get(index);
       }
   }
   ```

4. 修改 OrderController

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
   }
   ```

5. 测试

   访问 http://localhost:8000/consumer/payment/lb

