---
layout: post
title: Java 多线程
description: Java,多线程
author: MuZhi
date: 2024-6-11 17:45:35 +0800
categories: [Java,多线程]
tags: [Java,笔记,多线程]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/Java%E5%A4%9A%E7%BA%BF%E7%A8%8B.png
  alt: Java多线程
---

# Java 多线程

## 多线程概述

概述：它允许同时执行多个线程（即程序的执行路径），从而提高程序的效率和响应能力。Java提供了多线程的原生支持，主要通过`java.lang.Thread`类和`java.util.concurrent`包来实现。

### 并发和并行

- 并行：在同一时刻，多个指令在多个CPU上**同时**执行；
- 并发：在同一时刻，多个指令在单个CPU上**交替**执行；

### 进程和线程

- 进程：正在运行的程序
  - 独立性：进程是一个能独立运行的基本单位，同时也是系统分配资源和调度的独立单位；
  - 动态性：进程的实质是程序的一次执行过程，进程是动态产生，动态消亡；
  - 并发性：任何进程都可以同其他进程一起并发执行；
- 线程：是进程中的单个顺序控制流，是一条执行路径；（线程是进程中在作的事）
  - 单线程：一个进程只有一条执行路径，则为单线程；
  - 多线程：一个进程中有多条执行路径，则为多线程；

## 多线程的实现

### 继承Thread类实现

```java
public class MyThread extends Thread{
    @Override
    public void run() {
        System.out.println("MyThread启动~~~");
    }
}

public class MyThreadTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
}
```

- 为什么要重写`run()`方法

  因为`run()`是用来封装被线程执行的代码；

- `run()`方法和`start()`方法的区别

  `run()`方法仅仅是创建对象，用对象去调用方法，并没有开启线程；

  `start()`是开启一条线程；然后JVM调用此线程的`run()`方法；

  ```java
  public synchronized void start() {
      if (threadStatus != 0)
          throw new IllegalThreadStateException();
      group.add(this);
      boolean started = false;
      try {
          start0();
          started = true;
      } finally {
          try {
              if (!started) {
                  group.threadStartFailed(this);
              }
          } catch (Throwable ignore) {}
      }
  }
  // 与本地交互开启一条线程
  private native void start0();
  ```

### 实现Runnable接口实现

```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        System.out.println("MyRunnable启动~~~~");
    }
}

public class MyRunnableTest {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start();
    }
}
```

### 利用Callable和Future接口实现

```java
public class MyCallable implements Callable<Object> {
    @Override
    public Object call() throws Exception {
        // 返回值，线程运行完毕之后的结果
        return "Callable 启动~~~";
    }
}

public static void main(String[] args) {
    // 线程开启之后需要执行 call()方法
	MyCallable myCallable = new MyCallable();
    // 获取线程执行完毕后的结果，可以作为参数传递给Thread对象
	FutureTask<Object> futureTask = new FutureTask<Object>(myCallable);
	Thread thread = new Thread(futureTask);
	thread.start();
}
```

## 三种实现对比

|                            |                     优点                     |                    缺点                    |
| -------------------------- | :------------------------------------------: | :----------------------------------------: |
| 实现Runnable、Callable接口 | 扩展性强，实现该接口的同时，还可以继承其他类 | 编程相对复杂，不能直接使用Thread类中的方法 |
| 继承Thread类               |     编程简单，可以直接使用Thread类中方法     |        可扩展性差，不能在继承其他类        |

## 线程的常用方法

获取和设置线程名称

- `String getName()`：返回线程名称
- `String setName()`：设置线程名称

获取当前线程的对象

- `public static Thread currentThread()`：返回对当前正在执行的线程对象的引用

线程休眠

- `public static void sleep(long time)`：让线程休眠指定的时间，单位为毫秒

## 线程调度

多线程的并发运行：计算机中的CPU，在任意时刻只能执行一条指令，每个线程只有获取CPU的使用权才能执行代码，各个线程轮流获取CPU的使用权，分别执行各自任务

线程的优先级：

- 默认优先级: `5 `
- 最小优先级: `1`
- 最大优先级: `10`

设置线程优先级：`setPriority()`

获取线程优先级：`getPriority()`
