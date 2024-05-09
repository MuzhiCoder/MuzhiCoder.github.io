---
layout: post
title: 注解@Autowired、@Resource和@Inject的区别
description: Autowired,Resource,Inject的区别。
author: MuZhi
date: 2024-5-8 18:02:41 +0800
categories: [Spring,注解]
tags: [注解,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/@Autowired%E5%92%8C@Resource%E4%BB%A5%E5%8F%8A@Inject%E5%8C%BA%E5%88%AB-logo.png
  alt: 设计模式.
---

Spring 中支持使用 @Autowired，@Resource，@Inject三个注解来实现属性的依赖注入

## @Autowired 源码

```java
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {

	/**
	 * Declares whether the annotated dependency is required.
	 * <p>Defaults to {@code true}.
	 */
	boolean required() default true;

}
```

## @Resource 源码

```java
@Target({ElementType.TYPE, ElementType.FIELD, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Repeatable(Resources.class)
public @interface Resource {
    String name() default "";

    String lookup() default "";

    Class<?> type() default Object.class;

    AuthenticationType authenticationType() default Resource.AuthenticationType.CONTAINER;

    boolean shareable() default true;

    String mappedName() default "";

    String description() default "";

    public static enum AuthenticationType {
        CONTAINER,
        APPLICATION;

        private AuthenticationType() {
        }
    }
}
```

## @Inject 源码

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.CONSTRUCTOR, ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER})
public @interface Inject {
}
```

## 注解的区别

### 1. 注解中属性的定义不同

- `@Autowired` 注解仅包含 **1** 个 `required`属性，表示依赖注入的属性是否允许为 null;
- `@Resource` 注解包含 **7** 个属性，其中最重要的是 `name`和`type` 2个属性，分别用于指定依赖注入时 Bean 的名称和类型；
- `@Inject` 注解没有任何属性。

### 2. 注解的作用范围不同

- `@Autowired` 注解可以作用在构造方法、方法、方法参数、字段或者注解上；

  ```java
  @Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
  ```

- `@Resource` 注解可以作用在类、字段或者方法上；

  ```java
  @Target({TYPE, FIELD, METHOD})
  ```

- `@Inject` 注解可以作用在方法、构造方法或者字段上。

  ```java
  @Target({ElementType.CONSTRUCTOR, ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER})
  ```

### 3. 出处不同

- `@Autowired` 注解是 Spring 提供的，只有 Spring IOC 容器支持；
- `@Resource` 注解是 JDK 提供的，遵循 JSR 250 规范，定义在JDK的 rt.jar 中，无须单独引入；
- `@Inject` 注解是 JDK 提供的，遵循 JSR 330 规范，定义在 javax.Inject.jar 中，需要单独引入；

### 4. Spring中解析注解的后置处理器不同

- `@Autowired` 和 `@Inject` 都是使用`AutowiredAnnotationBe按PostProcessor`后置处理器进行处理；
- `@Resource` 注解则是使用 `CommonAnnotationBeanPostProcessor` 后置处理器进行处理；

### 5.默认的装配方式不同

- `@Autowired`默认是按照 byType 的方式进行装配，如果想按照 byName 的方式进行装配，需要搭配`@Qualifier`注解一起使用；
- `@Resource` 注解默认则是按照 byName的方式进行装配的；
- `@Inject` 默认是安装 byType的方式进行装配，如果想按照 byName的方式进行装配，需要搭配`@Named` 注解一起使用。

### 6.装配的执行流程不同

- `@Autowired` 和 `@Inject`注解的执行流程大致相同：
  - 默认都是先按照 byType 的方式进行匹配，如果有且仅有一个；则匹配成功；
  - 如果一个都没有匹配到，则抛出异常；
  - 如果匹配多个，则继续判断是否配置了`@Qualifier`注解或者`@Named`注解，如果配置了，则按照指定的名称进行匹配；如果没有配置，则按照属性名进行匹配；如果能够匹配到，则匹配成功，反之则抛出异常。
- `@Resource`注解的执行流程：
  - 如果指定了name属性，则会根据指定的 name 去 Spring 容器中查找 Bean ，匹配不到则抛出异常；
  - 如果没有指定name，则会先判断Spring 容器是否存在当前的属性名或者方法参数名的Bean ，存在则根据该名称去获取 Bean，不存在则按照注入点的类型去匹配，如果没有匹配到或者匹配多个，就会抛出异常。

## @Autowired装配示例

### 1. 构造器注入：

```java
@Component
public class ServiceComponent {

    private final DependencyComponent dependencyComponent;

    @Autowired
    public ServiceComponent(DependencyComponent dependencyComponent) {
        this.dependencyComponent = dependencyComponent;
    }
    // ...
}
```

在这个例子中，`ServiceComponent` 通过构造器接收一个 `DependencyComponent` 的实例。Spring 容器在创建 `ServiceComponent` 实例时自动注入相应的依赖。

### 2. Setter方法注入：

```java
@Component
public class ServiceComponent {

    private DependencyComponent dependencyComponent;

    @Autowired
    public void setDependencyComponent(DependencyComponent dependencyComponent) {
        this.dependencyComponent = dependencyComponent;
    }
    // ...
}
```

使用 `@Autowired` 注解的 Setter 方法允许在对象创建后注入依赖。

### 3. 字段注入：

```java
@Component
public class ServiceComponent {

    @Autowired
    private DependencyComponent dependencyComponent;
    // ...
}
```

字段注入是最简单的形式，Spring 容器将自动注入标注了 `@Autowired` 的字段。

### 4. 普通方法注入：

```java
@Component
public class ServiceComponent {

    private DependencyComponent dependencyComponent;

    @Autowired
    public void injectDependencyComponent(DependencyComponent dependencyComponent) {
        this.dependencyComponent = dependencyComponent;
    }
    // ...
}
```

在这个例子中，一个普通方法用于注入 `DependencyComponent`。

### 5. 配置方法注入：

```java
@Configuration
public class AppConfig {

    @Autowired
    @Bean
    public ServiceComponent serviceComponent(DependencyComponent dependencyComponent) {
        return new ServiceComponent(dependencyComponent);
    }
    // ...
}
```

### 注意事项

- 使用 `@Autowired` 时，如果存在多个匹配的 Bean，可以通过 `@Qualifier` 注解来指定注入哪一个。
- 如果构造器、Setter 方法或者普通方法上有多个参数，每个参数都可以使用 `@Autowired`，Spring 容器将为每个参数注入相应的依赖。
- 如果一个字段、构造器参数或 Setter 方法没有标注 `@Autowired`，Spring 容器将不会尝试自动注入，除非你在 Spring 配置中明确指定了自动装配的行为。
- 在字段注入的情况下，如果依赖注入失败，字段将保持 `null`。

### @Autowired存在多个匹配的 Bean，使用@Qualifier 注解来指定注入

#### 示例：

##### 1. 定义Bean

首先，定义两个实现了相同接口 `PaymentService` 的 Bean：

```java
@Service
public class CreditCardPaymentService implements PaymentService {
    // 实现支付逻辑
}

@Service
public class PayPalPaymentService implements PaymentService {
    // 实现支付逻辑
}
```

##### 2. 使用 `@Autowired` 和 `@Qualifier`

在需要注入 `PaymentService` 的组件中，我们使用 `@Autowired` 和 `@Qualifier` 来指定注入哪一个 Bean：

```java
@Component
public class ShoppingCart {

    private final PaymentService paymentService;

    @Autowired
    public ShoppingCart(@Qualifier("CreditCardPaymentService") PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void checkout() {
        // 使用注入的 PaymentService 执行结账操作
        paymentService.processPayment();
    }
}
```

构造器注入了一个 `PaymentService` 对象。由于存在多个 `PaymentService` 的实现，我们使用 `@Qualifier("CreditCardPaymentService")` 来告诉 Spring 容器，我们希望注入名为 "CreditCardPaymentService" 的 Bean。

##### 3. 使用 `@Qualifier` 注解的 Setter 方法

```java
@Component
public class ShoppingCart {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(@Qualifier("PayPalPaymentService") PaymentService paymentService) {
        this.paymentService = paymentService;
    }
    // 其他方法...
}
```

在这个例子中，通过 Setter 方法注入了 `PaymentService`。同样，`@Qualifier("PayPalPaymentService")` 指定了要注入的 Bean 名称。

#### 注意事项：

- `@Qualifier` 注解不仅可以用于构造器和 Setter 方法，还可以用于普通方法和特定参数。
- 使用 `@Qualifier` 注解时，需要确保 Bean 的名称在容器中是唯一的。
- 在某些情况下，使用 `@Qualifier` 注解可能使得代码耦合到特定的 Bean 名称，这可能会降低代码的可维护性。因此，在使用 `@Qualifier` 时需要权衡利弊。

## @Resource装配示例

### 示例：

#### 1. 定义Bean

定义两个 Bean，这里以 `PaymentService` 接口及其两个实现类为例：

```java
@Service
public class CreditCardPaymentService implements PaymentService {
    // 实现信用卡支付逻辑
}

@Service
public class PayPalPaymentService implements PaymentService {
    // 实现 PayPal 支付逻辑
}
```

#### 2. 使用 `@Resource` 注解

在需要注入 Bean 的组件中，使用 `@Resource` 注解来指定注入的 Bean：

##### 2.1. 基于字段的注入：

```java
@Component
public class ShoppingCart {

    @Resource(name = "PayPalPaymentService")
    private PaymentService paymentService;

    public void checkout() {
        // 使用注入的 PaymentService 执行结账操作
        paymentService.processPayment();
    }
}
```

`@Resource` 注解用于字段注入，`name` 属性指定了要注入的 Bean 名称。

##### 2.2. 基于构造器的注入：

```java
@Component
public class ShoppingCart {

    private PaymentService paymentService;

    @Resource(name = "CreditCardPaymentService")
    public ShoppingCart(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
    // 其他方法...
}
```

`@Resource` 注解用于构造器参数，指定了要注入的 Bean 名称。

##### 2.3. 基于类型的字段注入：

```java
@Component
public class ShoppingCart {

    @Resource
    private PaymentService paymentService; // 基于类型注入

    public void checkout() {
        // 使用注入的 PaymentService 执行结账操作
        paymentService.processPayment();
    }
}
```

如果没有多个 `PaymentService` 类型的 Bean，Spring 将自动注入一个 `PaymentService` 类型的 Bean。

##### 2.4. 基于类型的构造器注入：

```java
@Component
public class ShoppingCart {

    private PaymentService paymentService;

    @Resource
    public ShoppingCart(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
    // 其他方法...
}
```

##### 2.5. 基于类型和 `@Qualifier` 的构造器注入：

存在多个 `PaymentService` 类型的 Bean，使用 `@Qualifier` 来解决冲突：

```java
@Component
public class ShoppingCart {

    private PaymentService paymentService;

    @Resource
    public ShoppingCart(@Qualifier("PayPalPaymentService") PaymentService paymentService) {
        this.paymentService = paymentService;
    }
    // 其他方法...
}
```

`@Qualifier("PayPalPaymentService")` 指定了注入 `PayPalPaymentService` Bean。

### 注意事项：

- `@Resource` 注解默认是通过名称进行注入的，如果要基于类型进行注入，可以省略 `name` 属性。
- 如果有多个 Bean 匹配注入点的类型，将抛出 `UnsatisfiedDependencyException` 异常。在这种情况下，需要使用 `@Qualifier` 注解或指定 `name` 属性来解决歧义。
- `@Autowired` 注解是 Spring 推荐的依赖注入方式，因为它支持更多 Spring 特有的功能，如自动装配的候选策略和更丰富的配置选项。

## @Inject装配示例

### 示例：

#### 1. 定义Bean

定义两个 Bean，以 `PaymentService` 接口及其实现类为例：

```java
@Component
public class CreditCardPaymentService implements PaymentService {
    // 实现信用卡支付逻辑
}

@Component
public class PayPalPaymentService implements PaymentService {
    // 实现 PayPal 支付逻辑
}
```

#### 2. 使用 `@Inject` 注解

##### 2.1. 基于构造器的注入：

```java
@Component
public class ShoppingCart {

    private final PaymentService paymentService;

    @Inject
    public ShoppingCart(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void checkout() {
        // 使用注入的 PaymentService 执行结账操作
        paymentService.processPayment();
    }
}
```

##### 2.2. 基于 Setter 方法的注入：

```java
@Component
public class ShoppingCart {

    private PaymentService paymentService;

    @Inject
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
    // 其他方法...
}
```

##### 2.3. 基于字段的注入：

```java
@Component
public class ShoppingCart {

    @Inject
    private PaymentService paymentService;

    // 其他方法...
}
```

### 注意事项：

- `@Inject` 注解与 `@Autowired` 类似，但它不提供 `@Autowired` 的所有功能，如指定 `required` 属性或通过 `@Qualifier` 解决多个 Bean 匹配问题。
- 在 Spring 中，通常推荐使用 `@Autowired`，因为它是 Spring 特有的，并且提供了更丰富的自动装配能力。
- 如果你决定使用 `@Inject`，确保你的项目中包含了 `javax.inject` 的依赖。
