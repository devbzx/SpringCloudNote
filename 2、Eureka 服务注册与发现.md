* [Eureka 服务注册与发现](#eureka-%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%8E%E5%8F%91%E7%8E%B0)
  * [一、Eureka 基础知识](#%E4%B8%80eureka-%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86)
    * [1、服务治理](#1%E6%9C%8D%E5%8A%A1%E6%B2%BB%E7%90%86)
    * [2、服务注册与发现](#2%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%8E%E5%8F%91%E7%8E%B0)
    * [3、Eureka 两组件](#3eureka-%E4%B8%A4%E7%BB%84%E4%BB%B6)
      * [1）Eureka Server](#1eureka-server)
      * [2）Eureka Client](#2eureka-client)
  * [二、单机 Eureka 构建步骤](#%E4%BA%8C%E5%8D%95%E6%9C%BA-eureka-%E6%9E%84%E5%BB%BA%E6%AD%A5%E9%AA%A4)
    * [1、cloud\-eureka\-server7001（服务端）](#1cloud-eureka-server7001%E6%9C%8D%E5%8A%A1%E7%AB%AF)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）测试](#4%E6%B5%8B%E8%AF%95)
    * [2、cloud\-provider\-payment8001（客户端）](#2cloud-provider-payment8001%E5%AE%A2%E6%88%B7%E7%AB%AF)
      * [1）pom\.xml](#1pomxml-1)
      * [2）application\.yml](#2applicationyml-1)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
      * [4）业务逻辑编写](#4%E4%B8%9A%E5%8A%A1%E9%80%BB%E8%BE%91%E7%BC%96%E5%86%99)
        * [mapper](#mapper)
        * [service](#service)
        * [controller](#controller)
      * [5）测试](#5%E6%B5%8B%E8%AF%95)
    * [3、cloud\-consumer\-order8000（客户端）](#3cloud-consumer-order8000%E5%AE%A2%E6%88%B7%E7%AB%AF)
      * [1）pom\.xml](#1pomxml-2)
      * [2）application\.yml](#2applicationyml-2)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-2)
      * [4）业务逻辑编写](#4%E4%B8%9A%E5%8A%A1%E9%80%BB%E8%BE%91%E7%BC%96%E5%86%99-1)
        * [controller](#controller-1)
        * [config](#config)
      * [5）测试](#5%E6%B5%8B%E8%AF%95-1)
  * [三、集群 Eureka 构建](#%E4%B8%89%E9%9B%86%E7%BE%A4-eureka-%E6%9E%84%E5%BB%BA)
    * [1、Eureka 集群说明](#1eureka-%E9%9B%86%E7%BE%A4%E8%AF%B4%E6%98%8E)
    * [2、Eureka Server 集群环境构建步骤](#2eureka-server-%E9%9B%86%E7%BE%A4%E7%8E%AF%E5%A2%83%E6%9E%84%E5%BB%BA%E6%AD%A5%E9%AA%A4)
      * [2\.1 cloud\-eureka\-server7002](#21-cloud-eureka-server7002)
        * [2\.1\.1 pom\.xml](#211-pomxml)
        * [2\.1\.2 修改 hosts 文件](#212-%E4%BF%AE%E6%94%B9-hosts-%E6%96%87%E4%BB%B6)
        * [2\.1\.3 application\.yml](#213-applicationyml)
        * [2\.1\.4 主启动类](#214-%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [2\.2 cloud\-eureka\-server7001](#22-cloud-eureka-server7001)
        * [2\.2\.1 application\.yml](#221-applicationyml)
    * [3、将支付服务 8001 发布到上面两台 Eureka 集群配置中](#3%E5%B0%86%E6%94%AF%E4%BB%98%E6%9C%8D%E5%8A%A1-8001-%E5%8F%91%E5%B8%83%E5%88%B0%E4%B8%8A%E9%9D%A2%E4%B8%A4%E5%8F%B0-eureka-%E9%9B%86%E7%BE%A4%E9%85%8D%E7%BD%AE%E4%B8%AD)
      * [3\.1 cloud\-provider\-payment8001](#31-cloud-provider-payment8001)
        * [3\.1\.1 controller](#311-controller)
        * [3\.1\.2 application\.yml](#312-applicationyml)
    * [4、将订单微服务 8000 发布到上面两台 Eureka 集群配置中](#4%E5%B0%86%E8%AE%A2%E5%8D%95%E5%BE%AE%E6%9C%8D%E5%8A%A1-8000-%E5%8F%91%E5%B8%83%E5%88%B0%E4%B8%8A%E9%9D%A2%E4%B8%A4%E5%8F%B0-eureka-%E9%9B%86%E7%BE%A4%E9%85%8D%E7%BD%AE%E4%B8%AD)
      * [4\.1 cloud\-consumer\-order8000](#41-cloud-consumer-order8000)
        * [4\.1\.1 application\.yml](#411-applicationyml)
    * [5、测试01](#5%E6%B5%8B%E8%AF%9501)
    * [6、支付服务提供者集群环境构建](#6%E6%94%AF%E4%BB%98%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85%E9%9B%86%E7%BE%A4%E7%8E%AF%E5%A2%83%E6%9E%84%E5%BB%BA)
      * [6\.1 cloud\-provider\-payment8002](#61-cloud-provider-payment8002)
        * [6\.1\.1 pom\.xml](#611-pomxml)
        * [6\.1\.2 application\.yml](#612-applicationyml)
        * [6\.1\.3 主启动类](#613-%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
        * [6\.1\.4 修改controller](#614-%E4%BF%AE%E6%94%B9controller)
          * [8001/8002 controller](#80018002-controller)
    * [7、负载均衡](#7%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)
      * [7\.1 cloud\-consumer\-order8000 存在的问题](#71-cloud-consumer-order8000-%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%98)
      * [7\.2 添加 @LoadBalanced 赋予 RestTemplate 负载均衡能力](#72-%E6%B7%BB%E5%8A%A0-loadbalanced-%E8%B5%8B%E4%BA%88-resttemplate-%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E8%83%BD%E5%8A%9B)
    * [8、测试02](#8%E6%B5%8B%E8%AF%9502)
  * [四、actuator 微服务信息完善](#%E5%9B%9Bactuator-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E4%BF%A1%E6%81%AF%E5%AE%8C%E5%96%84)
    * [1、主机名称：服务名称修改](#1%E4%B8%BB%E6%9C%BA%E5%90%8D%E7%A7%B0%E6%9C%8D%E5%8A%A1%E5%90%8D%E7%A7%B0%E4%BF%AE%E6%94%B9)
      * [修改 cloud\-provider\-payment8001 的 application\.yml](#%E4%BF%AE%E6%94%B9-cloud-provider-payment8001-%E7%9A%84-applicationyml)
    * [2、访问信息有 ip 提示](#2%E8%AE%BF%E9%97%AE%E4%BF%A1%E6%81%AF%E6%9C%89-ip-%E6%8F%90%E7%A4%BA)
      * [修改 cloud\-provider\-payment8001 的 application\.yml](#%E4%BF%AE%E6%94%B9-cloud-provider-payment8001-%E7%9A%84-applicationyml-1)
  * [五、服务发现](#%E4%BA%94%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
    * [1、修改 cloud\-provider\-payment8001 的 controller](#1%E4%BF%AE%E6%94%B9-cloud-provider-payment8001-%E7%9A%84-controller)
    * [2、修改 8001 的主启动类](#2%E4%BF%AE%E6%94%B9-8001-%E7%9A%84%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
    * [3、自测](#3%E8%87%AA%E6%B5%8B)
  * [六、Eureka 自我保护](#%E5%85%ADeureka-%E8%87%AA%E6%88%91%E4%BF%9D%E6%8A%A4)
    * [1、故障现象](#1%E6%95%85%E9%9A%9C%E7%8E%B0%E8%B1%A1)
    * [2、导致原因](#2%E5%AF%BC%E8%87%B4%E5%8E%9F%E5%9B%A0)
    * [3、禁止自我保护（一般生产环境中不会禁止自我保护）](#3%E7%A6%81%E6%AD%A2%E8%87%AA%E6%88%91%E4%BF%9D%E6%8A%A4%E4%B8%80%E8%88%AC%E7%94%9F%E4%BA%A7%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%B8%8D%E4%BC%9A%E7%A6%81%E6%AD%A2%E8%87%AA%E6%88%91%E4%BF%9D%E6%8A%A4)
      * [3\.1 cloud\-eureka\-server7001](#31-cloud-eureka-server7001)
      * [3\.2 cloud\-provider\-payment8001](#32-cloud-provider-payment8001)
    * [4、测试](#4%E6%B5%8B%E8%AF%95-1)

# Eureka 服务注册与发现

## 一、Eureka 基础知识

### 1、服务治理

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务治理。

在传统的 rpc 远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，所以需要使用服务治理，管理服务与服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

### 2、服务注册与发现

Eureka 采用了 CS 的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server 并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前服务器的信息：比如，服务地址、通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者）以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地 RPC 远程调用框架设计核心思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系（服务治理概念）。在任何 rpc 远程框架中，都会有一个注册中心（存放服务地址相关信息（接口地址））

<img src="C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201109170652970.png" alt="image-20201109170652970"  />

### 3、Eureka 两组件

**Eureka 包含两个组件：Eureka Server 和 Eureka Client**

#### 1）Eureka Server

 提供服务注册服务

各个微服务节点通过配置启动后，会在 Eureka Server 中进行注册，这样 Eureka Server 中的服务这侧标中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

#### 2）Eureka Client

通过注册中心进行访问

是一个 java 客户端，用于简化 Eureka Server 的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向 Eureka Server 发送心跳（默认周期为30秒）。如果 Eureka Server 在多个心跳周期内没有接收到某个节点的心跳，Eureka Server 将会从服务注册表中把这个服务节点移除（默认90秒）

## 二、单机 Eureka 构建步骤

### 1、cloud-eureka-server7001（服务端）

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

    <artifactId>cloud-eureka-server7001</artifactId>

    <dependencies>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
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

#### 2）application.yml

```yaml
server:
  port: 7001
eureka:
  instance:
    hostname: eureka7001.com  #eureka服务端的实例名字
  client:
    register-with-eureka: false    #表识不向注册中心注册自己
    fetch-registry: false   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      #集群指向其他Eureka
#      defaultZone: http://eureka7002.com:7002/eureka/  #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      #单机指向自己
      defaultZone: http://eureka7001.com:7001/eureka/
  server:
    enable-self-preservation: false
    eviction-interval-timer-in-ms: 2000
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class,args);
    }
}
```

#### 4）测试

访问 http://localhost:7001/

### 2、cloud-provider-payment8001（客户端）

服务提供者

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

    <artifactId>cloud-provider-payment8001</artifactId>
    <dependencies>


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


</project>
```

#### 2）application.yml

```yaml
server:
  port: 8001
spring:
  application:
    name: cloud-payment-service
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
  #instance:
    #instance-id: payment8001
    #prefer-ip-address: true #访问路径显示ip
    #Eureka 客户端向服务器发送心跳的时间间隔，单位是秒（默认30秒）
    #lease-renewal-interval-in-seconds: 1
    #Eureka 服务端在收到最后一次心跳后等待时间上限，单位为秒（默认90秒），超时将剔除服务
    #lease-expiration-duration-in-seconds: 2
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaClient
//@EnableDiscoveryClient
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class);
    }
}
```

#### 4）业务逻辑编写

##### mapper

```java
@Mapper
public interface PaymentDao {

    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") Long id);

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.lhq.cloud2020.dao.PaymentDao">

    <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial});
    </insert>

    <resultMap id="BaseResultMap" type="com.lhq.cloud2020.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT"></id>
        <id column="serial" property="serial" jdbcType="VARCHAR"></id>
    </resultMap>

    <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id = #{id}
    </select>

</mapper>
```

##### service

```java
public interface PaymentService {

    public int create(Payment payment);

    public Payment getPaymentById(Long id);

}
```

```java
@Service("paymentService")
public class PaymentServiceImpl implements PaymentService {
    @Resource
    private PaymentDao paymentDao;


    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getPaymentById(Long id) {
        return paymentDao.getPaymentById(id);
    }
}
```

##### controller

```java
@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @PostMapping("/payment/create")
    public CommonResult<Payment> create(@RequestBody Payment payment){
        log.info("绑定结果："+payment);
        int result = paymentService.create(payment);
        log.info("*****插入结果:"+result);

        if (result > 0){
            return new CommonResult(200,"插入数据库成功",result);
        } else {
            return new CommonResult(444,"插入数据库失败",null);
        }
    }
    @GetMapping("/payment/get/{id}")
    public CommonResult create(@PathVariable("id") Long id){
        Payment result = paymentService.getPaymentById(id);
        log.info("*****查找结果:"+result);
        if (!ObjectUtils.isEmpty(result)){
            return new CommonResult(200,"查找成功",result);
        } else {
            return new CommonResult(444,"查找失败，对应ID："+id,null);
        }
    }
}
```

#### 5）测试

先启动 cloud-eureka-server7001 -> 启动 cloud-provider-payment8001 -> 访问 http://localhost:7001/

### 3、cloud-consumer-order8000（客户端）

服务消费者

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

    <artifactId>cloud-consumer-order8000</artifactId>

    <dependencies>

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

</project>
```

#### 2）application.yml

```yaml
server:
  port: 8000
spring:
  application:
    name: cloud-order-service

eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka #单机版
      #defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class OrderMain8000 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain8000.class);
    }
}
```

#### 4）业务逻辑编写

##### controller

```java
@RestController
public class OrderController {

    public static final String PROVIDER_URL = "http://localhost:8001";
    //public static final String PROVIDER_URL = "http://CLOUD-PAYMENT-SERVICE/";

    @Resource
    RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment){
        return restTemplate.postForObject(PROVIDER_URL+"payment/create",payment,CommonResult.class);
    }

    @GetMapping("/consumer/payment/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PROVIDER_URL+"payment/get/"+id,CommonResult.class);
    }
}
```

##### config

```java
@Configuration
public class ApplicationContextConfig {
    @Bean
    //@LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

#### 5）测试

先启动 EurekaServer7001 -> 启动服务提供者 8001 -> 启动消费者 8000

访问消费者 controller 的 http://localhost:8000/consumer/payment/get/1

## 三、集群 Eureka 构建

### 1、Eureka 集群说明

![image-20201110094031555](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201110094031555.png)

### 2、Eureka Server 集群环境构建步骤

#### 2.1 cloud-eureka-server7002

参考 cloud-eureka-server7001 构建 

##### 2.1.1 pom.xml

与 cloud-eureka-server7001 一致

##### 2.1.2 修改 hosts 文件

‪C:\Windows\System32\drivers\etc\hosts

将映射配置添加进去

```hosts
127.0.0.1  eureka7001.com
127.0.0.1  eureka7002.com
```

##### 2.1.3 application.yml

```yaml
server:
  port: 7002
eureka:
  instance:
    hostname: eureka7002.com  #eureka服务端的实例名字
  client:
    register-with-eureka: false    #表识不向注册中心注册自己
    fetch-registry: false   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/  #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
```

##### 2.1.4 主启动类

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7002 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7002.class,args);
    }
}
```

#### 2.2 cloud-eureka-server7001

##### 2.2.1 application.yml

```yaml
server:
  port: 7001
eureka:
  instance:
    hostname: eureka7001.com  #eureka服务端的实例名字
  client:
    register-with-eureka: false    #表示不向注册中心注册自己
    fetch-registry: false   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      #集群指向其他Eureka
      defaultZone: http://eureka7002.com:7002/eureka/  #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      #单机指向自己
      #defaultZone: http://eureka7001.com:7001/eureka/
  server:
    enable-self-preservation: false
    eviction-interval-timer-in-ms: 2000
```

### 3、将支付服务 8001 发布到上面两台 Eureka 集群配置中

#### 3.1 cloud-provider-payment8001

##### 3.1.1 controller

```java
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
    public CommonResult create(@PathVariable("id") Long id){
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
}
```

##### 3.1.2 application.yml

```yaml
server:
  port: 8001
  
spring:
  application:
    name: cloud-payment-service
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
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      #defaultZone: http://localhost:7001/eureka  #单机版
      
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

### 4、将订单微服务 8000 发布到上面两台 Eureka 集群配置中

#### 4.1 cloud-consumer-order8000

##### 4.1.1 application.yml

```yaml
server:
  port: 8000
spring:
  application:
    name: cloud-order-service

eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
#      defaultZone: http://localhost:7001/eureka #单机版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
```

### 5、测试01

启动 Eureka Server，7001/7002服务

启动 provider 8001服务

启动消费者，8000 服务

访问 http://localhost:8000/consumer/payment/1

### 6、支付服务提供者集群环境构建

#### 6.1 cloud-provider-payment8002

参考 cloud-provider-payment8002 构建

##### 6.1.1 pom.xml

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

    <artifactId>cloud-provider-payment8002</artifactId>
    <dependencies>


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

</project>
```

##### 6.1.2 application.yml

```yaml
server:
  port: 8002

spring:
  application:
    name: cloud-payment-service
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
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
#      defaultZone: http://localhost:7001/eureka #单机版
  instance:
    instance-id: payment8002
    prefer-ip-address: true
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

##### 6.1.3 主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class PaymentMain8002 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8002.class);
    }
}
```

##### 6.1.4 修改controller

###### 8001/8002 controller

```java
@RestController
@Slf4j
public class PaymentController {
    @Resource
    private PaymentService paymentService;

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
    public CommonResult create(@PathVariable("id") Long id){
        Payment result = paymentService.getPaymentById(id);
        log.info("*****查找结果:"+result);
        if (!ObjectUtils.isEmpty(result)){
            return new CommonResult(200,"查找成功,serverPort:"+serverPort,result);
        } else {
            return new CommonResult(444,"查找失败，对应ID："+id,null);
        }
    }
}
```

### 7、负载均衡

#### 7.1 cloud-consumer-order8000 存在的问题

订单服务访问地址不能写死在controller中

```java
	//public static final String PROVIDER_URL = "http://localhost:8001";
//使用这个
    public static final String PROVIDER_URL = "http://CLOUD-PAYMENT-SERVICE/";
```

#### 7.2 添加 @LoadBalanced 赋予 RestTemplate 负载均衡能力

cloud-consumer-order8000 的 config.ApplicationContextConfig

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

### 8、测试02

启动 Eureka Server，7001/7002

启动服务提供者 provider，8001/8002 服务

访问 http://localhost/consumer/payment/get/1

出现 8001/8002 交替执行

## 四、actuator 微服务信息完善

### 1、主机名称：服务名称修改

#### 修改 cloud-provider-payment8001 的 application.yml

修改部分

```yaml
instance:
    instance-id: payment8001
```

完整内容

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
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
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  #集群版
      #defaultZone: http://localhost:7001/eureka  #单机版
  instance:
    instance-id: payment8001
    
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

### 2、访问信息有 ip 提示

#### 修改 cloud-provider-payment8001 的 application.yml

修改部分

```yaml
prefer-ip-address: true #访问路径显示ip
```

完整内容

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
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

## 五、服务发现

对于注册进 eureka 的微服务，可以通过服务发现来获得该服务的信息

### 1、修改 cloud-provider-payment8001 的 controller

添加

```java
@Autowired
private DiscoveryClient discoveryClient;
```

```java
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
```

### 2、修改 8001 的主启动类

添加 @EnableDiscoveryClient 注解

### 3、自测

先启动 Eureka Server 7001/7002

再启动 8001 主启动类

访问 http://localhost:8001/payment/discovery

## 六、Eureka 自我保护

### 1、故障现象

![image-20201110103943603](C:\Users\13014\AppData\Roaming\Typora\typora-user-images\image-20201110103943603.png)

### 2、导致原因

某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存。

属于CAP里面的AP分支。

### 3、禁止自我保护（一般生产环境中不会禁止自我保护）

#### 3.1 cloud-eureka-server7001

出场默认自我保护机制是开启的

在 yml 文件里配置禁用自我保护模式

```yaml
eureka:
    server:
      enable-self-preservation: false
      eviction-interval-timer-in-ms: 2000
```

#### 3.2 cloud-provider-payment8001

配置 yml 

```yaml
eureka:
	client:
        #Eureka 客户端向服务器发送心跳的时间间隔，单位是秒（默认30秒）
        lease-renewal-interval-in-seconds: 1
        #Eureka 服务端在收到最后一次心跳后等待时间上限，单位为秒（默认90秒），超时将剔除服务
        lease-expiration-duration-in-seconds: 2
```

### 4、测试

启动 7001 再启动 8001

关闭 8001

马上被删除