# OpenFeign服务接口调用

## 一、概述

### 1、OpenFeign 是什么

**简介**

Feign是一个声明式的web服务客户端，让编写web服务客户端变得非常容易，只需创建一个接口并在接口上添加注解即可

它的使用方法是定义一个服务接口然后在上面添加注解。Feign 也支持可插拔式的编码器和解码器。Spring Cloud 对 Feign 进行了封装，使其支持了Spring MVC标准注解和 HttpMessageConverters。Feign 可以与 Eureka和Ribbon 组合使用以支持负载均衡。

**GitHub**

https://github.com/spring-cloud/spring-cloud-openfeign

### 2、能干嘛

Feign 旨在让编写 Java http 客户端变得更容易。

Feign 在使用 Ribbon + RestTemplate 的基础上做了进一步封装，由它来帮助我们定义和实现依赖服务接口的定义。在 Feign 的实现下，我们只需创建一个接口并使用注解的方式来配置它，即可完成对服务提供方的接口绑定。

Feign 集成了 Ribbon

与 Ribbon 不同的是，通过 Feign 只需定义服务绑定接口且以声明式的方法实现了服务调用。

#### Feign 和 OpenFeign 的区别

Feign是Springcloud组件中的一个轻量级Restful的HTTP服务客户端，Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务。Feign的使用方式是：使用Feign的注解定义接口，调用这个接口，就可以调用服务注册中心的服务

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-feign</artifactId>
</dependency>
```

OpenFeign是springcloud在Feign的基础上支持了SpringMVC的注解，如@RequestMapping等等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

## 二、OpenFeign 使用步骤

### 1、新建cloud-consumer-feign-order8000

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

    <artifactId>cloud-consumer-feign-order8000</artifactId>

    <dependencies>
        <!--openfeign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>com.lhq.cloud2020</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
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

### 3、application.yml

```yaml
server:
  port: 8000
eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka, http://eureka7002.com:7002/eureka
```

### 4、主启动类

```java
package com.lhq.cloud2020;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-17 16:56
 */
@SpringBootApplication
@EnableFeignClients
public class OrderFeignMain8000 {
    public static void main(String[] args){
        SpringApplication.run(OrderFeignMain8000.class,args);
    }
}

```

### 5、业务类

```java
package com.lhq.cloud2020.service;

import com.lhq.cloud2020.entities.CommonResult;
import com.lhq.cloud2020.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.util.ObjectUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-17 16:57
 */
@Component
@FeignClient("CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {

    @GetMapping("/payment/get/{id}")
    public CommonResult get(@PathVariable("id") Long id);

    @GetMapping(value = "/payment/feign/timeout")
    public String paymentFeignTimeout();

}

```

### 6、控制类

```java
package com.lhq.cloud2020.controller;

import com.lhq.cloud2020.entities.CommonResult;
import com.lhq.cloud2020.service.PaymentFeignService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-17 17:04
 */
@RestController
@Slf4j
public class OrderFeignController {

    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult get(@PathVariable("id") Long id){
        return paymentFeignService.get(id);
    }

    @GetMapping(value = "/consumer/payment/feign/timeout")
    public String paymentFeignTimeout(){
        //openFeign-ribbon，客户端一般默认等待1秒
        return paymentFeignService.paymentFeignTimeout();
    }

}

```

7、测试

启动 eureka 7001/7002

启动 微服务 8001/8002

启动 OrderFeignMain8000

访问 http:localhost:8000/consumer/payment/get/{id}

## 三、OpenFeign 超时控制

### 1、OpenFeign 超时控制是什么

默认 Feign 客户端只等待 1 秒，但是服务端处理需要超过 1 秒，导致 Feign 直接报错。

为了避免这种情况，我们需要设置 Feign 客户端的超时控制。

### 2、错误示例

有时我们写需要远程调用读写量大的服务，比如爬虫

我们的调用需要执行 addBatch() 方法，造成很大的延时

```java
package com.lhq.provider.service.impl;

import com.lhq.api.pojo.Movie;
import com.lhq.api.util.JSONResult;
import com.lhq.provider.dao.MovieDao;
import com.lhq.provider.service.MovieService;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import org.springframework.stereotype.Service;
import org.springframework.util.ObjectUtils;

import javax.annotation.Resource;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.UUID;
/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-12-08 08:45
 */
@Service("movieService")
public class MovieServiceImpl implements MovieService {

    @Resource
    MovieDao movieDao;
    @Override
    public JSONResult addBatch() {
        String url="";
        List<Movie> list=new ArrayList<Movie>();
        long start =new Date().getTime();
        for(int i=1;i<5;i++) {
            url = "https://www.dytt8.net/html/gndy/dyzz/list_23_" + i + ".html";
            try {
                Document doc = Jsoup.connect(url).userAgent("Mozilla/5.0 (Windows NT 6.1; rv:30.0) Gecko/20100101 Firefox/30.0").get();
                Elements elements = doc.select(" tbody > tr:nth-child(2) > td:nth-child(2) > b > a");
                for (Element e : elements) {
                    Movie movie = new Movie();
                    movie.setMName(e.text());
                    movie.setMUrl("https://www.dytt8.net" + e.attr("href"));
                    movie.setId(UUID.randomUUID().toString());
                    Document doc1 = Jsoup.connect("https://www.dytt8.net" + e.attr("href")).userAgent("Mozilla/5.0 (Windows NT 6.1; rv:30.0) Gecko/20100101 Firefox/30.0").get();
                    Elements elements1 = doc1.select("span > table > tbody > tr > td > a");
                    for (Element e1 : elements1) {
                        movie.setMFtp(e1.attr("href"));
                    }
                    Elements elements2 = doc1.select("p:nth-child(1) > img");
                    for (Element e1 : elements2) {
                        movie.setMPic(e1.attr("src"));
                    }
                    list.add(movie);
                }
            } catch (IOException e) {
                e.printStackTrace();
                return JSONResult.errorMsg("爬取失败");
            }
        }
        if (!ObjectUtils.isEmpty(list)){
            Long end = new Date().getTime();
            movieDao.insertBatch(list);
            return JSONResult.ok("添加成功，耗时：" + ((end - start)/1000) +"s");
        }
        return JSONResult.errorMsg("数据为空");
    }

    @Override
    public JSONResult selectAll() {
        List<Movie> movies = movieDao.selectAll();
        return JSONResult.ok(movies);
    }

    @Override
    public JSONResult selectByPage(Integer currentPage, Integer pageSize) {
        currentPage = pageSize * (currentPage - 1);
        List<Movie> movies = movieDao.selectByPage(currentPage,pageSize);
        for (Movie movie : movies) {
            String[] split = movie.getMName().split("《");
            String[] split1 = split[1].split("》");
            movie.setMName(split1[0]);
        }
        return JSONResult.ok(movies);
    }

    @Override
    public JSONResult getById(String id) {
        Movie movie = movieDao.selectByPrimaryKey(id);
        movie.setHits(movie.getHits()+1);
        movieDao.updateByPrimaryKeySelective(movie);
        return JSONResult.ok(movie);
    }
    
}

```

OpenFeign 只等待1秒，超过后会报错

### 3、解决方法

我们需要在 yml 里配置

```yaml
ribbon:
  ReadTimeout: 5000
  ConnectTimeout: 5000
```

## 四、OpenFeign 日志打印功能

### 1、日志功能

Feign 提供了日志打印功能，可以通过配置来调整日志级别，从而了解 Feign 中 Http 请求的细节。
说白了就是对接口的调用情况进行监控和输出

### 2、日志级别

- NONE：默认的，不显示任何日志
- BASIC：仅记录请求方法、URL、响应状态码及执行时间
- HEADERS：除了 BASIC 中定义的信息之外，还有请求和响应的头信息
- FULL：除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据

### 3、实现

#### 1）添加 config

```java
package com.lhq.cloud2020.config;

/**
 * TODO
 *
 * @author 李浩琦
 * @since 2020-11-17 17:32
 */

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {

    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }

}
```

#### 2）yml 配置开启日志的 Feign 客户端

```yaml
logging:
  level:
    com.lhq.cloud2020.service.PaymentFeignService: debug
```

