---
layout: post
title: Spring AOP
description: Spring AOP,AOP
author: MuZhi
date: 2024-6-11 09:47:01 +0800
categories: [Spring,AOP]
tags: [Spring,笔记,AOP]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/spring%20aop.png
  alt: Spring Boot.
---

# Spring AOP 原理

## 切面类

1. 只需要新建一个类，为它增加AOP切面注解`@Aspect`就可以使用，在里面定义一些**增强方法**，然后通过`@Before、@After、@AfterReturning、@AfterThrowing、@Round`注解来指定何时执行方法的增强，在方法之前，方法之后、方法正常返回后、在方法异常返回后、对方法进行包裹；在这5个注解中使用`execution`表达式进行增强范围的限定，可以精确到某些类的某些方法。

   > **@Before**: 这个注解用于在目标方法执行之前执行特定的代码。它可以用来进行前置处理，比如日志记录、权限检查等。
   >
   > **@After**: 这个注解用于在目标方法执行之后执行代码，无论目标方法是否成功执行或抛出异常。它可以用来进行清理工作，比如关闭数据库连接、日志记录等。
   >
   > **@AfterReturning**: 这个注解用于在目标方法成功执行并返回结果后执行代码。它可以用来进行后置处理，比如记录返回值、执行一些额外的逻辑等。
   >
   > **@AfterThrowing**: 这个注解用于在目标方法抛出异常后执行代码。它可以用来处理异常，比如记录错误信息、发送错误通知等。
   >
   > **@Around**: 这个注解用于在目标方法执行前后都执行代码，并且可以控制目标方法的执行。它可以用来进行性能监控、事务管理等。

   execution 表达式的基本语法如下：

   > ```
   > execution(<modifier-pattern>? <return-type-pattern> <method-pattern>(<parameter-pattern>) <throws-pattern>?)
   > ```
   >
   > `<modifier-pattern>`: 可选的修饰符模式，比如 `public`, `protected` 等。
   >
   > `<return-type-pattern>`: 返回类型模式，可以是 `*` 表示任意返回类型，或者具体的类型名。
   >
   > `<method-pattern>`: 方法名模式，可以包含通配符如 `*` 或 `+`。
   >
   > `<parameter-pattern>`: 参数模式，可以是具体的参数类型，`..` 表示任意数量的参数，或者 `*` 表示任意类型的参数。
   >
   > `<throws-pattern>`: 可选的抛出异常模式，用于匹配方法可能抛出的异常。

## AOP 具体如何运行

在启动时，会创建IOC容器，同时将Bean进行三个连续的动作：构造、填充属性、初始化；

**AOP功能** 就是通过一个专门处理AOP的：**Bean后置处理器** `DefaultAdvisorAutoProxyCreator`进行方法增强，所有的后置处理器都在Bean构建完并且填充了属性之后执行，当然也包括我们的AOP后置处理器，在每一个Bean初始化之后都会调用这个后置处理器的`postProcessAfterInitialization()`方法，

```java
public Object postProcessAfterInitialization(@Nullable Object bean, String beanName) {
    if (bean != null) {
        Object cacheKey = getCacheKey(bean.getClass(), beanName);
        if (this.earlyProxyReferences.remove(cacheKey) != bean) {
            return wrapIfNecessary(bean, beanName, cacheKey);
        }
    }
    return bean;
}
```

在这个方法中会需要使用AOP的Bean创建代理对象，先通过`getAdvicesAndAdvisorsForBean()`获取所有的增强`Advice` 同时判断当前Bean是否满足配置的切面条件，如果满足条件，就会为这个Bean构建**代理对象**来实现AOP，为更统一、更方便的构造代理对象，先搭建一个专门用来构造生成代理对象的工厂`proxyFactory`，

```java
protected Object wrapIfNecessary(Object bean, String beanName, Object cacheKey) {
    if (StringUtils.hasLength(beanName) && this.targetSourcedBeans.contains(beanName)) {
        return bean;
    }
    if (Boolean.FALSE.equals(this.advisedBeans.get(cacheKey))) {
        return bean;
    }
    if (isInfrastructureClass(bean.getClass()) || shouldSkip(bean.getClass(), beanName)) {
        this.advisedBeans.put(cacheKey, Boolean.FALSE);
        return bean;
    }

    // Create proxy if we have advice.
    Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(bean.getClass(), beanName, null);
    if (specificInterceptors != DO_NOT_PROXY) {
        this.advisedBeans.put(cacheKey, Boolean.TRUE);
        // 构建代理对象，实现AOP
        Object proxy = createProxy(
            bean.getClass(), beanName, specificInterceptors, new SingletonTargetSource(bean));
        this.proxyTypes.put(cacheKey, proxy.getClass());
        return proxy;
    }

    this.advisedBeans.put(cacheKey, Boolean.FALSE);
    return bean;
}
```

然后会告诉这个工厂，具体选择哪种方式进行代理，分别是`Cglib`和`JdkProxy`

```java
protected Object createProxy(Object target, List < Advisor > advisors, Class <? > ...interfaces) {
    ProxyFactory pf = new ProxyFactory(target);
    if (interfaces.length > 1 || interfaces[0].isInterface()) {
        pf.setInterfaces(interfaces);
    } else {
        pf.setProxyTargetClass(true);
    }

    // Required everywhere we use AspectJ proxies
    pf.addAdvice(ExposeInvocationInterceptor.INSTANCE);
    pf.addAdvisors(advisors);

    pf.setExposeProxy(true);
    return pf.getProxy();
}
```

以通过添加`@EnableAspectJAutoproxy`注解，并将，其`proxyTargetClass`配置改为`true`，来强制使用`Cglib`，当配置为`false`才使用`JdkProxy` ，这时就会构建`JdkDynamicAopProxy`或者`CglibAopProxy` ,然后就可以通过`getProxy()`方法获得真正的代理对象， 

在`getProxy` 中会构建一个实现同样Bean接口的代理对象，将真实Bean作为代理对象中的一个成员变量，

![JdkProxy](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/JdkProxy.png)

在调用Bean方法的时候，就会执行代理对象中的`invoke()`方法，主要进行两步操作：

```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    Object oldProxy = null;
    boolean setProxyContext = false;

    TargetSource targetSource = this.advised.targetSource;
    Object target = null;

    try {
        if (!this.equalsDefined && AopUtils.isEqualsMethod(method)) {
            // The target does not implement the equals(Object) method itself.
            return equals(args[0]);
        } else if (!this.hashCodeDefined && AopUtils.isHashCodeMethod(method)) {
            // The target does not implement the hashCode() method itself.
            return hashCode();
        } else if (method.getDeclaringClass() == DecoratingProxy.class) {
            // There is only getDecoratedClass() declared -> dispatch to proxy config.
            return AopProxyUtils.ultimateTargetClass(this.advised);
        } else if (!this.advised.opaque && method.getDeclaringClass().isInterface() &&
            method.getDeclaringClass().isAssignableFrom(Advised.class)) {
            // Service invocations on ProxyConfig with the proxy config...
            return AopUtils.invokeJoinpointUsingReflection(this.advised, method, args);
        }

        Object retVal;

        if (this.advised.exposeProxy) {
            // Make invocation available if necessary.
            oldProxy = AopContext.setCurrentProxy(proxy);
            setProxyContext = true;
        }

        // Get as late as possible to minimize the time we "own" the target,
        // in case it comes from a pool.
        target = targetSource.getTarget();
        Class <? > targetClass = (target != null ? target.getClass() : null);

        // Get the interception chain for this method.
        List < Object > chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);

        // Check whether we have any advice. If we don't, we can fall back on direct
        // reflective invocation of the target, and avoid creating a MethodInvocation.
        if (chain.isEmpty()) {
            // We can skip creating a MethodInvocation: just invoke the target directly
            // Note that the final invoker must be an InvokerInterceptor so we know it does
            // nothing but a reflective operation on the target, and no hot swapping or fancy proxying.
            Object[] argsToUse = AopProxyUtils.adaptArgumentsIfNecessary(method, args);
            retVal = AopUtils.invokeJoinpointUsingReflection(target, method, argsToUse);
        } else {
            // We need to create a method invocation...
            MethodInvocation invocation =
                new ReflectiveMethodInvocation(proxy, target, method, args, targetClass, chain);
            // Proceed to the joinpoint through the interceptor chain.
            retVal = invocation.proceed();
        }

        // Massage return value if necessary.
        Class <? > returnType = method.getReturnType();
        if (retVal != null && retVal == target &&
            returnType != Object.class && returnType.isInstance(proxy) &&
            !RawTargetAccess.class.isAssignableFrom(method.getDeclaringClass())) {
            // Special case: it returned "this" and the return type of the method
            // is type-compatible. Note that we can't help if the target sets
            // a reference to itself in another returned object.
            retVal = proxy;
        } else if (retVal == null && returnType != Void.TYPE && returnType.isPrimitive()) {
            throw new AopInvocationException(
                "Null return value from advice does not match primitive return type for: " + method);
        }
        return retVal;
    } finally {
        if (target != null && !targetSource.isStatic()) {
            // Must have come from TargetSource.
            targetSource.releaseTarget(target);
        }
        if (setProxyContext) {
            // Restore old proxy.
            AopContext.setCurrentProxy(oldProxy);
        }
    }
}
```

第一步：通过之前提到的**execution表达式** 获取所有与该方法匹配的所有**增强方法**，并组成调用链同时进行排序

```java
List < Object > chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
```

第二步：就是开始按顺序执行这些调用链，这里的调用方式就是经典的**责任链模式**，在调用中间会插入并执行Bean真实的方式

```java
MethodInvocation invocation =
                new ReflectiveMethodInvocation(proxy, target, method, args, targetClass, chain);
// Proceed to the joinpoint through the interceptor chain.
retVal = invocation.proceed();
```

接下来就是`CglibAopProxy`同样会在`getProxy()`方法中构造代理对象，用增强器**Enhancer**来设置代理基本信息以及增强方法的调用链，接着执行`Enhancer create()`方法来生成代理对象和`JdkDynamicAopProxy`不同的是`Cglib`是基于`Jdk rk` jar包中的ASM来生成一组新的.class文件，然后实例化对象，所以对于没有实现接口的Bean也可以生成代理对象，

![Cglib](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/Cglib.png)

在调用Bean方法的时候，会先执行代理对象的`intercept()`方法与`JdkProxy`一样也会通过责任链来执行所有的方法增强

```java
public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
    Object oldProxy = null;
    boolean setProxyContext = false;
    Object target = null;
    TargetSource targetSource = this.advised.getTargetSource();
    try {
        if (this.advised.exposeProxy) {
            // Make invocation available if necessary.
            oldProxy = AopContext.setCurrentProxy(proxy);
            setProxyContext = true;
        }
        // Get as late as possible to minimize the time we "own" the target, in case it comes from a pool...
        target = targetSource.getTarget();
        Class <? > targetClass = (target != null ? target.getClass() : null);
        List < Object > chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
        Object retVal;
        // Check whether we only have one InvokerInterceptor: that is,
        // no real advice, but just reflective invocation of the target.
        if (chain.isEmpty()) {
            // We can skip creating a MethodInvocation: just invoke the target directly.
            // Note that the final invoker must be an InvokerInterceptor, so we know
            // it does nothing but a reflective operation on the target, and no hot
            // swapping or fancy proxying.
            Object[] argsToUse = AopProxyUtils.adaptArgumentsIfNecessary(method, args);
            retVal = AopUtils.invokeJoinpointUsingReflection(target, method, argsToUse);
        } else {
            // We need to create a method invocation...
            retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();
        }
        retVal = processReturnType(proxy, target, method, retVal);
        return retVal;
    } finally {
        if (target != null && !targetSource.isStatic()) {
            targetSource.releaseTarget(target);
        }
        if (setProxyContext) {
            // Restore old proxy.
            AopContext.setCurrentProxy(oldProxy);
        }
    }
}
```



```java
List < Object > chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);

retVal = AopUtils.invokeJoinpointUsingReflection(target, method, argsToUse);
```

