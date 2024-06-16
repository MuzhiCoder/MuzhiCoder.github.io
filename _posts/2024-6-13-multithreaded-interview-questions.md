---
layout: post
title: 多线程 面试题
description: 多线程，面试
author: MuZhi
date: 2024-6-11 09:47:01 +0800
categories: [多线程,面试题]
tags: [面试题,多线程]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/%E5%A4%9A%E7%BA%BF%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.png
  alt: Spring Boot.
---

# 多线程 面试题

## 线程有那些状态

> Java 多线程状态分为六种

![多线程状态-1](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/多线程状态-1.png)

> 操作系统层面有五种状态

![。CPU线程状态png](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/。CPU线程状态png.png)

## 线程池的核心参数

> `ThreadPoolExecutor` 构造函数参数

![线程核心参数](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/线程核心参数.png)

- **`corePoolSize`**: 核心线程数目
  - 最多保留的线程数
- **`maximumPoolSize`**: 最大线程数目
  - 核心线程 + 救急线程
- **`keepAliveTime`**: 生存时间
  - 针对救急线程
- **`unit`**: `keepAliveTime` 参数的时间单位。
- **`workQueue`**: 一个阻塞队列，用于存储等待执行的任务。
- `unit`：时间单位
  - 针对救急线程
- `workQueue`
  - 阻塞队列
- `threadFactory`：线程工厂
  - 为线程在创建时，创建名称
- **`handler`**: 拒绝策略
  - 四种

## sleep 和 wait 方法区别

1. **来源**：
   - `sleep` 方法定义在 `java.lang.Thread` 类中。
   - `wait` 方法定义在 `java.lang.Object` 类中，因此它是所有 Java 对象的成员方法。
2. **目的**：
   - `sleep` 用于使当前正在执行的线程暂停执行指定的时间，让出 CPU 给其他线程，但不释放对象锁。
   - `wait` 用于在其他线程调用同一个对象的 `notify()` 或 `notifyAll()` 方法之前，使当前线程暂停执行，并且释放对象锁。
3. **中断响应**：
   - `sleep` 方法在指定时间结束后，线程继续执行，不会抛出 `InterruptedException`。
   - `wait` 方法可以在等待过程中被中断，如果当前线程在 `wait` 期间被中断，它会抛出 `InterruptedException`。
4. **锁**：
   - 在调用 `sleep` 方法期间，当前线程不会释放任何锁。
   - 在调用 `wait` 方法时，当前线程必须拥有对象的锁，并且会释放这个锁，进入等待状态，直到其他线程调用 `notify()` 或 `notifyAll()`。
5. **使用场景**：
   - `sleep` 通常用于控制程序的执行时间间隔，例如在循环中暂停执行一段时间。
   - `wait` 通常用于线程间的协调，特别是在生产者-消费者模型中，消费者可能需要等待生产者生产出产品。
6. **返回值**：
   - `sleep` 方法没有返回值，它接受一个表示时间的参数。
   - `wait` 方法没有参数，但它可以在调用时指定一个超时时间，并且可以在等待过程中被中断。

总结：`sleep` 主要用于简单的时间延迟，而 `wait` 是用于线程间的同步，它涉及到更复杂的线程通信和锁的释放与获取。在使用 `wait` 方法时，通常需要与 `synchronized` 块一起使用，以确保线程安全。

## Lock 和 synchronronized

> `Lock` 和 `synchronized` 都是 Java 并发编程中用于线程同步的机制，但它们在设计和使用上存在一些显著的区别
>
> - 语法方面
>   - `synchronronized` 是关键字，源码在 JVM 中，用 C++ 语言实现
>   - `Lock` 是接口，源码由 JDK 提供，同 Java 语法实现
>   - 使用`synchronronized` 时，退出同步代码块锁会自动释放，而使用`Lock`时，需要手动调用`unlock()`方法释放锁
> - 功能方面
>   - 二者均属于**悲观锁**，都具备基本的互斥、同步、锁重入功能
>   - `Lock` 提供了许多`synchronronized`不具备的功能，例：获取等待状态、公平锁、可打断、可超时、多条件变量
>   - `Lock` 适合不不同场景实现、如：` ReentrantLock`、` ReentrantReadWriteLock`
> - 性能方面
>   - 在没有竞争时，`synchronronized`做了很多优化，如：偏向锁、轻量级锁、性能不赖
>   - 在竞争激烈，`Lock` 的实现通常会提供更好的性能

以下是 `Lock` 和 `synchronized` 的详细对比：

1. 使用方式

- **synchronized**:
  - 可以用于修饰方法或代码块。
  - 使用简单，只需在方法或代码块前加上 `synchronized` 关键字。

- **Lock**:
  - 是 `java.util.concurrent.locks` 包中的一个接口。
  - 使用时需要先实例化一个锁对象，然后调用 `lock()` 方法获取锁，`unlock()` 方法释放锁。

2. 锁的获取方式

- **synchronized**:
  - 进入同步代码块或方法时，自动获取锁。
  - 离开同步代码块或方法时，自动释放锁。

- **Lock**:
  - 需要显式调用 `lock()` 方法获取锁。
  - 需要显式调用 `unlock()` 方法释放锁。

3. 响应中断

- **synchronized**:
  - 锁的获取过程中，如果线程被中断，不会抛出 `InterruptedException`。

- **Lock**:
  - 可以使用 `lockInterruptibly()` 方法，该方法可以在线程被中断时立即响应，并抛出 `InterruptedException`。

4. 尝试非阻塞获取锁

- **synchronized**:
  - 不支持尝试非阻塞获取锁。

- **Lock**:
  - 支持通过 `tryLock()` 方法尝试非阻塞获取锁。

5. 超时获取锁

- **synchronized**:
  - 不支持超时获取锁。

- **Lock**:
  - 支持通过 `tryLock(long timeout, TimeUnit unit)` 方法在指定时间内尝试获取锁。

6. 可重入性

- **synchronized** 和 **Lock** 都支持可重入性，即同一个线程可以多次获取同一个锁。

7. 公平性（Fairness）

- **synchronized**:
  - 不支持设置公平性，锁的获取顺序不保证公平。

- **Lock**:
  - 例如 `ReentrantLock` 可以设置公平性（`true` 或 `false`），公平性锁可以按照线程等待的顺序来分配锁。

8. 条件变量

- **synchronized**:
  - 使用 `wait()`, `notify()`, `notifyAll()` 方法实现条件变量。

- **Lock**:
  - 提供了更丰富的条件变量支持，通过 `newCondition()` 方法创建 `Condition` 对象，可以有更复杂的线程间协调。

9. 锁状态检查

- **synchronized**:
  - 没有提供检查锁状态的方法。

- **Lock**:
  - 提供了 `isLocked()`, `isHeldByCurrentThread()`, `hasQueuedThreads()` 等方法，可以检查锁的状态。

10. 示例代码

- **synchronized 示例**:
  ```java
  public class Counter {
      private int count = 0;
  
      public void increment() {
          synchronized (this) {
              count++;
          }
      }
  }
  ```

- **Lock 示例**:
  ```java
  import java.util.concurrent.locks.Lock;
  import java.util.concurrent.locks.ReentrantLock;
  
  public class Counter {
      private final Lock lock = new ReentrantLock();
      private int count = 0;
  
      public void increment() {
          lock.lock();
          try {
              count++;
          } finally {
              lock.unlock();
          }
      }
  }
  ```

总结

`Lock` 提供了比 `synchronized` 更丰富的控制能力和灵活性，特别是在处理复杂同步场景时。然而，`synchronized` 在简单场景下更加简洁和方便。选择使用哪种机制取决于具体的应用场景和性能要求。

## 公平锁和非公平锁

> 公平锁（Fair Lock）和非公平锁（Unfair Lock）是指锁的获取方式是否考虑了获取锁的顺序。在 Java 的 `ReentrantLock` 类中，可以通过构造函数设置锁的公平性。

以下是公平锁和非公平锁的详细对比：

**公平锁（Fair Lock）**

1. **定义**：公平锁是指多个线程按照请求锁的顺序去获取锁。线程获取锁的顺序是公平的，即先到的线程先获得锁。
2. **优点**：
   - 避免饥饿现象，即线程长时间无法获取到锁。
   - 适用于需要确保任务按顺序执行的场景。
3. **缺点**：
   - 吞吐量可能较低，因为需要维护一个队列来保证公平性。
   - 可能导致线程频繁的上下文切换，影响性能。
4. **实现**：在 `ReentrantLock` 的构造函数中设置 `true` 来创建公平锁。
   ```java
   Lock fairLock = new ReentrantLock(true);
   ```

**非公平锁（Unfair Lock）**

1. **定义**：非公平锁是指在获取锁时不考虑线程请求的顺序。线程可能随时抢占锁，不论其他线程等待的时间长短。
2. **优点**：
   - 吞吐量可能较高，因为省去了维护等待队列的开销。
   - 性能可能更好，因为减少了线程调度和上下文切换。
3. **缺点**：
   - 可能导致线程饥饿，即某些线程长时间无法获取到锁。
   - 可能导致线程饥饿现象，尤其是在高负载的情况下。
4. **实现**：在 `ReentrantLock` 的构造函数中设置 `false` 或者不设置（默认为 `false`）来创建非公平锁。
   ```java
   Lock unfairLock = new ReentrantLock(); // 默认非公平锁
   // 或者
   Lock unfairLock = new ReentrantLock(false);
   ```

**选择公平锁还是非公平锁：**

- 如果你的应用程序中线程需要按照请求顺序公平地访问资源，那么公平锁是一个好选择。
- 如果性能是关键考虑因素，并且锁竞争不激烈，或者你可以接受某些线程可能会饿死的风险，那么非公平锁可能更合适。

**示例代码**

以下是使用 `ReentrantLock` 创建公平锁和非公平锁的示例：

```java
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    public static void main(String[] args) {
        // 创建公平锁
        Lock fairLock = new ReentrantLock(true);
        // 创建非公平锁
        Lock unfairLock = new ReentrantLock();
        
        // 使用公平锁
        fairLock.lock();
        try {
            // 访问共享资源
        } finally {
            fairLock.unlock();
        }

        // 使用非公平锁
        unfairLock.lock();
        try {
            // 访问共享资源
        } finally {
            unfairLock.unlock();
        }
    }
}
```

在实际应用中，选择哪种类型的锁取决于具体的应用场景和性能要求。公平锁提供了更好的公平性保证，而非公平锁则可能提供更高的吞吐量。

## Lock条件变量

> 在 Java 中，条件变量允许线程在某些条件不满足时挂起（等待），并在条件变为满足时被唤醒。`Lock` 接口提供了条件变量的支持，通过 `Condition` 接口实现

以下是使用 `Lock` 和条件变量的详解：

**条件变量（Condition）**

- **定义**：条件变量是一种同步辅助工具，用于线程间的协调，允许一个或多个线程等待某个条件变为真，而其他线程在适当的时候发出信号通知等待的线程。

使用条件变量的步骤：

1. **获取锁**：在操作条件变量之前，必须先获取关联的 `Lock`。
2. **等待条件**：使用 `Condition` 的 `await()` 方法使当前线程等待，直到它被其他线程通过 `signal()` 或 `signalAll()` 唤醒。
3. **检查条件**：在 `await()` 方法返回后，再次检查条件是否满足，因为 `await()` 方法可能因为 `InterruptedException` 或其他原因而返回。
4. **释放锁**：在等待条件变量之前，确保释放锁，以便其他线程可以进入并可能改变条件状态。在 `await()` 方法调用后，当前线程会自动重新获取锁。

示例：

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ConditionVariableExample {
    private int signalCount = 0;
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();

    public void waitForSignal() throws InterruptedException {
        lock.lock();
        try {
            while (signalCount < 1) { // 等待条件满足
                condition.await();
            }
            // 处理信号
        } finally {
            lock.unlock();
        }
    }

    public void sendSignal() {
        lock.lock();
        try {
            signalCount++; // 更改条件
            condition.signalAll(); // 唤醒所有等待的线程
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        ConditionVariableExample example = new ConditionVariableExample();
        Thread waitingThread = new Thread(() -> example.waitForSignal());
        waitingThread.start();

        // 给等待线程发送信号
        Thread.sleep(1000); // 模拟一些工作
        example.sendSignal();

        // 等待等待线程完成
        waitingThread.join();
    }
}
```

**条件变量与 `wait()` 和 `notify()`**

- `wait()` 和 `notify()` 方法是 `Object` 类的成员，它们也用于线程间的协调。但是，它们与 `synchronized` 关键字一起使用，并且没有提供 `Lock` 那样的灵活性。
- 使用 `wait()` 时，线程必须先获得对象的 `synchronized` 锁，然后调用 `wait()` 来等待。线程会在释放锁后进入等待状态，并在接收到 `notify()` 或 `notifyAll()` 调用时被唤醒。
- `Condition` 接口提供了比 `wait()` 和 `notify()` 更丰富的功能，例如能够创建多个条件变量，并对它们分别进行 `await()` 和 `signal()` 操作。

**注意事项：**

- 使用条件变量时，要避免进入无限等待状态。在 `await()` 返回之后，应该总是重新检查条件是否满足。
- 确保在 `await()` 之前释放锁，并在 `await()` 之后重新获取锁，以避免死锁。
- 使用 `signal()` 唤醒单个等待线程，而 `signalAll()` 唤醒所有等待的线程。

条件变量提供了一种强大的方式来同步线程，使得线程可以根据特定的条件进行等待和唤醒，从而实现复杂的同步逻辑。

## volatile 能否保证线程安全

1. 线程安全要考虑三个方面：可见性、有序性、原子性

   1. **可见性（Visibility）：**

      可见性是指当多个线程访问同一个变量时，一个线程对变量的修改对其他线程是可见的。

      `volatile` 关键字可以确保变量的可见性。当一个线程修改了一个 `volatile` 变量时，新值会立即同步到主内存中，其他线程再次读取该变量时会从主内存中读取新值。

   2. **有序性（Ordering）：**

      有序性是指程序执行的顺序按照代码的先后顺序进行。在多线程环境中，由于编译器优化和处理器优化，指令重排可能导致代码执行顺序与编写顺序不同。

      `volatile` 变量的写操作在读取操作之前不会发生指令重排，从而确保了有序性。

   3. **原子性（Atomicity）：**

      原子性是指一个操作或者一系列操作要么全部执行，要么全部不执行，中间不会穿插其他线程的操作。

      基本数据类型的访问和操作（如 int、long、boolean 等）通常具有原子性，但是复合操作（如递增 i++ 或 ++i）不是原子的。Java 提供了 synchronized 和 java.util.concurrent 包中的原子类（如 AtomicInteger）来确保复合操作的原子性。

### 线程安全的实现方法：

- **使用 `volatile`**：适用于只读操作或对单个变量的写入操作，确保变量的可见性和有序性，但不保证复合操作的原子性。
- **使用 `synchronized`**：确保同一时刻只有一个线程可以访问被 `synchronized` 修饰的代码块或方法，从而保证原子性、可见性和有序性。
- **使用 `Lock` 接口**：提供了比 `synchronized` 更丰富的锁操作，可以设置尝试非阻塞获取锁、超时获取锁等，同样可以保证原子性、可见性和有序性。
- **使用原子类**：如 `AtomicInteger`、`AtomicLong` 等，它们利用 CAS（Compare-And-Swap）操作来保证复合操作的原子性。
- **使用线程局部变量**：每个线程有自己的变量副本，不需要与其他线程共享，从而避免线程安全问题。
- **使用不可变对象**：不可变对象的状态在创建后不能被修改，因此天然是线程安全的。