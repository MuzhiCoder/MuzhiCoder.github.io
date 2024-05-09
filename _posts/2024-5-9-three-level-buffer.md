---
layout: post
title: Spring三级缓存
description: Spring三级缓存是什么，它有什么作用，以及如何使用它。
author: MuZhi
date: 2024-5-9 15:20:56 +0800
categories: [Spring,三级缓存]
tags: [三级缓存,Spring,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/Spring%E4%B8%89%E7%BA%A7%E7%BC%93%E5%AD%98-logo.png
  alt: 三级缓存.
---

## 源码

```java
public class DefaultSingletonBeanRegistry extends SimpleAliasRegistry implements SingletonBeanRegistry {
     // 一级缓存：完全初始化的单例Bean存储
    Map<String, Object> singletonObjects = new ConcurrentHashMap<>();

    // 二级缓存：早期暴露的Bean引用存储
    Map<String, Object> earlySingletonObjects = new ConcurrentHashMap<>();

    // 三级缓存：正在创建中的Bean存储
    Map<String, ObjectFactory<?>> singletonFactories = new ConcurrentHashMap<>();   
}
```

## 一级缓存

**一级缓存（Singleton Cache）**：singletonObjects

- 也称为单例对象池。
- **存储完全初始化的单例Bean。**
- 一旦一个单例Bean被完全创建和初始化，它就会被存储在这里，以便于后续的请求可以直接提供该实例。

> 单例池并非专门用于解决 Spring 的循环依赖问题，即便不考虑循环依赖，经历了 Spring 容器完整创建过程的单例 Bean 对象也会被放入该单例池。只不过在 Spring 解决循环依赖问题的三级缓存中被称为一级缓存。其中，Bean 对象被存入单例池的源码是在**` DefaultSingletonBeanRegistry # getSingleton(String beanName)`**方法中，此外，该类中还有一个该方法的重载方法：**`getSingleton(String beanName, boolean allowEarlyReference)`**，它会首先从单例池中获取Bean对象。

## 二级缓存

**二级缓存（Early Singleton Cache）**：earlySingletonObjects

- **用于存放早期暴露的Bean引用，这些Bean尚未完全初始化。**
- 在Bean的初始化过程中，如果调用了`ObjectFactory.getObject()`或通过`@Autowired`注入到其他Bean的字段中，Spring会将这些尚未初始化的Bean提前暴露出来。
- 这个缓存允许Spring解决通过方法注入或字段注入产生的循环依赖。

> 二级缓存的存储结构和一级缓存完全一样，只不过存储的对象有所不同，一个是已经创建好的完整的Bean对象；另一个则是尚未创建好的单例Bean对象，或者叫半成品对象。
>
> 在创建单例Bean时，如果发现该Bean存在循环依赖，则会提前把这个尚未完全创建好的半成品Bean对象放入到二级缓存中。如果该Bean需要进行AOP，则该半成品对象就是它的代理对象，否则就是它实例化之后但是尚未属性填充的原始对象。
>
> **二级缓存的主要作用是用来保证这些尚未完全创建好的半成品对象是单例的**

## 三级缓存

**三级缓存（Singleton Objects Cache）**：singletonFactories

- **用于存放正在创建中的单例Bean。（尚未完全创建好的单例Bean对象）**
- 当一个Bean被标记为正在创建中，它的早期引用（可能未完全初始化）可以被其他Bean使用，以解决构造器中的循环依赖问题。
- 三级缓存通常与`ObjectFactory`和`ApplicationListener`等高级特性一起使用。

> 三级缓存是一个`HashMap`它的Key是String类型，保存的是`beanName`；value是 `ObjectFactory`类型，保存的是尚未创建好的单例Bean。
>
> ObjectFactory：是Spring提供的函数式接口，可以使用Lambda表达式实现。
>
> 在创建单例Bean的实例化后阶段，如果允许循环依赖，就会提前基于当前 Bean 的原始对象暴露一个 Lambda表达式，并保存到三级缓存中。
>
> ```java
> boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&
> 				isSingletonCurrentlyInCreation(beanName));
> 		if (earlySingletonExposure) {
> 			if (logger.isTraceEnabled()) {
> 				logger.trace("Eagerly caching bean '" + beanName +
> 						"' to allow for resolving potential circular references");
> 			}
> 			addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
> 		}
> ```
>
> 这个Lambda表达式，主要取决于当前 Bean 是否存在循环依赖，来判断是否使用；Bean会按照自己生命周期正常执行，直到创建完成后放入单例池中，反之，则会从三级缓存中取出Lambda表达式并执行，执行返回的对象会被放入到二级缓存。

如果，不考虑AOP，仅两级缓存就可以解决循环依赖的问题，但是，正是因为当前 Bean对象可能进行AOP，我们就只能在出现循环依赖的时候，执行一段逻辑：判断当前 Bean 对象是否需要进行AOP，如果需要，则会进行AOP并返回一个当前Bean原始对象的代理对象；否则就会直接返回当前 Bean的原始对象，这段逻辑正式Lambda 表达式的执行逻辑。此时，通过二级和三级缓存的配合，优雅解决循环依赖问题。

**三级缓存最主要的作用就是为了打破循环依赖**

Spring解决循环依赖的过程中，除了三级缓存，还有两个缓存也是同样重要：

1. `earlyProxyReferences` 是一个Map,记录了某个 Bean 的原始对象是否已经进行了AOP。

   ```java
   private final Map<Object, Object> earlyProxyReferences = new ConcurrentHashMap<>(16);
   ```

2. `singletonsCurrentlylnCreation` 是一个Set，记录了当前正在创建Bean的集合，使用它可以用来判断当前Bean对象是否存在循环依赖。

   ```java
   private final Set<String> singletonsCurrentlyInCreation = 
   		Collections.newSetFromMap(new ConcurrentHashMap<>(16));
   ```

   
