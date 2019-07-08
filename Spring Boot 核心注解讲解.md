# Spring Boot 核心注解讲解

Spring Boot 最大的特点是无需 XML 配置文件，能自动扫描包路径装载并注入对象，并能做到根据 classpath 下的 jar 包自动配置。

## 三大核心注解

#### 1、@Configuration

org.springframework.context.annotation.Configuration

这是 Spring 3.0 添加的一个注解，用来代替 applicationContext.xml 配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。

相关注解:

**@Bean**

用来代替 XML 配置文件里面的 `<bean ...>` 配置。

**@ImportResource**

如果有些通过类的注册方式配置不了的，可以通过这个注解引入额外的 XML 配置文件，有些老的配置文件无法通过 `@Configuration` 方式配置的非常管用。

**@Import**

用来引入额外的一个或者多个 `@Configuration` 修饰的配置文件类。

**@SpringBootConfiguration**

这个注解就是 `@Configuration` 注解的变体，只是用来修饰是 Spring Boot 配置而已，或者可利于 Spring Boot 后续的扩展，源码如下。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

#### 2、@ComponentScan

> org.springframework.context.annotation.ComponentScan

这是 Spring 3.1 添加的一个注解，用来代替配置文件中的 `component-scan` 配置，开启组件扫描，即自动扫描包路径下的 `@Component` 注解进行注册 bean 实例到 context 中。

另外，`@ComponentScans` 是可重复注解，即可以配置多个，用来配置注册不同的子包。

#### 3、@EnableAutoConfiguration

> org.springframework.boot.autoconfigure.EnableAutoConfiguration

看全路径就知道，这是自 Spring Boot 诞生时添加的注解，用来提供自动配置，上面的两个都是 `spring-context` 包下的，不属于 Spring Boot，所以 Spring 3.0 之后的去 XML 配置方式已经为 Spring Boot 埋下了伏笔！

---

 `@SpringBootApplication` 注解就包含了以上 3 个主要注解，平时没有自定义配置的需求，则使用 `@SpringBootApplication` 注解完全就可以了

# Java面试题全集（上） <https://blog.csdn.net/jackfrued/article/details/44921941>

# Java面试题全集（中）

<https://blog.csdn.net/jackfrued/article/details/44931137>

# Java面试题全集（下）

<https://blog.csdn.net/jackfrued/article/details/44931161>