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

## 3.  解决循环依赖一定需要第二级缓存吗

**二级缓存存储的是尚未创建好的单例Bean对象，它的主要作用是用来保证这些尚未创建好的半成品对象是单例的**

综上所示：Spring解决循环依赖，一定是需要二级缓存的。

> 例：如果没有二级缓存。
>
> 1. 对象A开始创建，首先会进行实例化，实例化后会提前基于对象A实例化后的原始对象暴露一个Lambda表达式，并保存到三级缓存中，接着进行属性填充，尝试注入属性B；
> 2. 对象B开始创建，首先会进行实例化，接着进入属性填充阶段，尝试注入属性A；接下来会去获取A的Bean对象，首先会从一级缓存中检索，此时是检索不到，是因为对象A目前处于创建中；假设现在不考虑二级缓存；接着会直接取出并执行三级缓存中的Lambda表达式，并将返回该执行结果，注意：此时会将对象A的三级缓存清除，属性A 注入成功，接着对象B也会创建成功；
> 3. 对象A中属性B注入成功，接着尝试注入属性C；
> 4. 对象C 开始创建，同样的首先会进行实例化，接着进入属性填充阶段，尝试注入属性A；接下来会去获取A的Bean对象，首先会从一级缓存中检索，此时是检索不到的并且对象A还处于创建中；同样不考虑二级缓存；接着会直接取出并执行三级缓存的Lambda表达式，**因为对象A的三级缓存已经被清除，所以会报错，其实，即时第二步中Spring不会将对象A的三级缓存清除，此时依然有问题！因为此时通过三级缓存中的Lambda表达式执行结果的对象和第二步中属性B通过三级缓存中的Lambda表达式执行结果的对象很有可能不是同一个对象，但A对象又是单例，因此Spring解决循环依赖一定需要二级缓存。**

## 4. 解决循环依赖一定需要第三级缓存吗

Spring中的AOP是在Bean 生命周期的**初始化后阶段**,通过 `AnnotationAwareAspectJAutoProxyCreator` 后置处理器完成的，如果当前Bean对象需要进行AOP，那么当前Bean最终生成的对象将是一个**代理对象**，否则就是其原始对象。

如果当前Bean需要进行AOP并且**存在循环依赖**，那么当前Bean的AOP操作就需要**提前进行**！提前到Bean生命周期，**实例化之后和属性填充之前**！

> **Bean对象存不存在依赖循环，是无法预知的。**
>
> 方案一：**在实例化后，属性填充之前，不管存不存在循环依赖，都提前进行AOP并生成代理对象**。这种做法简单，粗暴，但和Bean的生命周期设计是相悖的，不可取。
>
> 方案二：**只有在出现循环依赖时，才提前进行AOP并生成代理对象。**那么，怎么判断当前Bean对象是否存在循环依赖，Spring中，是当前Bean对象创建过程中的属性填充阶段，根据其依赖对象是否存在创建中来判断！例：只有在对象B的创建过程中的属性填充阶段，才能判断出对象A, B是否出现了循环依赖，如果出现循环依赖，就需要需要进行，判断对象A是否需要进行AOP，如果需要进行AOP，则返回一个代理对象，不需要进行AOP 则返回一个对象A实例化后的原始对象。这些处理，仅依靠一个二级缓存是很难解决的！Spring 把者一系列逻辑操作以Lambda表达式的形式放入到了三级缓存中，而把这一段逻辑的执行结果存储到了二级缓存中，通过二级缓存和三级缓存的配合解决此问题。

鉴于上述方案，Spring解决循环依赖问题是一定需要三级缓存！否则就只能在实例化后，属性填充之前，不管是否存在循环依赖，都提前进行AOP并生成代理对象。而这种做法，与Bean生命周期的设计相悖，是不可取。

三级缓存中保存的`ObjectFactory` 是一个函数式接口，它是可以在任何想要获取对象的时候都可以通过获取并执行Lambda表达式来直接获取对象。通过这种方式，我们就可以在出现循环依赖时，通过获取并执行Lambda表达式，来判断注入对象是否需要进行AOP，如果需要，则返回一个代理对象；否则，则返回其实例化后的原始对象, Spring的三级缓存设计，只有在出现循环依赖的情况下才会打破Bean的生命周期的设计。

## 5. 什么场景下的循环依赖是Spring无法解决的

1. **原型（Prototype）作用域Bean的循环依赖**：Spring无法处理原型Bean的循环依赖，因为每个原型Bean都是独立的实例，Spring容器不会对它们进行缓存和复用，所以无法通过缓存来解决循环问题。

2. **构造器循环依赖**：当两个Bean通过构造器互相注入对方时，Spring无法解决这种循环依赖，因为构造器注入是必须在Bean实例化时就完成的，而此时另一个Bean尚未创建，导致无法注入。

3. **生成代理对象产生的循环依赖**：在某些情况下，如使用`@Async`注解，Spring会为Bean创建代理对象。如果这些代理对象之间存在循环依赖，Spring可能无法解决，因为代理对象的创建发生在Bean的初始化之后，循环依赖此时可能已经形成。

4. **使用`@DependsOn`注解产生的循环依赖**：如果两个Bean使用`@DependsOn`注解指定了彼此的依赖，这将导致循环依赖，因为Spring会按照注解指定的顺序来创建Bean，但当存在循环依赖时，这种顺序就无法满足。

5. **普通的AOP代理Bean的循环依赖**：对于通过AOP生成的代理Bean，如果存在循环依赖，Spring默认是可以解决的。但是，如果循环依赖涉及到特殊的AOP场景，如`@Async`增强的Bean，Spring可能无法解决。

6. **复杂的循环依赖**：在一些复杂的应用场景中，可能存在多个Bean通过不同的方式（如字段注入、setter注入等）相互依赖，形成复杂的循环依赖关系，这种情况下Spring可能无法解决所有问题。
