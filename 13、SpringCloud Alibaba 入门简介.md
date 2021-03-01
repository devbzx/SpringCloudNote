* [SpringCloud Alibaba 入门简介](#springcloud-alibaba-%E5%85%A5%E9%97%A8%E7%AE%80%E4%BB%8B)
  * [一、为什么会出现 SpringCloud Alibaba](#%E4%B8%80%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BC%9A%E5%87%BA%E7%8E%B0-springcloud-alibaba)
  * [二、SpringCloud Alibaba 带来了什么](#%E4%BA%8Cspringcloud-alibaba-%E5%B8%A6%E6%9D%A5%E4%BA%86%E4%BB%80%E4%B9%88)
    * [1、是什么](#1%E6%98%AF%E4%BB%80%E4%B9%88)
    * [2、能干嘛](#2%E8%83%BD%E5%B9%B2%E5%98%9B)
    * [3、官方文档](#3%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3)
    * [4、怎么玩](#4%E6%80%8E%E4%B9%88%E7%8E%A9)

# SpringCloud Alibaba 入门简介

## 一、为什么会出现 SpringCloud Alibaba

Spring Cloud Netflix 项目进入维护模式

Spring Cloud 团队不会再向模块添加新功能。

将修复 block 级别的 bug 以及安全问题，也会考虑并审查社区的小型 pull request。

## 二、SpringCloud Alibaba 带来了什么

### 1、是什么

Spring Cloud for Alibaba，它是由一些阿里巴巴的开源组件和云产品组成的。这个项目的目的是为了让大家所熟知的 Spring 框架，其优秀的设计模式和抽象理念，以给使用阿里巴巴产品的 java 开发者带来使用 Spring Boot 和 Spring Cloud 的更多便利。

### 2、能干嘛

服务限流降级：默认支持 Servlet、Feign、RestTemplate、Dubbo 和 RocketMQ 限流降级功能的接入，可以在运行时通过控制台实时修改限流降级规则，还支持查看限流降级 Metrics 监控。

服务注册与发现：适配 Spring Cloud 服务注册与发现标准，默认继承了 Ribbon 的支持。

分布式配置管理：支持分布式系统中的外部化配置，配置更改时自动刷新。

消息驱动能力：基于 Spring Cloud Stream 为微服务应用构建消息驱动能力。

阿里云对象存储：阿里云提供的海量、安全、低成本、高可靠的云存储服务。支持在任何应用、任何时间、任何地点存储和访问任意类型的数据。

分布式任务调度：提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。同时提供分布式的任务执行模型，如网格任务。网格任务支持海量子任务均匀分配到所有 Worker（schedulerx-client）上执行。

### 3、官方文档

https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md

### 4、怎么玩

**[Sentinel](https://github.com/alibaba/Sentinel)**：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

**[Nacos](https://github.com/alibaba/Nacos)**：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。

**[RocketMQ](https://rocketmq.apache.org/)**：一款开源的分布式消息系统，基于高可用分布式集群技术，提供低延时的、高可靠的消息发布与订阅服务。

**[Dubbo](https://github.com/apache/dubbo)**：Apache Dubbo™ 是一款高性能 Java RPC 框架。

**[Seata](https://github.com/seata/seata)**：阿里巴巴开源产品，一个易于使用的高性能微服务分布式事务解决方案。

**[Alibaba Cloud OSS](https://www.aliyun.com/product/oss)**: 阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。

**[Alibaba Cloud SchedulerX](https://help.aliyun.com/document_detail/43136.html)**: 阿里中间件团队开发的一款分布式任务调度产品，提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。

**[Alibaba Cloud SMS](https://www.aliyun.com/product/sms)**: 覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。

