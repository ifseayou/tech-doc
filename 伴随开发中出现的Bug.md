#  伴随开发中出现的Bug

```java
Spring Hibernate - Could not obtain transaction-synchronized Session for current thread

// 解决：在Dao层的方法上添加 @Transactional  Did you try adding an @Transactional to your DAO create method
```

~~~java 
// 如果出现了一个object类型，可以先转为String类型，然后在用Double.valueOf()方法将其转为Double这样的可计算类型。
Double.valueOf(obj.toString())
~~~

~~~javascript
// 在JavaScript中如何将一个可以转化为数字的类型转为Number类型
parseInt(**)
~~~

Maven依赖的问题：

~~~xml
<dependency>
    <groupId>com.joinbright</groupId>
    <artifactId>junit</artifactId>
    <version>3.8.1</version>
    <scope>system</scope>
    <systemPath>${basedir}/src/main/resources/lib/junit-3.8.1.jar</systemPath>
</dependency>
~~~

