---
layout: post
title: Bean生命周期
description: Bean生命周期。
author: MuZhi
date: 2024-5-8 01:07:03 +0800
categories: [Spring,Bean生命周期]
tags: [Bean生命周期,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F-logo.png
  alt: Bean创建过程.
---

## Bean 生命周期流程

![Bean生命周期](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/Bean%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)


> 在Spring 容器启动过程中，会调用`AbstractApplicationContext`类中`refresh()`，该方法是 Spring 容器启动的核心方法。其中，Spring Bean 的创建就是在该方法中的`finishBeanFactoryInitialization()`中进行的，在 Spring Bean 的创建之前，其实 Spring 在`refresh()`方法中。

1. 生成`BeanDefinition`，并存储到一个 `beanDefinitionMap`对象中。Spring 容器启动时，会根据配置的扫描路径得到所有的 class 文件资源；然后通过 ASM 技术将每个文件资源解析为`MetadataReader`对象；最后根据`MetadataReader`对象生成`BeanDefinition`，并存储到`beanDefinitionMap`对象中。

```java
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);
```

2. 合并`BeanDefinition`。

   > 为什么要做合并操作 ?
   >
   > 是基于Spring 中的父子 BeanDefinitiong 功能点考虑。

3. 将 Spring 容器中内置的 `BeanPostProcessor`和扫描得到的自定义所有的`BeanPostProcessor`添加到`beanPostProcessors`集合对象中。

```java
private final List<BeanPostProcessor> beanPostProcessors = new BeanPostProcessorCacheAwareList();
```

基于上述三点，Spring 容器就可以根据合并后的 `BeanDefinition`进行 Bean 对象的创建。Spring 容器 Bean 创建的源码主要是：`AbstractAutowireCapableBeanFactory`类 `createBean()`、`doCreateBean()`、`initializeBean()`等方法。

## Bean创建

### 1. 实例化前：

- 在Bean 的实例化之前，Spring 提供了一个 `InstantiationAwareBeanPostProcessor`的扩展点，可以通过该扩展点，在 Bean 的实例化之前或者实例化之后完成相应的业务逻辑。

> 例：自定义了一个扩展点，该扩展点通过使用`@Component`注解修饰，并通过实现 `InstantiationAwareBeanPostProcessor`接口以及实现 `postProcessBeforeInstantiation()`方法，该方法就会在Bean 的实例化之前被调用。如果我们在`postProcessBeforeInstantiation()`中返回值不为 null，接下来 `userService`的创建就不会在经过后面完整的生命周期了，而是会直接跳到初始化后的执行阶段。
>
> >```java
> >public class MyInstantiationAwareBeanPostProcessor implements InstantiationAwareBeanPostProcessor {
> >
> >@Override
> >public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
> >   // 如果 beanName 是 "user"，则提前返回一个 Bean 实例，阻止正常的实例化过程
> >   if ("user".equals(beanName)) {
> >       System.out.println("在实例化之前 " + beanName);
> >       // 这里可以创建一个代理对象或者修改后的实例来替代原始的 Bean 实例
> >       return new User(); // User 是一个自定义类
> >   }
> >   return null; // 继续正常的实例化过程
> >}
> >}
> >
> >@Component
> >public class User {
> >public User() {
> >   System.out.println("通过默认构造函数创建的 User 实例");
> >}
> >
> >public void doSomething() {
> >   System.out.println("User 做点什么");
> >}
> >}
> >@Test
> >public void testAppConfig() {
> >   AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("com.muzhi.pojo");
> >   // 注册配置类或Bean定义
> >   context.register(AppConfig.class);
> >   User user = context.getBean(User.class);
> >   user.doSomething();
> >   context.close();
> >}
> >```

### 2. 实例化：

- 接下来就可以根据`BeanDefinition`实例化 Bean 对象了，该阶段的主要执行逻辑如下：
  - 首先判断该`BeanDefinition`中是否设置了`Supplier`，如果设置了则调用`Supplier`的 get() 获取对象；
  - 如果没有设置`Supplier`，则会检查该`BeanDefinition`中是否设置了 `factoryMethod`，如果设置了，则会调用工厂方法获取对象。
  - 如果也没有设置工厂方法，则会执行推断构造方法，根据推断处理的构造方法示例化 Bean 对象。


### 3. BeanDefinition 的后置处理：

- 实例化 Bean 对象之后，就可以为其属性赋值，不过在属性赋值之前，Spring 提供了一个扩展点：`MergedBeanDefinitionPostProcessor`。可以通过该扩展点对此时的`BeanDefinition`对象进行加工。

```java
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.beans.factory.support.GenericBeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class PostProcessorDemo {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        context.register(PostProcessorConfig.class);
        context.refresh();

        // 获取处理后的Bean
        MyBean myBean = context.getBean(MyBean.class);
        System.out.println("Custom property value: " + myBean.getCustomProperty());

        context.close();
    }
}

@Configuration
class PostProcessorConfig {

    @Bean
    public static MyBeanDefinitionPostProcessor myBeanDefinitionPostProcessor() {
        return new MyBeanDefinitionPostProcessor();
    }
}

class MyBeanDefinitionPostProcessor implements BeanDefinitionPostProcessor {

    @Override
    public void postProcessBeanDefinition(BeanDefinition bd, String beanName) {
        if ("MyBean".equals(beanName)) {
            // 确保是一个GenericBeanDefinition，以支持设置额外的属性
            GenericBeanDefinition gbd = (GenericBeanDefinition) bd;
            gbd.getPropertyValues().add("customProperty", "Hello, World!");
        }
    }

    @Override
    public void postProcessMergedBeanDefinition(BeanDefinition bd, String beanName) {
        // 可以在Bean定义合并后进行进一步的处理
    }
}

class MyBean {
    private String customProperty;

    public String getCustomProperty() {
        return customProperty;
    }

    // 其他方法...
}
```

### 4. 实例化后：

- 在`BeanDefinition`的后置处理之后，Spring 又提供了一个和步骤  1  中完全相同的扩容点：`InstantiationAwareBeanPostProcessor`。通过该扩展点的`postProcessAfterInstantiation()`也可以实现在 Bean 的实例化之后完成业务逻辑的功能。

```java
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.stereotype.Component;

@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        // 可以在 Bean 初始化之前执行一些操作
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        // 可以在 Bean 初始化之后执行一些操作
        if ("myBean".equals(beanName)) {
            // 假设 MyBean 有一个 "setName" 方法
            ((MyBean) bean).setName("Processed by BeanPostProcessor");
        }
        return bean;
    }

    @Override
    public Object postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
        // Bean 实例化之后、属性填充之前的处理
        if ("myBean".equals(beanName)) {
            System.out.println("MyBean instance created: " + bean);
            // 可以在这里修改 Bean 实例的某些属性或执行其他逻辑
            // 注意：此时 Bean 的属性还未设置
        }
        return bean;
    }
}

@Component
class MyBean {
    private String name;

    // 假设有一个构造器
    public MyBean() {
    }

    // 普通方法
    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

### 5. 自动注入：

- Spring 自动注入

### 6. 处理属性：

- 该步骤也是通过`InstantiationAwareBeanPostProcessor`扩展来实现的，它会处理`@Autowired`、`@Resource`、`@Value`等注解。通过实现该扩展点的`postProcessProperties()`。

### 7. Aware 接口回调：

- 依赖注入完成后，Spring会判断该对象是否实现了 `BeanNameAware`、`BeanClassLoaderAware`、`BeanFactoryAware`接口。
- 如果实现了，Spring就会相应的调用`setBeanName()`、`setClassLoader()`、`setBeanFactory()`方法并传入相应的 `BeanName`、`BeanClassLoader`、`BeanFactory`参数。

### 8. 初始化前：

- 该步骤会遍历`BeanPostProcessors`对象，并执行它们的`postProcessBeforeInitialization()`方法。
- 其中，`InitDestroyAnnotationBeanPostProcessor`后置处理器会在该步骤中执行被`@PostConstruct`注解声明的方法；
- `ApplicationContextAwareProcessor`后置处理器会在该步骤执行一系列`Aware`接口的回调；
- 当，如果自定义了 `BeanPostProcessor`后置处理器并实现了`postProcessBeforeInitialization()`方法，也会在该步骤中被调用。

### 9. 初始化：

- 该步骤会先判断当前 Bean 对象是否实现了`InitializingBean`接口，如果实现了，则会调用当前对象中的`afterPropertiesSet()`方法。
- 另外，如果配置了初始化方法，也会在此步骤中调用。

### 10. 初始化后：

- 该步骤会遍历`BeanPostProcessors`对象，并执行`postProcessAfterInitialization()`方法。
- 也可以对 Bean 进行最终处理，如：AOP 的实现。

## Bean使用

Spring 中 Bean 对象创建完成后，会放入到`singletonObjects`的Map对象中，称为单例池；接下来使用Bean 对象，直接从单例池中获取。

```java
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);
```

## Bean销毁

在`AbstractAutowireCapableBeanFactory` 类`doCreateBean()`方法中，执行完Bean 的初始化方法后，会执行`registerDisposableBeanIfNecessary()`，该方法的主要作用就是判断当前Bean 对象是否是一个`DisposableBean`，如果是则会把当前Bean包装为`DisposableBeanAdapter`适配器对象并放入到一个`disposableBeans`的 Map 对象中缓存起来。

其中，判断当前 Bean 对象中是否是一个`DisposableBean`的逻辑如下：

1. 当前 Bean 对象是否实现了`DisposableBean`接口；

2. 当前 Bean 对象是否实现了`AutoCloseable`接口；

3. 当前 Bean 对象是否配置了`destriyMethod()`方法；

4. 遍历所有的`DestructionAwareBeanPostProcessor`后置处理器，并通过`requiresDestruction()`方法进行判断，当前 Bean 对象是否返回**`true ` ** 。

   > **注意：** `InitDestroyAnnotationBeanPostProcessor`后置处理器的`requiresDestruction()`方法会判端当前 Bean 对象中是否使用`@PreDestroy`注解，如果存在，也会返回true

缓存了`DisposableBean`对象后，Spring 中的 Bean对象在什么时候会被销毁？

> 在Spring 容器关闭时，有两中方式进行关闭Spring容器：
>
> ```java
> AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext("com.muzhi.pojo");
> ac.close();// 方式一
> ac.registerShutdownHook();// 方式二
> ```
>
> 方式一：立即关闭Spring容器。
>
> 方式二：调用 JDK 关闭钩子来执行关闭容器操作。

Spring 容器关闭，就会销毁所有的单例 Bean。在单例Bean销毁之前，会先遍历`DisposableBeans`对象，并调用`DisposableBean`的`destroy()`方法。所谓单例 Bean 的销毁，无非就是从单例池等缓存对象移除Bean对象。

