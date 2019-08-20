

# spring cloud

Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装成为Spring Cloud，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。微服务是可以独立部署、水平扩展、独立访问（或者有独立的数据库）的服务单元，spring cloud就是这些微服务的大管家



## 微服务

All In One 可以看成是eclipse中只有一个大的工程，Tmall: com.isea.service(一个包): 商品/交易/积分/订单… 然后达成一个war包部署在Tomcat中（或者是多个Tomcat）所有的项目耦合在一起，如果一个模块存在bug，将会对其他的模块产生影响，而微服务现在将（其按照业务）拆分开来，形成独立的模块。

马丁福乐说：

> a definition of this new architectural term there is no precise definition of this architectural style

但是微服务具有下面的特点：

* 微服务是一种架构模式或者说是一种架构风格，它**提倡将单一应用程序划分成一组小的服务**
* 每个服务运行在自己**独立的进程**中；
* 服务之间使用简单的**RESTful** API通信；
* 独立编码，独立部署、发布，可以使用不同的语言来编写服务，并使用不同的数据存储。

Dubbo的通信机制是基于远程过程调用（RPC），而微服务是基于HTTP的RESTful API；一个是品牌机，一个是组装机。Dubbo的定位始终是一款RPC框架，而Spring cloud是微服务架构下的一站式解决方案。

**微服务的优点是：**

* 每个服务足够内聚，小，代码独立，聚焦于业务；
* 每个微服务可以有小团队完成，开发效率高；
* 服务解耦；可以使用不同的语言开发并使用不同的数存储；
* 微服务只是业务逻辑代码，不会和HTML，CSS或者是其他的界面混合。

**微服务的缺点**：

* 开发人员要处理分布式系统的复杂性，运维部署的成本大；

* 服务间的通信成本的上升；数据一致性；系统测试等。

## Eureka

Eureka是Netflix开源的一款提供服务注册和发现的产品，它提供了完整的Service Registry和Service Discovery实现。也是spring cloud体系中最重要最核心的组件之一。

### 服务中心

服务中心又称注册中心，管理各种服务功能包括服务的注册、发现、熔断、负载、降级等，有了服务中心调用关系会有什么变化，画几个简图来帮忙理解

### Eureka 原理

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务注册和发现。Eureka 采用了 C-S 的设计架构。Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server，并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。Spring Cloud 的一些其他模块（比如Zuul）就可以通过 Eureka Server 来发现系统中的其他微服务，并执行相关的逻辑。

Eureka由两个组件组成：

* Eureka服务器
* Eureka客户端。

Eureka服务器用作服务注册服务器。Eureka客户端是一个java客户端，用来简化与服务器的交互、作为轮询负载均衡器，并提供服务的故障切换支持。Netflix在其生产环境中使用的是另外的客户端，它提供基于流量、资源利用率以及出错状态的加权负载均衡。

用一张图来认识以下：

![1564971424537](img/cld/1.png)

上图简要描述了Eureka的基本架构，由3个角色组成：

1、Eureka Server

- 提供服务注册和发现

2、Service Provider

- 服务提供方
- 将自身服务注册到Eureka，从而使服务消费方能够找到

3、Service Consumer

- 服务消费方
- 从Eureka获取注册服务列表，从而能够消费服务

### Eureka单节点例子

使用***Spring initializer*** 勾选 ***Eureka client & server***，其中的```pom.xml``` 如下：

#### pom和code

~~~xml
       <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
~~~

~~~java
@SpringBootApplication
@EnableEurekaServer
public class SecondEruekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(SecondEruekaApplication.class, args);
    }

}
~~~

#### 配置

~~~properties
spring.application.name=spring-cloud-eureka

server.port=8000

#是否将自己注册到Eureka Server，默认为true
eureka.client.register-with-eureka=false
 
#表示是否从Eureka Server获取注册信息，默认为true。
eureka.client.fetch-registry=false

# 设置与Eureka Server交互的地址，查询服务和注册服务都需要依赖这个地址。默认是http://localhost:8761/eureka ；多个地址可使用 , 分隔。
eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/
~~~

#### 测试

访问：http://localhost:8000/ 

### Eureka集群例子

Eureka作为服务注册中心，应该保证其高可靠性，防止单点故障，所以在生产中要配置三台或者三台以上的配置中心来保证服务的稳定性，要点是：**将每台注册中心分别指向其他两台节点的注册中心即可**

配置文件：

~~~yml
---
spring:
  application:
    name: spring-cloud-eureka
  profiles: peer1
server:
  port: 8000
eureka:
  instance:
    hostname: peer1
  client:
    serviceUrl:
      defaultZone: http://peer2:8001/eureka/,http://peer3:8002/eureka/
---
spring:
  application:
    name: spring-cloud-eureka
  profiles: peer2
server:
  port: 8001
eureka:
  instance:
    hostname: peer2
  client:
    serviceUrl:
      defaultZone: http://peer1:8000/eureka/,http://peer3:8002/eureka/
---
spring:
  application:
    name: spring-cloud-eureka
  profiles: peer3
server:
  port: 8002
eureka:
  instance:
    hostname: peer3
  client:
    serviceUrl:
      defaultZone: http://peer1:8000/eureka/,http://peer2:8001/eureka/
~~~

将该项目package成为jar包，然后执行下面命令：

~~~bash
java -jar second-erueka-0.0.1-SNAPSHOT.jar --spring.profiles.active=peer1
java -jar second-erueka-0.0.1-SNAPSHOT.jar --spring.profiles.active=peer2
java -jar second-erueka-0.0.1-SNAPSHOT.jar --spring.profiles.active=peer3
~~~

### 服务注册、服务发现

服务注册中心、服务提供者、服务消费者，其中服务注册中心就是我们上一篇的eureka单机版启动既可，流程是首先启动注册中心，服务提供者生产服务并注册到服务中心中，消费者从服务中心中获取服务并执行。

#### 服务提供

我们假设服务提供者有一个hello方法，可以根据传入的参数，提供输出“hello xxx，this is first messge”的服务









## 服务网关zuul

外部的应用如何来访问内部各种各样的微服务呢？在微服务架构中，后端服务往往不直接开放给调用端，而是通过一个API网关根据请求的`url`，路由到相应的服务。当添加API网关后，在第三方调用端和服务提供方之间就创建了一面墙，这面墙直接与调用方通信进行权限控制，后将请求均衡分发给后台服务端。

## 分布式配置中心`config`

### describe

马丁福乐，在自己的文章中论述：微服务有很多个，**可以有一个轻量级集中式管理来协调这些服务**，下面的这个场景其实很常见：在我们企业的开发中有很多的微服务（boot项目），每一个微服务都有一个配置文件且这些配置文件各自为营，如此多的配置文件，会给运维工程师带来巨大的压力。所以说需要一套集中式配置管理设施是必不可少的，`SpringCloud`提供了`ConfigServer`来解决这个问题。

![](img/cld/2.png)

A，B，C...都是微服务，`Config Sever`自己也是一个微服务，配置中心有服务端和客户端，和`erueka`一样。也就是说A，B，C微服务的配置自己不在携带，而是交给配置中心来管理，所有的配置文件都放在`Git`仓库，当我们修改了远程仓库之后，有配置中心获取最新的配置，所有的微服务从配置中心获得配置，进行下一步操作。

`config server`是班长，每一个同学是微服务，右边是班长。

### what

`Spring Cloud Config` 为微服务架构中的微服务提供集中化的外部配置支持，假设DBA，修改了库，通知运维，运维要通知Java工程师修改`yml`配置文件，如果此时Java工程师在睡觉，有了配置中心之后，运维直接修改`git`上的配置，配置中心自动获取配置，`config`客户端从配置中心获取新的配置信息。Cloud推荐使用`git`来进行配置中心的集中式管理。

配置发生变化的时候，服务不需要重启即可感知到相应的变化，并应用新的配置。所有的配置信息已REST接口的信息进行暴露。