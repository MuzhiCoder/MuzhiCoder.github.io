---
layout: post
title: Spring Boot 填充容器
description: Spring Boot启动流程,填充容器,IOC
author: MuZhi
date: 2024-6-11 09:47:01 +0800
categories: [SpringBoot,填充容器,IOC]
tags: [SpringBoot填充容器,笔记,IOC]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-imagesSpringboot%E5%A1%AB%E5%85%85%E5%AE%B9%E5%99%A8.png
  alt: Spring Boot.
---

# Spring Boot填充容器

> 填充容器，即：自动装配Bean

1. 第一步：通过`prepareRefresh()`方法，在已有的**系统环境 **基础上，准备SERVLET相关的环境Environment，其他的环境在上篇第二大阶段**环境准备** 中就已经注册完成了，通过**初始化属性资源** `initServletPropertySources()`方法，对**Servlet初始化参数** `servletContextInitParams` 和 `servletConfigInitParams` 进行赋值，

   ```java
   public static void initServletPropertySources(MutablePropertySources sources, @Nullable ServletContext servletContext, @Nullable ServletConfig servletConfig) {
   
       Assert.notNull(sources, "'propertySources' must not be null");
       String name = StandardServletEnvironment.SERVLET_CONTEXT_PROPERTY_SOURCE_NAME;
       if (servletContext != null && sources.get(name) instanceof StubPropertySource) {
           sources.replace(name, new ServletContextPropertySource(name, servletContext));
       }
       name = StandardServletEnvironment.SERVLET_CONFIG_PROPERTY_SOURCE_NAME;
       if (servletConfig != null && sources.get(name) instanceof StubPropertySource) {
           sources.replace(name, new ServletConfigPropertySource(name, servletConfig));
       }
   }
   ```

   然后，通过`validateRequiredProperties()`检验是否有必要填充的环境变量，可以在自定义**初始化属性资源** `initPropertySources()`方法中，

   ```java
   public void validateRequiredProperties() {
       MissingRequiredPropertiesException ex = new MissingRequiredPropertiesException();
       for (String key: this.requiredProperties) {
           if (this.getProperty(key) == null) {
               ex.addMissingRequiredProperty(key);
           }
       }
       if (!ex.getMissingRequiredProperties().isEmpty()) {
           throw ex;
       }
   }
   ```

   通过`setRequiredProperties()`将某些环境变量设置为必填

   ```java
   // 把"MYSQL_HOST"作为启动时必须验证的环境变量
   getEnvironment().setRequiredProperties("MYSQL_HOST");
   ```

   最后，完成监听器和事件初始化之后，环境准备就完成了

2. 第二三步：通过`obtainFreshBeanFactory()` 和 `prepareBeanFactory()`方法，在获取容器同时在使用`BeanFactory`之前进行一些准备工作，由于SpringBoot选择`ServletWebServerApplicationContext`作为容器，之前步骤已经构建好`beanFactiry`所以`obatainFreshBeanFactory`中不进行任何处理，**注意：在原始Spring中很多情况下会选择`ClassPathXmlApplicationContext`作为容器**，每次执行`obtainFreshBeanFactory`时，会通过它的`refreshBeanFactory()`方法重新构建`beanFactory`并重新加载**Bean定义** BeanDefinition，而是准备容器`prepareBeanFactory`过程中，主要准备**类加载器** `BeanClassLoader`，**表达式解析器**`BeanExpressionResolver`、**配置文件处理器** `PropertyEditorRegistrar`等系统处理器，以及两个**Bean后置处理器**；用来解析`Aware`接口的`ApplicationContextAwareProcessor` 用来处理自定义监听器注册和销毁的`ApplicationListenerDetector` 同时会注册一些**特殊Bean** 和 **系统级Bean** 系统环境`Environment`、系统属性`SystemProperties`等，将它们放入**特殊对象池** 和 **单例池** 中

   ```java
   protected void prepareBeanFactory(ConfigurableListableBeanFactory beanFactory) {
       // Tell the internal bean factory to use the context's class loader etc.
       beanFactory.setBeanClassLoader(getClassLoader());
       beanFactory.setBeanExpressionResolver(new StandardBeanExpressionResolver(beanFactory.getBeanClassLoader()));
       beanFactory.addPropertyEditorRegistrar(new ResourceEditorRegistrar(this, getEnvironment()));
   
       // Register early post-processor for detecting inner beans as ApplicationListeners.
       beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(this));
   }
   ```

3. 第四步：通过`postProcessBeanFactory()`方法，对BeanFactory进行额外设置或修改，这里主要定义了包括`request`、`session`在内的Servlet相关作用域Scopes，同时也注册与Servlet相关的一些特殊Bean，包括：`ServletRequest`、`ServletResponse`、`HttpSession`等；

   ```java
   public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
   	try {
   		Properties mergedProps = mergeProperties();
   
   		// Convert the merged properties, if necessary.
   		convertProperties(mergedProps);
   
   		// Let the subclass process the properties.
   		processProperties(beanFactory, mergedProps);
   	}catch (IOException ex) {
   	throw new BeanInitializationException("Could not load properties: " + ex.getMessage(), ex);
   	}
   }
   
   public static void registerWebApplicationScopes(ConfigurableListableBeanFactory beanFactory,@Nullable ServletContext sc) {
   
   	beanFactory.registerScope(WebApplicationContext.SCOPE_REQUEST, new RequestScope());
   	beanFactory.registerScope(WebApplicationContext.SCOPE_SESSION, new SessionScope());
   	if (sc != null) {
   		ServletContextScope appScope = new ServletContextScope(sc);
   		beanFactory.registerScope(WebApplicationContext.SCOPE_APPLICATION, appScope);
   		// Register as ServletContext attribute, for ContextCleanupListener to detect it.
   		sc.setAttribute(ServletContextScope.class.getName(), appScope);
   	}
   
   	beanFactory.registerResolvableDependency(ServletRequest.class, new RequestObjectFactory());
   	beanFactory.registerResolvableDependency(ServletResponse.class, new ResponseObjectFactory());
   	beanFactory.registerResolvableDependency(HttpSession.class, new SessionObjectFactory());
   	beanFactory.registerResolvableDependency(WebRequest.class, new WebRequestObjectFactory());
   	if (jsfPresent) {
   		FacesDependencyRegistrar.registerFacesDependencies(beanFactory);
   	}
   }
   ```

4. 第五步：开始执行非常核心的`invokeBeanFactoryPostProcessors()`方法，首先，逐一执行在第三个大阶段**容器创建**中，注册的各种**BeanFactory后置处理器** `beanFactoryPostProcessor()`，其中最主要的就是用来加载所有**Bean定义**的**配置处理器** `ConfigurationClassPostProcessor`通过它加载所有`@Configuration`配置类，同时检索指定的**Bean扫描路径** `componentScans`，然后通过**Bean扫描器** `ClassPathBeanDefinitionScanner`中的`doScan()`方法扫描每个类，将每个扫描出来的**Bean定义**都放到**Bean定义池** `beanDefinitionMap`中，同样也会扫描所有加了`@Bean、@Import`等注解的类和方法，将它们对应的**Bean定义** 也都放到 **Bean定义池** 中，这样后续就可以通过这些Bean定义构造相应的Bean对象。![invokeBeanFactoryPostProcessor](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-imagesinvokeBeanFactoryPostProcessor.png)

5. 第六步：通过`registerBeanPostProcessors()`方法，检索所有的**Bean后置处理器**，同时根据指定的`order`为它们进行排序，然后放入**后置处理器** `BeanPostProcessor`中，每一个Bean后置处理器，都会在Bean初始化之前和之后分别执行对应的逻辑；

   ```java
   public static void registerBeanPostProcessors(
       ConfigurableListableBeanFactory beanFactory, AbstractApplicationContext applicationContext) {
       String[] postProcessorNames = beanFactory.getBeanNamesForType(BeanPostProcessor.class, true, false);
   
       // Register BeanPostProcessorChecker that logs an info message when
       // a bean is created during BeanPostProcessor instantiation, i.e. when
       // a bean is not eligible for getting processed by all BeanPostProcessors.
       int beanProcessorTargetCount = beanFactory.getBeanPostProcessorCount() + 1 + postProcessorNames.length;
       beanFactory.addBeanPostProcessor(new BeanPostProcessorChecker(beanFactory, beanProcessorTargetCount));
   
       // Separate between BeanPostProcessors that implement PriorityOrdered,
       // Ordered, and the rest.
       List < BeanPostProcessor > priorityOrderedPostProcessors = new ArrayList < > ();
       List < BeanPostProcessor > internalPostProcessors = new ArrayList < > ();
       List < String > orderedPostProcessorNames = new ArrayList < > ();
       List < String > nonOrderedPostProcessorNames = new ArrayList < > ();
       for (String ppName: postProcessorNames) {
           if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
               BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
               priorityOrderedPostProcessors.add(pp);
               if (pp instanceof MergedBeanDefinitionPostProcessor) {
                   internalPostProcessors.add(pp);
               }
           } else if (beanFactory.isTypeMatch(ppName, Ordered.class)) {
               orderedPostProcessorNames.add(ppName);
           } else {
               nonOrderedPostProcessorNames.add(ppName);
           }
       }
   
       // First, register the BeanPostProcessors that implement PriorityOrdered.
       sortPostProcessors(priorityOrderedPostProcessors, beanFactory);
       registerBeanPostProcessors(beanFactory, priorityOrderedPostProcessors);
   
       // Next, register the BeanPostProcessors that implement Ordered.
       List < BeanPostProcessor > orderedPostProcessors = new ArrayList < > (orderedPostProcessorNames.size());
       for (String ppName: orderedPostProcessorNames) {
           BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
           orderedPostProcessors.add(pp);
           if (pp instanceof MergedBeanDefinitionPostProcessor) {
               internalPostProcessors.add(pp);
           }
       }
       sortPostProcessors(orderedPostProcessors, beanFactory);
       registerBeanPostProcessors(beanFactory, orderedPostProcessors);
   
       // Now, register all regular BeanPostProcessors.
       List < BeanPostProcessor > nonOrderedPostProcessors = new ArrayList < > (nonOrderedPostProcessorNames.size());
       for (String ppName: nonOrderedPostProcessorNames) {
           BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
           nonOrderedPostProcessors.add(pp);
           if (pp instanceof MergedBeanDefinitionPostProcessor) {
               internalPostProcessors.add(pp);
           }
       }
       registerBeanPostProcessors(beanFactory, nonOrderedPostProcessors);
   
       // Finally, re-register all internal BeanPostProcessors.
       sortPostProcessors(internalPostProcessors, beanFactory);
       registerBeanPostProcessors(beanFactory, internalPostProcessors);
   
       // Re-register post-processor for detecting inner beans as ApplicationListeners,
       // moving it to the end of the processor chain (for picking up proxies etc).
       beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(applicationContext));
   }
   ```

6. 第七八步：通过`initMessageSource()` 和 `initApplicationEventMulticaster()`方法，从**单例池** 中获取两个非常实用的Bean放在ApplicationContext中，一个是用于国际化，名为`messageSource`的Bean，可以通过自定义名为`MessageSource`的Bean，结合`messages.properties`配置文件就可以进行多语言的切换配置

   ```java
   protected void initMessageSource() {
       ConfigurableListableBeanFactory beanFactory = getBeanFactory();
       if (beanFactory.containsLocalBean(MESSAGE_SOURCE_BEAN_NAME)) {
           this.messageSource = beanFactory.getBean(MESSAGE_SOURCE_BEAN_NAME, MessageSource.class);
           // Make MessageSource aware of parent MessageSource.
           if (this.parent != null && this.messageSource instanceof HierarchicalMessageSource hms &&
               hms.getParentMessageSource() == null) {
               // Only set parent context as parent MessageSource if no parent MessageSource
               // registered already.
               hms.setParentMessageSource(getInternalParentMessageSource());
           }
           if (logger.isTraceEnabled()) {
               logger.trace("Using MessageSource [" + this.messageSource + "]");
           }
       } else {
           // Use empty MessageSource to be able to accept getMessage calls.
           DelegatingMessageSource dms = new DelegatingMessageSource();
           dms.setParentMessageSource(getInternalParentMessageSource());
           this.messageSource = dms;
           beanFactory.registerSingleton(MESSAGE_SOURCE_BEAN_NAME, this.messageSource);
           if (logger.isTraceEnabled()) {
               logger.trace("No '" + MESSAGE_SOURCE_BEAN_NAME + "' bean, using [" + this.messageSource + "]");
           }
       }
   }
   ```

   另一个是用于自定义广播事件，名为`applicationEventMulticaster`的Bean，有了它就可以通过`publishEvent()`方法进行事件的发布

   ```java
   protected void initApplicationEventMulticaster() {
       ConfigurableListableBeanFactory beanFactory = getBeanFactory();
       if (beanFactory.containsLocalBean(APPLICATION_EVENT_MULTICASTER_BEAN_NAME)) {
           this.applicationEventMulticaster =
               beanFactory.getBean(APPLICATION_EVENT_MULTICASTER_BEAN_NAME, ApplicationEventMulticaster.class);
           if (logger.isTraceEnabled()) {
               logger.trace("Using ApplicationEventMulticaster [" + this.applicationEventMulticaster + "]");
           }
       } else {
           this.applicationEventMulticaster = new SimpleApplicationEventMulticaster(beanFactory);
           beanFactory.registerSingleton(APPLICATION_EVENT_MULTICASTER_BEAN_NAME, this.applicationEventMulticaster);
           if (logger.isTraceEnabled()) {
               logger.trace("No '" + APPLICATION_EVENT_MULTICASTER_BEAN_NAME + "' bean, using " +
                   "[" + this.applicationEventMulticaster.getClass().getSimpleName() + "]");
           }
       }
   }
   ```

   

7. 第九步：需要通过`onRefresh`构造并启动Web服务器，先查找实现了`ServletWebServerFactory`这个接口的应用服务器Bean，体内默认的服务器是`Tomcat`，接下来通过`getWebServer`方法构造一个`Tomcat`对象，同时通过`start()`方法进行启动,这样Web服务器就可以开始运行![onRefresh](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-imagesonRefresh.png)

8. 第十步：通过`registerListeners()`方法，在Bean中查找所有的**监听器Bean**，将它们注册到，第八步构造的**消息广播器** `applicationEventMulticaster`中

   ```java
   protected void registerListeners() {
       // Register statically specified listeners first.
       for (ApplicationListener <? > listener: getApplicationListeners()) {
           getApplicationEventMulticaster().addApplicationListener(listener);
       }
   
       // Do not initialize FactoryBeans here: We need to leave all regular beans
       // uninitialized to let post-processors apply to them!
       String[] listenerBeanNames = getBeanNamesForType(ApplicationListener.class, true, false);
       for (String listenerBeanName: listenerBeanNames) {
           getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
       }
   
       // Publish early application events now that we finally have a multicaster...
       Set < ApplicationEvent > earlyEventsToProcess = this.earlyApplicationEvents;
       this.earlyApplicationEvents = null;
       if (!CollectionUtils.isEmpty(earlyEventsToProcess)) {
           for (ApplicationEvent earlyEvent: earlyEventsToProcess) {
               getApplicationEventMulticaster().multicastEvent(earlyEvent);
           }
       }
   }
   ```

9. 第十一步：这一步就是通过`finishBeanFactoryInitialization()`来生产所有的Bean，整体分为**构造对象、填充属性、初始化实例、注册销毁**四个步骤，可以参考**Bean生命周期**，Bean生成之后会放入**单例池** `singletonObjects`中

   ```java
   protected void finishBeanFactoryInitialization(ConfigurableListableBeanFactory beanFactory) {
       // Initialize conversion service for this context.
       if (beanFactory.containsBean(CONVERSION_SERVICE_BEAN_NAME) &&
           beanFactory.isTypeMatch(CONVERSION_SERVICE_BEAN_NAME, ConversionService.class)) {
           beanFactory.setConversionService(
               beanFactory.getBean(CONVERSION_SERVICE_BEAN_NAME, ConversionService.class));
       }
   
       // Register a default embedded value resolver if no BeanFactoryPostProcessor
       // (such as a PropertySourcesPlaceholderConfigurer bean) registered any before:
       // at this point, primarily for resolution in annotation attribute values.
       if (!beanFactory.hasEmbeddedValueResolver()) {
           beanFactory.addEmbeddedValueResolver(strVal - > getEnvironment().resolvePlaceholders(strVal));
       }
   
       // Initialize LoadTimeWeaverAware beans early to allow for registering their transformers early.
       String[] weaverAwareNames = beanFactory.getBeanNamesForType(LoadTimeWeaverAware.class, false, false);
       for (String weaverAwareName: weaverAwareNames) {
           getBean(weaverAwareName);
       }
   
       // Stop using the temporary ClassLoader for type matching.
       beanFactory.setTempClassLoader(null);
   
       // Allow for caching all bean definition metadata, not expecting further changes.
       beanFactory.freezeConfiguration();
   
       // Instantiate all remaining (non-lazy-init) singletons.
       beanFactory.preInstantiateSingletons();
   }
   ```

10. 第十二步：最后一步会通过`finishRefresh()`方法构造并注册**生命周期管理器** `lifecycleProcessor`，同时会调用所有实现了**生命周期接口** `Lifecycle`的Bean中的`start()`方法，在容器关闭时也会自动调用对应的`stop()`方法，接着发布一个**容器刷新完成** 的事件

    ```java
    protected void finishRefresh() {
        // Clear context-level resource caches (such as ASM metadata from scanning).
        clearResourceCaches();
    
        // Initialize lifecycle processor for this context.
        initLifecycleProcessor();
    
        // Propagate refresh to lifecycle processor first.
        getLifecycleProcessor().onRefresh();
    
        // Publish the final event.
        publishEvent(new ContextRefreshedEvent(this));
    }
    ```

    
