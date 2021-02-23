* [SpringCloud Config 分布式配置中心](#springcloud-config-%E5%88%86%E5%B8%83%E5%BC%8F%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83)
  * [一、概述](#%E4%B8%80%E6%A6%82%E8%BF%B0)
    * [1、分布式系统面临的问题](#1%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E9%9D%A2%E4%B8%B4%E7%9A%84%E9%97%AE%E9%A2%98)
    * [2、是什么](#2%E6%98%AF%E4%BB%80%E4%B9%88)
      * [如何使用](#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8)
    * [3、能干嘛](#3%E8%83%BD%E5%B9%B2%E5%98%9B)
    * [4、与 GitHub 整合配置](#4%E4%B8%8E-github-%E6%95%B4%E5%90%88%E9%85%8D%E7%BD%AE)
  * [二、Config 服务端配置与测试](#%E4%BA%8Cconfig-%E6%9C%8D%E5%8A%A1%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%B8%8E%E6%B5%8B%E8%AF%95)
    * [1、模拟环境](#1%E6%A8%A1%E6%8B%9F%E7%8E%AF%E5%A2%83)
      * [1）在 GitHub 或者 Gitee 上新建 springcloud\-config 仓库](#1%E5%9C%A8-github-%E6%88%96%E8%80%85-gitee-%E4%B8%8A%E6%96%B0%E5%BB%BA-springcloud-config-%E4%BB%93%E5%BA%93)
      * [2）拿到仓库地址](#2%E6%8B%BF%E5%88%B0%E4%BB%93%E5%BA%93%E5%9C%B0%E5%9D%80)
      * [3）克隆到本地（可选）](#3%E5%85%8B%E9%9A%86%E5%88%B0%E6%9C%AC%E5%9C%B0%E5%8F%AF%E9%80%89)
      * [4）本地代码](#4%E6%9C%AC%E5%9C%B0%E4%BB%A3%E7%A0%81)
    * [2、新建 cloud\-config\-center3344 模块](#2%E6%96%B0%E5%BB%BA-cloud-config-center3344-%E6%A8%A1%E5%9D%97)
      * [1）pom\.xml](#1pomxml)
      * [2）application\.yml](#2applicationyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
      * [4）windows下修改hosts文件，增加映射](#4windows%E4%B8%8B%E4%BF%AE%E6%94%B9hosts%E6%96%87%E4%BB%B6%E5%A2%9E%E5%8A%A0%E6%98%A0%E5%B0%84)
      * [5）测试通过 Config 微服务是否可以从 GitHub 上获取配置内容](#5%E6%B5%8B%E8%AF%95%E9%80%9A%E8%BF%87-config-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%98%AF%E5%90%A6%E5%8F%AF%E4%BB%A5%E4%BB%8E-github-%E4%B8%8A%E8%8E%B7%E5%8F%96%E9%85%8D%E7%BD%AE%E5%86%85%E5%AE%B9)
    * [3、配置的读取规则](#3%E9%85%8D%E7%BD%AE%E7%9A%84%E8%AF%BB%E5%8F%96%E8%A7%84%E5%88%99)
  * [三、Config 客户端配置与测试](#%E4%B8%89config-%E5%AE%A2%E6%88%B7%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%B8%8E%E6%B5%8B%E8%AF%95)
    * [1、新建 cloud\-config\-client3355](#1%E6%96%B0%E5%BB%BA-cloud-config-client3355)
      * [1）pom\.xml](#1pomxml-1)
      * [2）bootstrap\.yml](#2bootstrapyml)
      * [3）主启动类](#3%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB-1)
      * [4）控制类](#4%E6%8E%A7%E5%88%B6%E7%B1%BB)
      * [5）测试](#5%E6%B5%8B%E8%AF%95)
    * [2、问题随之而来，分布式配置的动态刷新问题](#2%E9%97%AE%E9%A2%98%E9%9A%8F%E4%B9%8B%E8%80%8C%E6%9D%A5%E5%88%86%E5%B8%83%E5%BC%8F%E9%85%8D%E7%BD%AE%E7%9A%84%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0%E9%97%AE%E9%A2%98)
  * [四、Config 客户端之动态刷新](#%E5%9B%9Bconfig-%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%B9%8B%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0)
    * [1、避免每次更新配置都要重启客户端微服务 3355](#1%E9%81%BF%E5%85%8D%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E9%85%8D%E7%BD%AE%E9%83%BD%E8%A6%81%E9%87%8D%E5%90%AF%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1-3355)
    * [2、动态刷新](#2%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0)
      * [1）修改 3355 模块](#1%E4%BF%AE%E6%94%B9-3355-%E6%A8%A1%E5%9D%97)
      * [2）测试](#2%E6%B5%8B%E8%AF%95)
        * [1、修改 git 仓库的文件](#1%E4%BF%AE%E6%94%B9-git-%E4%BB%93%E5%BA%93%E7%9A%84%E6%96%87%E4%BB%B6)
        * [2、观察](#2%E8%A7%82%E5%AF%9F)
        * [3、向 3355 发送刷新请求，必须 POST](#3%E5%90%91-3355-%E5%8F%91%E9%80%81%E5%88%B7%E6%96%B0%E8%AF%B7%E6%B1%82%E5%BF%85%E9%A1%BB-post)
        * [4、再次观察](#4%E5%86%8D%E6%AC%A1%E8%A7%82%E5%AF%9F)
    * [3、缺陷](#3%E7%BC%BA%E9%99%B7)

# SpringCloud Config 分布式配置中心

## 一、概述

### 1、分布式系统面临的问题

微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务。由于每个服务都需要必要的配置信息才能运行，所以一套集中式的、动态的配置管理设施是必不可少的。

Spring Cloud 提供了 ConfigServer 来解决这个问题。

### 2、是什么

![image-20210222143918907](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222143919.png)

SpringCloud Config 为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为**各个不同微服务应用**的所有环境提供了一个**中心化的外部配置**。

#### 如何使用

![image-20210222144525062](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222144525.png)

### 3、能干嘛

集中配置文件

不同环境不同配置，动态化的配置更新，分环境部署比如 dev/test/prod/beta/release

运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息

当配置发生变动时，服务不需要重启即可感知到配置的变化并应用新的配置

将配置信息以 REST 接口形式暴露，使用 post、curl 访问刷新均可

### 4、与 GitHub 整合配置

由于 SpringCloud Config 默认使用 Git 来存储配置文件（也有其他形式，比如支持 SVN 和本地文件），但最推荐的还是 Git，而且使用的是 http/https 访问的形式。

## 二、Config 服务端配置与测试

### 1、模拟环境

#### 1）在 GitHub 或者 Gitee 上新建 springcloud-config 仓库

![image-20210222161327516](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222161327.png)

#### 2）拿到仓库地址

https://github.com/devbzx/springcloud-config.git

#### 3）克隆到本地（可选）

git clone git@github.com:devbzx/springcloud-config.git

#### 4）本地代码

![image-20210222161520827](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222161520.png)

表示多个环境的配置文件

保存格式必须为UTF-8

如果需要修改，此处模拟运维人员操作 git 和 github

```shell
git add
git commit -m "init.yml"
git push origin master
```

### 2、新建 cloud-config-center3344 模块

#### 1）pom.xml

```xml
<dependencies>

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

#### 2）application.yml

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
          uri: https://github.com/devbzx/springcloud-config.git
          # 搜索目录
          search-paths:
            - springcloud-config
      # 读取分支
      label: main

# 服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    instance-id: cloud-config-server
    prefer-ip-address: true
```

#### 3）主启动类

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigCenterMain3344 {
    public static void main(String[] args){
        SpringApplication.run(ConfigCenterMain3344.class,args);
    }
}
```

#### 4）windows下修改hosts文件，增加映射

127.0.0.1  config-3344.com

#### 5）测试通过 Config 微服务是否可以从 GitHub 上获取配置内容

1、启动微服务3344                

2、发送请求：            

http://config-3344.com:3344/main/config-prod.yml

3、效果

![image-20210222163141016](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163141.png)

4、映射规则

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163449.png)

5、我们就将 ConfigServer config 服务中心和远程仓库之间打通了。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163513.png)

### 3、配置的读取规则

> /{label}/{application}-{profile}.yml

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163604.png)

> /{application}-{profile}.yml
>
> 默认去找master分支，所以这里不写分支
>
> 但 github 将新建仓库的主分支改为 main 分支后仍需要指定分支名称。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163631.png)

> /{application}/{profile}[/{label}]
>
> 逆写法，结果是 json 串

![image-20210222163852779](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222163852.png)

## 三、Config 客户端配置与测试

>上面我们的ConfigServer连接上了GitHub，成功的从GitHub上获取到了config信息。

![img](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222172329.png)

### 1、新建 cloud-config-client3355

#### 1）pom.xml

```xml
<dependencies>
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

#### 2）bootstrap.yml

> 为了配置文件的记载顺序和分级管理，我们这里用bootstrap.yml

application.yml使用户籍的资源配置项

bootstrap.yml是系统级的，优先级更高

SpringCloud会创建一个“Bootstrap Context”，作为Spring应用的`Application Context`的父上下文。

初始化的时候，`Bootstrap Context`负责从外部源加载配置属性并解析配置。

这两个上下文共享一个从外部获取的`Environment`

`Bootstrap`属性有高优先级，默认情况下，他们不会被本地配置覆盖。

`Bootstrap Context`和`Application Context`有着不同的约定，所以新增了一个`bootstrap.yml`文件，

保证`Bootstrap Context`和`Application Context`配置的分离。

要将Client模块下的application.yml文件改为bootstrap.yml，这是很关键的，因为bootstrap.yml

是比application.yml先加载的。bootstrap.yml优先级高于application.yml

![image-20210222173046593](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222173046.png)

```yml
server:
  port: 3355

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
#服务注册到 eureka
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
#    instance:
#      #instance-id: springcloud-config-client01
#      prefer-ip-address: true

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

> SpringCloud Config能将配置信息以REST接口的形式暴露。

```java
@RestController
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }

}
```

#### 5）测试

访问 http://localhost:3355/configInfo

![image-20210222173358521](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210222173358.png)

### 2、问题随之而来，分布式配置的动态刷新问题

1）Linux 运维修改 GitHub 上的配置文件内容做调整

2）刷新 3344，发现 ConfigServer 配置中心立刻响应

3）刷新 3355，发现 ConfigClient 客户端没有任何响应

4）3355 没有变化除非自己重启或者重新加载

5）每次修改配置文件，客户端都需要重启？？？

## 四、Config 客户端之动态刷新

### 1、避免每次更新配置都要重启客户端微服务 3355

### 2、动态刷新

#### 1）修改 3355 模块

引入 actuator 监控

```xml
 <!--监控-->
<dependency>
    <groupId>
        org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

改 yml，暴露监控端口，在 bootstrap.yml 里添加如下配置

```yml
# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

修改控制类，加上 `@RefreshScope` 注解

```java
@RestController
//增加 @RefreshScope 注解
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

#### 2）测试

##### 1、修改 git 仓库的文件

![image-20210223100427271](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223100427.png)

##### 2、观察 

3344

![image-20210223100557492](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223100557.png)

3355

![image-20210223100620201](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223100620.png)

没改变？？？

##### 3、向 3355 发送刷新请求，必须 POST

curl -X POST "http://localhost:3355/actuator/refresh"

![image-20210223101017063](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223101017.png)

##### 4、再次观察

![image-20210223101052582](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20210223101052.png)

成功！！！

### 3、缺陷

假如有多个微服务客户端 3355/3366/3377。。。

每个微服务都要执行一次 post请求，手动刷新？

可否广播，一次通知，处处生效？

如何大范围自动刷新？