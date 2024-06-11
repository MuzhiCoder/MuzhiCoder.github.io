---
layout: post
title: Spring Boot 启动流程
description: Spring Boot启动流程
author: MuZhi
date: 2024-5-14 21:29:46 +0800
categories: [SpringBoot,启动流程]
tags: [SpringBoot启动流程,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/SpringBoot%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B-logo.png
  alt: Spring Boot.
---

# Spring Boot启动流程

## 1. 大概步骤

Spring Boot 的启动流程可以概括为以下几个主要步骤：

1. **初始化`SpringApplication`**：
   - 创建 `SpringApplication` 实例，它负责启动 Spring 应用。
2. **运行SpringApplication.run()**：
   - 调用 `SpringApplication.run()` 方法，传入应用的主类（通常是带有 `@SpringBootApplication` 注解的类）以及命令行参数（如果有的话）。
3. **加载应用配置**：
   - Spring Boot 会加载 `application.properties` 或 `application.yml` 文件中的配置信息。
4. **执行SpringApplication的run方法**：
   - 这个方法会创建并配置 `ApplicationContext`，它是 Spring 应用的核心容器。
5. **自动配置**：
   - Spring Boot 会根据类路径中的库、bean 的定义以及各种属性设置来自动配置 Spring 应用。
6. **注册并调用所有的`ApplicationContextInitializer`**：
   - 这些 Initializer 可以初始化 `ApplicationContext`，提供额外的配置。
7. **注册并调用所有的`ApplicationContextBeanPostProcessor`**：
   - 这些 `BeanPostProcessor` 可以在 bean 创建前后进行额外的处理。
8. **注册并调用所有的`BeanDefinitionRegistryPostProcessor`**：
   - 这些 PostProcessor 可以在 bean 定义注册到容器之前进行修改。
9. **注册并调用所有的`BeanPostProcessor`**：
   - 这些 PostProcessor 可以在 bean 初始化之后，设置之前进行处理。
10. **注册所有的`BeanFactoryPostProcessor`**：
    - 这些 PostProcessor 可以在 bean 工厂的后处理阶段进行处理。
11. **实例化所有的Bean**：
    - 根据注册的 bean 定义创建并初始化所有的 bean。
12. **调用所有的`ApplicationListener`**：
    - 通知所有的事件监听器，Spring 应用已经准备好。
13. **运行所有的`CommandLineRunner`和`ApplicationRunner`**：
    - 如果定义了这些接口的实现，它们会在应用启动后执行。
14. **启动内嵌的Web服务器**（如果应用是一个Web应用）：
    - 如果应用包含 Web 组件，Spring Boot 会启动内嵌的服务器（如 Tomcat、Jetty 或 Undertow）。
15. **应用启动完成**：
    - 至此，Spring Boot 应用已经完全启动，可以接收请求并处理业务逻辑。

## 2. 文本描述

首先需要有一个加了` @SpringBootApplication`注解的启动类

> ` @SpringBootApplication` 是由`@EnableAutoConfiguration`、`@SpringBootConfiguration`、`@ComponentScan` 三个注解连起来构成
>
> 其中`@EnableAutoConfiguration`是核心注解，有了它之后在启动时就会导入**自动配置** `AutoConfigurationImportSelector`类
>
> `AutoConfigurationImportSelector`类会将所有符合条件的`@Configuration`配置都进行加载
>
> `@SpringBootConfiguration`等同于`@Configuration` 就是将这个类标记为配置类会被加载到容器中
>
> `@ComponentScan`就是自动扫描并加载符合条件的 Bean
>
> 如果启动类中不需要增加配置内容也不需要指定扫描路径，那可以使用`@EnableAutoConfiguration`替代`@SpringBootApplication`也可以完成启动
>
> 注解加载完成后，就可以调用 `SpringApplication.run()` 方法

在`run()`开始执行后，会进行四个阶段：*服务构建*、*环境准备*、*容器创建*、*填充容器*

### 1. 服务构建

服务即是一个功能强大的Spring服务对象`SpringApplication`

在构造方法`SpringApplication()`中，首先要把传入的**资源加载器、主方法类**记录在内存中，然后逐一判断对应的服务类是否存在，来确定Web服务的类型，默认是`SERVLET`，即基于Servlet的Web服务，如：Tomcat；响应式非阻塞服务REACTIVE，如：spring-webflux；以及什么都不用的NONE，确定选择那个web服务之后，就要加载初始化类，会去读取所有`META-INF/spring.factories`文件中的**注册初始、上下文初始化、监听器**这三类配置，即`BootstrapRegistryInitializer、ApplicationContextInitializer、ApplicantListener`，在没有默认的**注册初始化**配置，而spring-boot和spring-boot-autoconfigure这两个工程中配置了 7 个**上下文初始化** 和 8 个 **监听器**，这些配置信息回在后续的启动过程中使用，也可以自定义这三个配置，只需要放到`spring.factories`文件中，就可以一并加载，接下来就会通过**运行栈** `stackTrace`判断出`main` 方法所在类，大概率为启动类；

### 2. 环境准备

1. 调用`run()`方法进入环境准备阶段，这个阶段会new一个后续会陆续使用到的**启动上下文** `BootstrappContext`同时逐一调用刚加载的“启动注册初始器”`bootstrapRegistryInitializers`中的初始`initialize()`方法，因为并没有默认的`bootstrapRegistryInitializers`所以默认并不执行什么；
2. 将`java.awt.headless`这个设置改为`true`表示缺少显示器、键盘等输入设备也可以正常启动；
3. 然后启动“运行监视器”`SpringApplicationRunListeners`,同时发布**启动**事件，它获取并加载Spring-Boot工程中spring.factories配置文件中的`EventPublishingRunListener`启动时，会将8个`SpringApplicationRunListeners`都进行引入，这样就可以通过监听这些事件，然后在启动流程中加入自定义逻辑；
4. 接下来就是要通过`prepareEnvironment()`方法，**组装启动参数**
   1. 首先第一步就是构建一个**可配置环境**`ConfigurableEnvironment`根据不同的Web服务类型会构造不同的环境，同样默认是Servlet，构造之后会加载很多诸如：**系统环境变量**`systemEnvironment`、**jvm系统属性** 、`systemProperties`等在内的4组配置信息，把这些配置信息都加载到一个叫做`propertySources`的内存集合中，这样后续使用到这些信息就无须重新加载。
   2. 这时也会通过**配置环境**`configureEnvironment()`方法将我们启动时传入的环境参数`args`进行设置，例如启动时传入的诸如**开发/生产** 环境配置等都会在这一步进行加载；同时在`propertySouces`集合的首个位置添加一个值为空的配置内容`configurationProperties`，接下来就会发布**环境准备完成**这个事件，刚加载进来的8个`Listener`会监听到这个事件，其中的部分**监听器**会进行相应处理，诸如**环境配置后处理监听器**`EnvironmentPostProcessorApplicantListener`会去加载`spring.factories`配置文件中**环境配置后处理器**`EnvironmentPostProcessor`，这里要注意**监听器**，通过观察者模式设计是逐一**串行**执行，并不是异步**并行**，需要等待所有监听器都处理完成之后，才会继续走后续的逻辑，将环境绑定到体内之后，剩下的就是考虑到刚创建**可配置环境**在一系列过程中可能会有变化进而做的补偿，通过二次更新保证匹配，紧接着将`spring.beaninfo.ignore`设为`true`表示不加载Bean的元数据信息，同时打印`Banner`图，这样环境变量就完成了

### 3. 容器创建

1. 通过`createrApplicationContext`来创建容器
   1. 首先根据服务类创建**容器** `ConfigurableApplicationContext`默认的服务类型是SERVLET,所以创建的是**注解配置的Servlet-Web服务容器**，即`AnnotationConfigServletWebServerApplicationContext`在这个过程中，会构造诸如：存放和生成Bean实例的**Bean工厂**`DefaultListableBeanFactory`、用来解析`@Component`、`@ComponentScan`等注解的**配置类后处理器** `ConfigurationClassPostProcessor`、用来解析`@Autowired`、`@Value`、`@Inject`等注解的**自动注解Bean后处理器** `AutowiredAnnotationBeanPostProcessor`等在内的属性对象把它们都放入容器中；
   2. 通过`prepareContext()`方法对容器中的部分属性进行初始化了，先用`postProcessApplicationContext()`方法设置**Bean名称生成器、资源加载器、类型转换器**等；接着就是要执行之前加载进来的**上下文初始化**`ApplicationContextInitializer`默认加载7个，容器ID、警告日志处理、日志监听都是在这里实现的；在发布**容器准备完成**监听事件之后会陆续为容器注册**启动参数、Banner、Bean引用策略和懒加载策略**等等，之后通过Bean定义加载器将**启动类**在内的资源加载到**Bean定义池**，`BeanDefinitionMap`中，以便于后续根据Bean定义创建Bean对象，然后发布一个**资源加载完成**事件

### 4. 填充容器



## 3. 源码描述

### 3.1. 服务构建

```java
public SpringApplication(ResourceLoader resourceLoader, Class <? > ...primarySources) {
    // 传入“资源加载器、主方法类” 记录在内存中
    this.resourceLoader = resourceLoader;
    Assert.notNull(primarySources, "PrimarySources must not be null");
    this.primarySources = new LinkedHashSet < > (Arrays.asList(primarySources));
    // 逐一判断对应的服务类是否存在
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    // 读取所有 META-INF/spring.factories文件中的 “注册初始、上下文初始化、监听器”这三类配置
    this.bootstrapRegistryInitializers = new ArrayList < > (
        getSpringFactoriesInstances(BootstrapRegistryInitializer.class));
    setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    // 通过“运行栈” stackTrace判断main方法所在类
    this.mainApplicationClass = deduceMainApplicationClass();
}
```

### 3.2. 环境构建

```java
public ConfigurableApplicationContext run(String...args) {
    if (this.registerShutdownHook) {
        SpringApplication.shutdownHook.enableShutdownHookAddition();
    }
    long startTime = System.nanoTime();
    // 启动上下文同时逐一调用刚刚加载的“启动注册初始化器”
    DefaultBootstrapContext bootstrapContext = createBootstrapContext();
    ConfigurableApplicationContext context = null;
    // 将“java.awt.headless”这个设置为 true
    configureHeadlessProperty();
    // 启动“运行监听器”
    SpringApplicationRunListeners listeners = getRunListeners(args);
    // 发布“启动”事件
    listeners.starting(bootstrapContext, this.mainApplicationClass);
    try {
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        // 通过prepareEnvironment方法 “组装启动参数” 组装源码向下↓↓↓
        ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        // 打印 Banner图
        Banner printedBanner = printBanner(environment);
        context = createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        refreshContext(context);
        afterRefresh(context, applicationArguments);
        Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
        if (this.logStartupInfo) {
            new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), timeTakenToStartup);
        }
        listeners.started(context, timeTakenToStartup);
        callRunners(context, applicationArguments);
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, listeners);
    }
    try {
        if (context.isRunning()) {
            Duration timeTakenToReady = Duration.ofNanos(System.nanoTime() - startTime);
            listeners.ready(context, timeTakenToReady);
        }
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, null);
    }
    return context;
}
```

组装启动参数：

```java
private ConfigurableEnvironment prepareEnvironment(SpringApplicationRunListeners listeners,
    DefaultBootstrapContext bootstrapContext, ApplicationArguments applicationArguments) {
    // 构建一个“可配置环境” 根据不同的WEB服务类型会构造不同的环境，默认Servlet
    ConfigurableEnvironment environment = getOrCreateEnvironment();
    // 构造后加载很多诸如“系统环境变量”，“JVM系统属性”等，并将这些配置信息加载到PropertySources内存集合中
    configureEnvironment(environment, applicationArguments.getSourceArgs());
    ConfigurationPropertySources.attach(environment);
    // 发布“环境准备完成”事件
    listeners.environmentPrepared(bootstrapContext, environment);
    DefaultPropertiesPropertySource.moveToEnd(environment);
    Assert.state(!environment.containsProperty("spring.main.environment-prefix"),
        "Environment prefix cannot be set via properties.");
    bindToSpringApplication(environment);
    if (!this.isCustomEnvironment) {
        EnvironmentConverter environmentConverter = new EnvironmentConverter(getClassLoader());
        environment = environmentConverter.convertEnvironmentIfNecessary(environment, deduceEnvironmentClass());
    }
    // 二次更新保存
    ConfigurationPropertySources.attach(environment);
    return environment;
}
```

### 3.3. 容器创建

```java
public ConfigurableApplicationContext run(String...args) {
    if (this.registerShutdownHook) {
        SpringApplication.shutdownHook.enableShutdownHookAddition();
    }
    long startTime = System.nanoTime();
    // 启动上下文同时逐一调用刚刚加载的“启动注册初始化器”
    DefaultBootstrapContext bootstrapContext = createBootstrapContext();
    ConfigurableApplicationContext context = null;
    // 将“java.awt.headless”这个设置为 true
    configureHeadlessProperty();
    // 启动“运行监听器”
    SpringApplicationRunListeners listeners = getRunListeners(args);
    // 发布“启动”事件
    listeners.starting(bootstrapContext, this.mainApplicationClass);
    try {
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        // 通过prepareEnvironment方法 “组装启动参数” 组装源码向上↑↑↑
        ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        // 打印 Banner图
        Banner printedBanner = printBanner(environment);
        // 创建容器，首先根据服务类型创建
        context = createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        // 对容器中的部分属性进行初始化
        prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        
        refreshContext(context);
        afterRefresh(context, applicationArguments);
        Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
        if (this.logStartupInfo) {
            new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), timeTakenToStartup);
        }
        listeners.started(context, timeTakenToStartup);
        callRunners(context, applicationArguments);
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, listeners);
    }
    try {
        if (context.isRunning()) {
            Duration timeTakenToReady = Duration.ofNanos(System.nanoTime() - startTime);
            listeners.ready(context, timeTakenToReady);
        }
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, null);
    }
    return context;
}
```

创建容器：

```java
public GenericApplicationContext() {
	this.customClassLoader = false;
	this.refreshed = new AtomicBoolean();
	// 存放Bean实例的“Bean工厂”
	this.beanFactory = new DefaultListableBeanFactory();
}

public AnnotationConfigServletWebServerApplicationContext() {
	this.annotatedClasses = new LinkedHashSet();
    // 解析@Component、@ComponentScan等注解的“配置类后处理器” ConfigurationClassPostProcessor
    // 解析@Autowired、@Value、@Inject等注解的“自动注解Bean后处理器” AutowiredAnnotationBeanPostProcessor
	this.reader = new AnnotatedBeanDefinitionReader(this);
	this.scanner = new ClassPathBeanDefinitionScanner(this);
}
```

容器中部分属性初始化：

```java
private void prepareContext(DefaultBootstrapContext bootstrapContext, ConfigurableApplicationContext context,
    ConfigurableEnvironment environment, SpringApplicationRunListeners listeners,
    ApplicationArguments applicationArguments, Banner printedBanner) {
    context.setEnvironment(environment);
    // 设置“Bean名称生成器、资源加载器、类型转换器”等
    postProcessApplicationContext(context);
    addAotGeneratedInitializerIfNecessary(this.initializers);
    // 执行之前加载进来的“上下文初始化” ApplicationContextInitializer
    applyInitializers(context);
    listeners.contextPrepared(context);
    bootstrapContext.close(context);
    if (this.logStartupInfo) {
        logStartupInfo(context.getParent() == null);
        logStartupProfileInfo(context);
    }
    // Add boot specific singleton beans
    ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
    beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
    if (printedBanner != null) {
        beanFactory.registerSingleton("springBootBanner", printedBanner);
    }
    if (beanFactory instanceof AbstractAutowireCapableBeanFactory autowireCapableBeanFactory) {
        autowireCapableBeanFactory.setAllowCircularReferences(this.allowCircularReferences);
        if (beanFactory instanceof DefaultListableBeanFactory listableBeanFactory) {
            listableBeanFactory.setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
        }
    }
    if (this.lazyInitialization) {
        context.addBeanFactoryPostProcessor(new LazyInitializationBeanFactoryPostProcessor());
    }
    // 加载“启动参数、引用策略、属性排序、Banner、加载策略”
    context.addBeanFactoryPostProcessor(new PropertySourceOrderingBeanFactoryPostProcessor(context));
    if (!AotDetector.useGeneratedArtifacts()) {
        // Load the sources
        Set < Object > sources = getAllSources();
        Assert.notEmpty(sources, "Sources must not be empty");
        // 通过Bean定义加载器将“启动类”在内的资源，加载到“Bean定义池” BeanDefinitionMap中
        load(context, sources.toArray(new Object[0]));
    }
    // 发布“资源加载完成”事件
    listeners.contextLoaded(context);
}
```

### 3.4. 填充容器

```java
public ConfigurableApplicationContext run(String...args) {
    if (this.registerShutdownHook) {
        SpringApplication.shutdownHook.enableShutdownHookAddition();
    }
    long startTime = System.nanoTime();
    // 启动上下文同时逐一调用刚刚加载的“启动注册初始化器”
    DefaultBootstrapContext bootstrapContext = createBootstrapContext();
    ConfigurableApplicationContext context = null;
    // 将“java.awt.headless”这个设置为 true
    configureHeadlessProperty();
    // 启动“运行监听器”
    SpringApplicationRunListeners listeners = getRunListeners(args);
    // 发布“启动”事件
    listeners.starting(bootstrapContext, this.mainApplicationClass);
    try {
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        // 通过prepareEnvironment方法 “组装启动参数” 组装源码向上↑↑↑
        ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        // 打印 Banner图
        Banner printedBanner = printBanner(environment);
        // 创建容器，首先根据服务类型创建
        context = createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        // 对容器中的部分属性进行初始化
        prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        // 自动装配 
        refreshContext(context);
        afterRefresh(context, applicationArguments);
        Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
        if (this.logStartupInfo) {
            new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), timeTakenToStartup);
        }
        listeners.started(context, timeTakenToStartup);
        callRunners(context, applicationArguments);
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, listeners);
    }
    try {
        if (context.isRunning()) {
            Duration timeTakenToReady = Duration.ofNanos(System.nanoTime() - startTime);
            listeners.ready(context, timeTakenToReady);
        }
    } catch (Throwable ex) {
        throw handleRunFailure(context, ex, null);
    }
    return context;
}
```

