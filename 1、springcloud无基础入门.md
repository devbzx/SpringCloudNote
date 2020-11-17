* [Spring Cloud无基础入门](#spring-cloud%E6%97%A0%E5%9F%BA%E7%A1%80%E5%85%A5%E9%97%A8)
  * [1、技术选型](#1%E6%8A%80%E6%9C%AF%E9%80%89%E5%9E%8B)
    * [1）Spring Boot 的版本](#1spring-boot-%E7%9A%84%E7%89%88%E6%9C%AC)
      * [git 源码地址](#git-%E6%BA%90%E7%A0%81%E5%9C%B0%E5%9D%80)
      * [新特性介绍地址](#%E6%96%B0%E7%89%B9%E6%80%A7%E4%BB%8B%E7%BB%8D%E5%9C%B0%E5%9D%80)
    * [2）Spring Cloud 的版本](#2spring-cloud-%E7%9A%84%E7%89%88%E6%9C%AC)
      * [git 源码地址](#git-%E6%BA%90%E7%A0%81%E5%9C%B0%E5%9D%80-1)
      * [官网地址](#%E5%AE%98%E7%BD%91%E5%9C%B0%E5%9D%80)
      * [文档地址](#%E6%96%87%E6%A1%A3%E5%9C%B0%E5%9D%80)
    * [3）Spring Boot 和 Spring Cloud 的版本对应关系](#3spring-boot-%E5%92%8C-spring-cloud-%E7%9A%84%E7%89%88%E6%9C%AC%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB)
    * [4）组件的使用](#4%E7%BB%84%E4%BB%B6%E7%9A%84%E4%BD%BF%E7%94%A8)
  * [2、微服务架构编码构建](#2%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E7%BC%96%E7%A0%81%E6%9E%84%E5%BB%BA)
    * [1）父工程](#1%E7%88%B6%E5%B7%A5%E7%A8%8B)
      * [1、新建 maven 工程](#1%E6%96%B0%E5%BB%BA-maven-%E5%B7%A5%E7%A8%8B)
      * [2、pom](#2pom)
    * [2）Rest 微服务工程构建](#2rest-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%B7%A5%E7%A8%8B%E6%9E%84%E5%BB%BA)
      * [1、cloud\-api\-commons](#1cloud-api-commons)
        * [1\.1 改 pom 文件](#11-%E6%94%B9-pom-%E6%96%87%E4%BB%B6)
        * [1\.2 entities](#12-entities)
          * [1\.2\.1 Payment](#121-payment)
          * [1\.2\.2 CommonResult](#122-commonresult)
        * [1\.3 打包](#13-%E6%89%93%E5%8C%85)
      * [2、cloud\-provider\-payment8001](#2cloud-provider-payment8001)
        * [2\.1 改 pom 文件](#21-%E6%94%B9-pom-%E6%96%87%E4%BB%B6)
        * [2\.2 写 YML](#22-%E5%86%99-yml)
        * [2\.3 主启动类](#23-%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
        * [2\.4 业务类](#24-%E4%B8%9A%E5%8A%A1%E7%B1%BB)
          * [2\.4\.1 建表 sql](#241-%E5%BB%BA%E8%A1%A8-sql)
          * [2\.4\.2 dao](#242-dao)
          * [2\.4\.3 service](#243-service)
          * [2\.4\.4 controller](#244-controller)
      * [3、cloud\-consumer\-order8000](#3cloud-consumer-order8000)
        * [3\.1 改 pom 文件](#31-%E6%94%B9-pom-%E6%96%87%E4%BB%B6)
        * [3\.2 写 YML](#32-%E5%86%99-yml)
        * [3\.3 主启动类](#33-%E4%B8%BB%E5%90%AF%E5%8A%A8%E7%B1%BB)
        * [3\.4 业务类](#34-%E4%B8%9A%E5%8A%A1%E7%B1%BB)
          * [3\.4\.1 config 配置类](#341-config-%E9%85%8D%E7%BD%AE%E7%B1%BB)
          * [3\.4\.2 controller](#342-controller)
  * [3、工程样图](#3%E5%B7%A5%E7%A8%8B%E6%A0%B7%E5%9B%BE)

# Spring Cloud无基础入门

## 1、技术选型

### 1）Spring Boot 的版本

#### git 源码地址

https://github.com/spring-projects/spring-boot/releases/

#### 新特性介绍地址

https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Release-Notes

### 2）Spring Cloud 的版本

#### git 源码地址

https://github.com/spring-projects/spring-cloud/wiki

#### 官网地址

https://spring.io/projects/spring-cloud

#### 文档地址

https://spring.io/projects/spring-cloud#overview

### 3）Spring Boot 和 Spring Cloud 的版本对应关系

https://start.spring.io/actuator/info

```json
{
  "git": {
    "branch": "82af3869647d62a1e520a076908c14eee4715d8d",
    "commit": {
      "id": "82af386",
      "time": "2020-11-02T15:56:02Z"
    }
  },
  "build": {
    "version": "0.0.1-SNAPSHOT",
    "artifact": "start-site",
    "versions": {
      "spring-boot": "2.3.5.RELEASE",
      "initializr": "0.10.0-SNAPSHOT"
    },
    "name": "start.spring.io website",
    "time": "2020-11-02T16:15:14.702Z",
    "group": "io.spring.start"
  },
  "bom-ranges": {
    "azure": {
      "2.0.10": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
      "2.1.10": "Spring Boot >=2.1.0.RELEASE and <2.2.0.M1",
      "2.2.4": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
      "2.3.5": "Spring Boot >=2.3.0.M1"
    },
    "codecentric-spring-boot-admin": {
      "2.0.6": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
      "2.1.6": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
      "2.2.4": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
      "2.3.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1"
    },
    "solace-spring-boot": {
      "1.0.0": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1",
      "1.1.0": "Spring Boot >=2.3.0.M1"
    },
    "solace-spring-cloud": {
      "1.0.0": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1",
      "1.1.1": "Spring Boot >=2.3.0.M1"
    },
    "spring-cloud": {
      "Finchley.M2": "Spring Boot >=2.0.0.M3 and <2.0.0.M5",
      "Finchley.M3": "Spring Boot >=2.0.0.M5 and <=2.0.0.M5",
      "Finchley.M4": "Spring Boot >=2.0.0.M6 and <=2.0.0.M6",
      "Finchley.M5": "Spring Boot >=2.0.0.M7 and <=2.0.0.M7",
      "Finchley.M6": "Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
      "Finchley.M7": "Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
      "Finchley.M9": "Spring Boot >=2.0.0.RELEASE and <=2.0.0.RELEASE",
      "Finchley.RC1": "Spring Boot >=2.0.1.RELEASE and <2.0.2.RELEASE",
      "Finchley.RC2": "Spring Boot >=2.0.2.RELEASE and <2.0.3.RELEASE",
      "Finchley.SR4": "Spring Boot >=2.0.3.RELEASE and <2.0.999.BUILD-SNAPSHOT",
      "Finchley.BUILD-SNAPSHOT": "Spring Boot >=2.0.999.BUILD-SNAPSHOT and <2.1.0.M3",
      "Greenwich.M1": "Spring Boot >=2.1.0.M3 and <2.1.0.RELEASE",
      "Greenwich.SR6": "Spring Boot >=2.1.0.RELEASE and <2.1.999.BUILD-SNAPSHOT",
      "Greenwich.BUILD-SNAPSHOT": "Spring Boot >=2.1.999.BUILD-SNAPSHOT and <2.2.0.M4",
      "Hoxton.SR8": "Spring Boot >=2.2.0.M4 and <2.3.6.BUILD-SNAPSHOT",
      "Hoxton.BUILD-SNAPSHOT": "Spring Boot >=2.3.6.BUILD-SNAPSHOT and <2.4.0.M1",
      "2020.0.0-M3": "Spring Boot >=2.4.0.M1 and <=2.4.0.M1",
      "2020.0.0-M4": "Spring Boot >=2.4.0.M2 and <=2.4.0-M3",
      "2020.0.0-SNAPSHOT": "Spring Boot >=2.4.0-M4"
    },
    "spring-cloud-alibaba": {
      "2.2.1.RELEASE": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
    },
    "spring-cloud-services": {
      "2.0.3.RELEASE": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
      "2.1.8.RELEASE": "Spring Boot >=2.1.0.RELEASE and <2.2.0.RELEASE",
      "2.2.6.RELEASE": "Spring Boot >=2.2.0.RELEASE and <2.3.0.RELEASE",
      "2.3.0.RELEASE": "Spring Boot >=2.3.0.RELEASE and <2.4.0-M1"
    },
    "spring-statemachine": {
      "2.0.0.M4": "Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
      "2.0.0.M5": "Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
      "2.0.1.RELEASE": "Spring Boot >=2.0.0.RELEASE"
    },
    "vaadin": {
      "10.0.17": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
      "14.4.2": "Spring Boot >=2.1.0.M1 and <2.4.0-M1"
    },
    "wavefront": {
      "2.0.2": "Spring Boot >=2.1.0.RELEASE and <2.4.0-M1",
      "2.1.0-SNAPSHOT": "Spring Boot >=2.4.0-M1"
    }
  },
  "dependency-ranges": {
    "okta": {
      "1.2.1": "Spring Boot >=2.1.2.RELEASE and <2.2.0.M1",
      "1.4.0": "Spring Boot >=2.2.0.M1 and <2.4.0-M1"
    },
    "mybatis": {
      "2.0.1": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
      "2.1.3": "Spring Boot >=2.1.0.RELEASE and <2.4.0-M1"
    },
    "geode": {
      "1.2.10.RELEASE": "Spring Boot >=2.2.0.M5 and <2.3.0.M1",
      "1.3.4.RELEASE": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
      "1.4.0-M4": "Spring Boot >=2.4.0-M1"
    },
    "camel": {
      "2.22.4": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
      "2.25.2": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
      "3.3.0": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
      "3.5.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1"
    },
    "open-service-broker": {
      "2.1.3.RELEASE": "Spring Boot >=2.0.0.RELEASE and <2.1.0.M1",
      "3.0.4.RELEASE": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
      "3.1.1.RELEASE": "Spring Boot >=2.2.0.M1 and <2.4.0-M1"
    }
  }
}
```

### 4）组件的使用

![image-20201104101726632](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201116191202.png)

## 2、微服务架构编码构建

**约定 > 配置 > 编码**

### 1）父工程

#### 1、新建 maven 工程

#### 2、pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.lhq.cloud2020</groupId>
  <artifactId>springcloud</artifactId>
  <version>1.0-SNAPSHOT</version>
  <modules>
    <module>cloud-provider-payment8001</module>
    <module>cloud-api-commons</module>
    <module>cloud-consumer-order8000</module>
  </modules>
  <packaging>pom</packaging>
  <!-- 统一管理jar包版本 -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>8.0.18</mysql.version>
    <druid.version>1.1.10</druid.version>
  <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
  </properties>
  <!-- 子模块继承之后，提供作用：锁定版本+子modlue不用写groupId和version  -->
  <dependencyManagement>
    <dependencies>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud Hoxton.SR1-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>

      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
        <optional>true</optional>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

**父工程创建完成执行 mvn : install 将父工程发布到仓库方便子工程继承**

### 2）Rest 微服务工程构建

#### 1、cloud-api-commons

##### 1.1 改 pom 文件

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

    <artifactId>cloud-api-commons</artifactId>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
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

        <!-- https://mvnrepository.com/artifact/cn.hutool/hutool-all -->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.0</version>
        </dependency>
    </dependencies>
</project>
```

##### 1.2 entities

###### 1.2.1 Payment

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment {
    private Long id;
    private String serial;
}
```

###### 1.2.2 CommonResult

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T> {
    private Integer code;
    private String message;
    private  T data;
    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}
```

##### 1.3 打包

使用 maven 的 clean 和 install

其他模块即可通过 pom 引入

```xml
<dependency>
    <groupId>com.lhq.cloud2020</groupId>
    <artifactId>cloud-api-commons</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

#### 2、cloud-provider-payment8001 

**微服务提供支付 module 模块**

##### 2.1 改 pom 文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
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

##### 2.2 写 YML

```yml
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
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.lhq.cloud2020.entities
```

##### 2.3 主启动类

```java
@SpringBootApplication
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class);
    }
}
```

##### 2.4 业务类

###### 2.4.1 建表 sql 

```mysql
DROP TABLE IF EXISTS `payment`;
CREATE TABLE `payment` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `serial` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;
```

###### 2.4.2 dao

**接口**

```java
@Mapper
public interface PaymentDao {
    
    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") Long id);

}
```

**Mapper**

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

###### 2.4.3 service

**PaymentService**

```java
public interface PaymentService {
    
    public int create(Payment payment);

    public Payment getPaymentById(Long id);
    
}
```

**PaymentServiceImpl**

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

###### 2.4.4 controller

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
            return new CommonResult(200,"插入成功",result);
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

#### 3、cloud-consumer-order8000

##### 3.1 改 pom 文件

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

##### 3.2 写 YML

```yaml
server:
  port: 8000
```

##### 3.3 主启动类

```java
@SpringBootApplication
public class OrderMain8000 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain8000.class);
    }
}
```

##### 3.4 业务类

###### 3.4.1 config 配置类

```java
@Configuration
public class ApplicationContextConfig {

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

###### 3.4.2 controller

```java
@RestController
public class OrderController {

    public static final String PROVIDER_URL = "http://localhost:8001/";

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

## 3、工程样图

![image-20201104105317362](https://gitee.com/li_hao_qi/imgbed/raw/master/img/20201116191222.png)



