---
layout: post
title: Spring循环依赖
description: Spring循环依赖
author: MuZhi
date: 2024-5-9 21:43:56 +0800
categories: [Spring,循环依赖]
tags: [循环依赖,Spring,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/Spring%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96-logo.png
  alt: 循环依赖.
---


## 1. 什么是Spring循环依赖

> 循环依赖是多个Bean之间，相互持有对方的引用，比较典型的场景有三种：

1. 自我依赖

2. 相互依赖

3. 多个Bean之间的依赖。

   如：A依赖B，B依赖C，……，N依赖A。

   多个Bean直接相互持有对方的引用，从而无法完成Bean的创建。

## 2. Spring是如何解决循环依赖

### 2.1. Spring容器启动到Bean创建完成放入单例池流程图

![Spring容器创建Bean流程](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/Spring%E5%AE%B9%E5%99%A8%E5%88%9B%E5%BB%BABean%E6%B5%81%E7%A8%8B.png)

其中，标记为红色区域为Spring源码中解决循环依赖问题的两处关键。

1. `DefaultSingletonBeanRegistry # getSingleton(String beanName) `  当从 Spring 容器中获取 Bean 对象时，该方法首先会被调用：

   ```java
   protected Object getSingleton(String beanName, boolean allowEarlyReference) {
       // 1. 首先从一级缓存singletonObjects中获取Bean对象，如果存在则直接返回
       // 这是最快的路径，因为不需要进行同步锁定
       Object singletonObject = this.singletonObjects.get(beanName);
       
       // 2. 如果一级缓存中不存在该Bean对象，并且该Bean正在创建过程中
       // 则尝试从二级缓存earlySingletonObjects中获取Bean对象
       // 如果二级缓存中有，则直接返回这个早期的Bean对象
       if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
           singletonObject = this.earlySingletonObjects.get(beanName);
           
           // 3. 如果二级缓存中也不存在该Bean对象，并且调用者允许早期引用（allowEarlyReference为true）
           // 则尝试从三级缓存singletonFactories中获取ObjectFactory对象来创建Bean
           if (singletonObject == null && allowEarlyReference) {
               // 4. 先加锁，确保在多线程环境下的一致性和线程安全
               // 锁定的是singletonObjects，这是为了确保在等待其他线程创建Bean时，不会有其他线程进行干扰
               synchronized (this.singletonObjects) {
                   // 再次检查一级缓存singletonObjects，看是否有其他线程已经创建了该Bean
                   singletonObject = this.singletonObjects.get(beanName);
                   if (singletonObject == null) {
                       // 如果一级缓存中仍然没有，再次检查二级缓存earlySingletonObjects
                       singletonObject = this.earlySingletonObjects.get(beanName);
                       if (singletonObject == null) {
                           // 如果二级缓存中也没有，从三级缓存singletonFactories中获取ObjectFactory
                           ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
                           if (singletonFactory != null) {
                               // 使用ObjectFactory创建Bean对象
                               singletonObject = singletonFactory.getObject();
                               // 将新创建的Bean对象放入二级缓存earlySingletonObjects中
                               // 这样，其他线程在创建该Bean时就可以从二级缓存中获取到早期引用
                               this.earlySingletonObjects.put(beanName, singletonObject);
                               // 从三级缓存singletonFactories中移除对应的ObjectFactory
                               // 因为Bean对象已经创建完成，不再需要ObjectFactory
                               this.singletonFactories.remove(beanName);
                           }
                       }
                   }
               }
           }
       }
       // 返回获取到的单例对象，如果没有找到或创建Bean实例，则返回null
       return singletonObject;
   }
   ```

2. 在抽象类`AbstractAutowiredCapableBeanFactory # doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)`方法中：

  1. Spring 在创建Bean的实例化完成之后，会构建该 beanName 对应的 ObjectFactory 并放入到三级缓存中。因为 ObjectFactory 是一个函数式接口，支持Lambda 表达式，该Lambda 表达式的执行逻辑是：判断当前Bean 是否需要进行AOP，如果需要，则会执行AOP的逻辑并返回一个代理对象；否则就会把实例化好的原始Bean对象返回，即参数中的Bean对象。

     ```java
     boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences && isSingletonCurrentlyInCreation(beanName));
     // 判断当前Bean是否应该进行早期暴露：
     // 1. 检查Bean是否是单例（mbd.isSingleton()）
     // 2. 检查容器是否允许循环引用（this.allowCircularReferences）
     // 3. 检查Bean是否当前正在创建中（isSingletonCurrentlyInCreation(beanName)）
     // 如果上述所有条件都满足，则将earlySingletonExposure设置为true
     if (earlySingletonExposure) {
         // 如果决定进行早期暴露
         if (logger.isTraceEnabled()) {
             // 检查日志记录器是否启用了TRACE级别
             logger.trace("Eagerly caching bean '" + beanName +
                          "' to allow for resolving potential circular references");
             // 如果启用了TRACE级别日志，则记录一条日志信息
             // 说明正在缓存Bean 'beanName' 以解决潜在的循环引用问题
         }
         // 使用lambda表达式创建一个ObjectFactory，并将其添加到singletonFactories缓存中
         addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
         // 这里getEarlyBeanReference方法将返回一个代理对象，该对象能够用于早期引用，
         // 同时确保当实际的Bean创建完成时，所有对早期引用的调用都能被正确地转接到完全初始化的Bean上
     }
     ```

  2. 解决循环依赖流程图

     ![Spring循环依赖](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/Spring%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96.png)
