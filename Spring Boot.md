# Spring Boot

## Boot简介

2014年，Spring4.0 时候，伴随开发出：简化Spring 应用开发的一个框架，整个spring技术栈的大整合，javaee开发的一站式解决方案。 

### Boot的优点：

* 快速创建独立运行的Spring项目以及与主流的框架集成
* 使用嵌入式的Servlet容器，应用无需打成war包（打成war包之后，部署到服务器的时候，需要服务器有Tomcat的环境）直接打成jar包，使用java -jar直接来运行，因为Boot直接集成了Tomcat。
* Boot中有很多的starters，也即启动器，作用是帮我们进行自动的依赖管理和版本控制。如果我们想要用某一块的功能，就会有对应的启动器。比如我们要使用web功能，我们导入web启动器，那么web功能里面要带的其他的jar包，包括每一个jar包的版本，Boot都会将我们控制好，如果我们想要用jdbc相关的功能，直接导入JDBC相关的staters，使用redis相关的功能，就导入redis相关的starters 
* 大量的自动配置，也可以修改默认配置。  
* 准生产环境的运行时应用监控，和云计算的天然集成。

### Boot缺点：

* 入门容易，精通难。

## Boot与微服务

### All in one 的单体应用：

一个project打成一个War包，然后部署到服务器的Tomcat里面。这样做的好处是：

* 开发简单，测试简单，只有一个应用

* 部署简单，只需要部署一次就OK了，不会给运维带来困难
* 扩展，容易，当我们的应用的负载能力不行的时候，直接水平扩展，将应用复制多份， 放置于多个服务器中，以这样的方式来提高并发能力。  

缺点：

* 牵一发而动全身；

 而微服务，，每一个功能，都可以作为一个独立的模块，运行在独立的进程中，作为一个服务对外提供。服务之前的通信使用HTTP RESTful 

## 环境准备

学习Spring boot之前，应该首选掌握内容：

1） Spring框架的使用经验

2） Maven进行项目构建和依赖管理

3） 熟练使用IDEA和Eclipse

**JDK1.8/ maven 3.x / IDEA2017 / Boot1.5.9Release**

需要将Maven的配置文件setting.xml的profiles标签添加：

~~~xml
 <profile>  
  	     <id>jdk‐1.8</id>  
 	<activation>   
 		 <activeByDefault>true</activeByDefault>    
	 <jdk>1.8</jdk> 
  	</activation>  
 	<properties>    
    	 <maven.compiler.source>1.8</maven.compiler.source>    
 		 <maven.compiler.target>1.8</maven.compiler.target>     
		 <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion> 
  </properties> 
</profile>

~~~

这部分就是IDEA和Maven的整合，如果不进行设置，IDEA默认使用的是自己的Maven，

## Boot的 Hello world

 浏览器发送hello请求，服务器接收请求并处理，响应Hello boot 字符串。这是一个典型的web应用，如果不使用boot去实现的话，首先我们会创建一个web项目，导入spring，springmvc 相关的依赖，然后编写一堆配置文件，完成之后将整个项目打成war包，然后放入到Tomcat里运行。

如果使拥Boot来解决这份事情的话：

1） 创建一个Maven工程

2） 导入boot相关的依赖，在boot的官网的quickstart中copy即可

~~~xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.7.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<!--她的父项目是：-->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.1.7.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
<!--有她来真正的管理Spring boot应用的所有依赖-->
~~~

3） 编写一个主程序，来编写spring boot的应用

4） 编写业务逻辑，包括controller，或者service，不需要再做任何的配置

5） 测试我们这个应用写好了没有，（不在向以前那样，整合服务器进来，进行运行，现在直接运行main方法即可）

**代码：**

~~~java
package com.isea.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @ResponseBody //  将hello world.. 写给浏览器
    @RequestMapping("hello") // 用来接收浏览器的"hello" 请求
    public String hello(){
        return "hello world.."; // 给浏览器返回hello world..,如果我们想要写给浏览器，需要结合一个注解，ResponseBody
    }
}
~~~

直接发送一个hello请求，浏览器，就会响应：

6） 简化部署，此时完全不用打war包，而是直接在pom.xml文件中添加一个插件的包，就可以将boot项目打成一个jar包，直接运行（这就是这个插件的作用）。

**package方法进行 打包**

打成jar包之后，找到打成jar包之后的路径，然后直接执行

然后在浏览器端测试是否成功。插件里面自带了tomcat环境，服务器没有tomcat环境也是没有问题的。打开我们自己打成的jar包，在lib里面有非常多的jar包，有我们在写程序的时候导入的jar包，还有tomcat的jar包。打包的时候携带了服务器，这就是boot的强大。

使用boot 向导（***Spring Initializer***）创建boot项目，可以非常的快速，非常的过瘾。

这些几乎都是没有用的，可以删掉。

static：放置静态资源，js，css，html，images等

template: 保存所有的模板页面。



 ## Spring 中的一些注解

### @SpringBootApplication注解

 说明这个类是SpringBoot的主配置类，SpringBoot 会运行这个类的main方法来启动**SpringBoot**应用。如果进入该注解，可以发现：

~~~java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
~~~

* ***@SpringBootConfiguration***  标注在某一个类上，表名这是 一个Spring Boot的配置类，该配置类里面有 ***@Configuration*** 注解，在配置类上标记该注解，表示这相当于是一个配置文件，***@Configuration***注解下还有一个注解 ***@Component*** ，表明配置类也是容器中的一个组件。

* ***EnableAutoConfiguration*** 告诉Spring Boot开启自动配置功能，在***@EnableAutoConfiguration*** 注解下还有

  ~~~java
  @AutoConfigurationPackage // 自动配置包，该注解内部有一个@Import({Registrar.class})，该注解会导入一个组件，这个组件的功能是：
  
  //将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；
   
  @Import({AutoConfigurationImportSelector.class}) // 导入自动配置导入选择器，有了这个选择器之后，可以导入该场景下的所需要的所有组件，并完成配置。将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中。有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；
  ~~~

  Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConﬁguration指定的值，将 这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己配置的东 西，自动配置类都帮我们；

### Spring的注解

~~~java
// 用来映射hello请求 
@RequestMapping("hello")

// 将返回的数据写给浏览器，如果放置在类上，表示该类下的所有方法的返回结果返回给浏览器，如果放在方法上仅仅表示该方法
@ResponseBody

// 该注解 == @ResponseBody + @Controller
@RestController

@Value("${person.last-name}") // 
private String name;
~~~

### 关于boot的config注解

~~~java
@Component  // 只有当前组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能，所以加入component注解
@ConfigurationProperties(prefix = "person")  // @ConfigurationProperties:告诉Boot将本类（Person）类中的所有属性和配置文件中相关的配置进行绑定
@Validated // 校验的功能
@PropertySource(value = {"classpath:person.properties"}) // 避免全局的配置文件太大，加载特定的配置文件 加载类路径下的配置文件，

// 如果不使用上面的配置属性注解，可以使用@Value注解，
public class Person {
    //@Value("${person.last-name}") // 从配置文件中获取值，为LastName赋值

    @Email // 表示lastName的格式必须是邮箱的格式
    private String lastName;

    @Value("#{10 * 2}") // 完成计算的功能，为age来赋值
    private Integer age;

    @Value("true") // 直接为boss来赋值
    private Boolean boss;
    // .......
}
~~~

以上赋值使用是YML文件中的值：

~~~yml
person:
  last-name: z3
  age: 23
  boss: false
  birth: 2018/02/01
~~~



## 配置文件

重点是***YML、properties***配置文件，Spring Boot的思想是约定大于配置，配置文件的主要目的是未来改变约定的配置项。

### yml配置文件

以数据为中心，KV的形式写。下面是一个例子：

```yml
person:
  last-name: z3 
  age: 23
  boss: false
  birth: 2018/02/01
  maps: {k1: v1,k2: v2} # map 
  lists: # list或者是set
    - l4
    - w5
  dog: # 对象
    name: 小狗
    age: 22
```

### yml配置文件值的注入

~~~java
@Component  
// 只有当前组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能，所以加入component注解
@ConfigurationProperties(prefix = "person")  
// @ConfigurationProperties:告诉Boot将本类（Person）类中的所有属性和配置文件中相关的配置进行绑定

// 如果不使用上面的配置属性注解，可以使用@Value注解，
public class Person {
    
    private String lastName;
    private Integer age;

    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
~~~

此外，还需要导入配置文件的处理器，这样在code的时候就会产生提示：

~~~xml
<!--导入配置文件处理器，配置文件进行绑定之后，就会有提示-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
~~~

### 配置文件注入值校验

~~~java
@Validated // 校验的功能
public class Person {
    //@Email // 表示lastName的格式必须是邮箱的格式
    private String lastName;
~~~

### properties配置文件 

properties配置文件在idea中默认utf-8可能会乱码：

![](img/boot/5.png)

## 配置文件的读取方式：

### 利用Environment来读取

所有的配置信息，都会加载到Environment实体中，因此我们可以通过这个对象来获取系统的配置，通过这种方式不仅可以获取`application.yml`配置信息，还可以获取更多的系统信息

配置文件如下：

~~~properties
#服务端口号
server.port=8081

app.proper.key=${random.uuid}
app.proper.id=${random.int}
app.proper.value=test123

app.demo.val=autoInject
~~~

读取文件：

~~~java
@RestController
public class HelloController {
    @Autowired
    private Environment environment;
    @GetMapping(path = "show")
    public String show(){
        Map<String, String> result = new HashMap<>(4);
        result.put("server.port", environment.getProperty("server.port"));
        result.put("app.demo.val", environment.getProperty("app.demo.val"));
        return JSON.toJSONString(result);
    }
}
~~~

其中的`JSON.toJSONString(result)`使用的是：

~~~xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.31</version>
</dependency>
~~~

读取到了之后，可以在浏览器看到如下返回的Json字符串：

![](img/boot/2.png)

***这里如果想要获取信息，必须要设置get方法***



说明读取到了配置文件中的信息。

### @Value注解的方式

**我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value。**

`@Value`注解可以将配置信息注入到Bean的属性，也是比较常见的使用方式，但有几点需要额外注意：

- 如果配置信息不存在会怎样？
- 配置冲突了会怎样（即多个配置文件中有同一个key时）？

通过 ${}，大括号内为配置的Key；如果配置不存在时，给一个默认值时，可以用冒号分割，后面为具体的值

~~~java
@RestController
public class HelloController {

    // 配置必须存在，且获取的是配置名为 app.demo.val 的配置信息
    @Value("${app.demo.val}")
    private String autoInject;
    // 配置app.demo.not不存在时，不抛异常，给一个默认值data。
    @Value("${app.demo.not:dada}")
    private String notExists;

    @GetMapping(path = "show")
    public String show(){
        Map<String, String> result = new HashMap<>(4);
        result.put("app.demo.val", autoInject);
        result.put("app.demo.not", notExists);
        return JSON.toJSONString(result);
    }
}
~~~

如下，说明读取到了配置文件中的信息。

![](img/boot/3.png)

### 对象映射的方式

**专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConﬁgurationProperties，来进行文件的映射**，如果我们想要我们在映射的时候有提示的话，添加如下的依赖；

~~~xml
<!--导入配置文件处理器，配置文件进行绑定之后，就会有提示-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
~~~

**配置文件.yaml文件**，将会使用这个配置文件对Person对象赋值

~~~yaml
person:
  last-name: z3
  age: 23
  boss: false
  birth: 2018/02/01
  maps: {k1: v1,k2: v2} # map
  lists: # list或者是set
  - l4
  - w5
  dog: # 对象
    name: 小狗
    age: 22
~~~

**Person**对象

~~~java
@Component
// 只有当前组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能，所以加入component注解
@ConfigurationProperties(prefix = "person")
// @ConfigurationProperties:告诉Boot将本类（Person）类中的所有属性和配置文件中相关的配置进行绑定
// 必须要设置set方法，set方式是完成值的设置的基础
// 如果不使用上面的配置属性注解，可以使用@Value注解，
public class Person {
    private String lastName;
    private Integer age;

    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    public Person() {
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }
}


// dog类
package com.isea.getbootconfig.model;

public class Dog {
    private String name;
    private Integer age;

    public Dog() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}

// 测试：
@RestController
public class HelloController {

    @Autowired
    private Person person;

    @GetMapping(path = "show")
    public String show() {
        return JSON.toJSONString(person);
    }
}
~~~

测试的结果如下图：

![](img/boot/4.png)

### 多profile

~~~yml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
     port: 8084
spring:
  profiles: prod
~~~

**激活的方式：**

* 在配置文件中指定 spring.proﬁles.active=dev
* 命令行：可以直接在测试的时候，配置传入命令行参数

~~~shell
  java -jar spring-boot-02-conﬁg-0.0.1-SNAPSHOT.jar --spring.proﬁles.active=dev；
~~~

* 虚拟机参数；

~~~shell
 -Dspring.proﬁles.active=dev 
~~~

配置文件的位置和优先级按照下面的方式进行排列。各个配置互补形成最终的配置。

![](img/boot/6.png)



## 日志

### 日志框架

一位程序员在开发一个大型的系统

* `System.out.println()`将关键的数据打印在控制台，注释，放开，写在一个文件中，该程序员产生了一个写一个日志框架的想法，例如叫做xxlogging.jar，在以后的开发中都可以使用。
* 随着时间的推移，该程序员想要给该框架添加个高大上的功能，比如说：异步模式，自动归档（将每天的日志形成一个文件，然后自动归档）xx-good-logging.jar
* 将以前的框架卸下来之后，换上新的框架，还需要重新修改API，如果以后还有该框架的升级xx-perfect-logging.jar，每次都修改代码的话，就会显得很麻烦，
* 该程序员想到了JDBC和数据库驱动的关系，JDBC是sun公司定义的接口，在具体的需要连接什么数据库的时候，只是需要将该厂商对JDBC的实现放进程序就行了，所以该程序员为日志框架写了一个统一的接口层：日志的抽象层，logging-abstract.jar,在具体的项目中，我们只是需要导入具体的日志实现即可。

**市面上的一些日志框架：**

`JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....`

| 日志的抽象层（日志门面）                                     | 日志实现                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| ~~JCL（Jakarta  Commons Logging）~~      ~~jboss-logging~~     SLF4j（Simple  Logging Facade for Java） | Log4j  JUL（java.util.logging）  Log4j2  Logback |

Log4j  ，Logback，其实是同 一个人写的，最开始的时候是Log4j，只不过该日志框架有一定的性能问题，所以作者要对该日志框架进行升级，但是他觉得的对Log4j进行升级，改动太大，他就重写了一个日志框架Logback，他想到未来可能还有更多的日志框架，所以他就写了了日志框架的抽象，SLF4j。Log4j2  是Apache对日志框架的实现，该框架也非常的优秀。

**左边选一个抽象层、右边来选一个实现；**

| 日志抽象（日志门面） | 日志实现  |
| -------------------- | --------- |
| SLF4J                | Logback； |

底层是Spring框架，Spring框架默认是用JCL；‘

SpringBoot：和我们一样机智，选择的是上述我们说的。

### SLF4j的使用

以后在开发的时候，日志记录方法的使用，不应该直接调用日志的实现类，而是应该调用日志抽象层里面的方法，如果是spring boot的项目的，boot的依赖管理会有日志框架，其实就是上述表格中提到的。

代码层的使用：

~~~java
public class HelloWorld {
    public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
    }
}
~~~

![](img/boot/7.png)

图示：

![](img/boot/8.png)

其中的`adapter`叫做适配层，或者是适配器，向上实现`slf4j` 向下调用具体的日志实现，比如`log4j` ，该层主要的作用是用来适配本来不认识slf4j框架的日志实现，原因是这些框架出来的时候，slf4j还没有出现。这些适配层都是slf4j来实现的。

每个日志框架都有自己的配置文件，使用了`slf4j`之后，**配置文件还是做成日志实现框架的日志文件**



## Web开发

### 简介：

* 创建Spring Boot应用，选择我们需要使用的模块
* Boot已经帮我们完成了配置，我们只是需要少量的配置就能运行起来
* 自己编写业务代码

## view、controller、service、dao、model层级关系

**如图：**

![1562651342197](img/boot/1.png)



* view层：    结合control层，显示前台页面。
* control层：业务模块流程控制，调用service层接口。
* service层：业务操作实现类，调用dao层接口。
* dao层：     数据业务处理，持久化操作
* model层： pojo，OR maping，持久层

## websocket

webSocket主要的应用场景离不开即时通讯与消息推送，但只要应用程序需要在浏览器和服务器之间来回发送消息，就可以使用webSocket来降低客户端流量与服务器的负载。



Streaming Text Oriented Messaging Protocol 