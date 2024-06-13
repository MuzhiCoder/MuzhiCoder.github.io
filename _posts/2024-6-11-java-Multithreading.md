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

**线程优先级高，更有几率优先执行，并不是一定优先执行**

## 后台线程/守护线程

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        // 创建一个线程
        Thread thread = new Thread(new DaemonTask());

        // 将线程设置为守护线程
        thread.setDaemon(true);

        // 启动线程
        thread.start();

        // 主线程睡眠一段时间，模拟执行一些任务
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 主线程结束，守护线程也会随之结束
        System.out.println("Main thread is ending.");
    }
}

class DaemonTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Daemon thread is running.");
        // 执行一些后台任务
    }
}
```

`DaemonTask` 是一个实现了 `Runnable` 接口的类，它将被作为守护线程执行。当主线程（main thread）结束时，守护线程也会结束，即使它可能还在执行中。

守护线程不应该执行关键任务，因为它们可能会在任何时候被终止。此外，守护线程不应该依赖于 JVM 的关闭来释放资源，因为 JVM 的关闭可能不会等待守护线程完成。

## 线程的安全问题

1. **数据不一致性**：当多个线程同时读写同一资源，未采取适当的同步措施时，可能导致数据的不一致性。

2. **竞态条件（Race Condition）**：多个线程对共享资源的访问顺序影响程序的输出，导致不确定的行为。

3. **死锁（Deadlock）**：两个或多个线程在等待对方释放资源，导致它们都无法继续执行。

4. **活锁（Livelock）**：线程不断尝试执行操作但由于某些条件未满足而不断回退，导致线程无法取得进展。

5. **资源耗尽**：线程可能因为等待资源而长时间阻塞，导致资源无法被释放，最终耗尽系统资源。

6. **原子性问题**：一个复合操作需要多个步骤完成，如果这些步骤没有被原子性地执行，就可能出现中间状态的数据。

7. **可见性问题**：一个线程对共享变量的修改，其他线程不能立即看到这些更改。

8. **顺序性问题**：指令重排可能导致多线程环境下的执行顺序与预期不一致。

为了解决这些线程安全问题，可以采用以下一些策略：

- **同步**：使用`synchronized`关键字或显式的锁（如`ReentrantLock`）来确保共享资源在同一时间只被一个线程访问。
- **原子变量**：使用`java.util.concurrent.atomic`包中的原子类，如`AtomicInteger`，来保证变量的原子操作。
- **线程局部变量**：使用`ThreadLocal`类为每个线程创建变量的副本，避免共享状态。
- **不可变对象**：设计对象为不可变，确保对象一旦创建后状态就不能改变，从而避免并发问题。
- **并发集合**：使用线程安全的集合类，如`ConcurrentHashMap`，来管理共享数据。
- **条件变量**：使用`wait()`、`notify()`和`notifyAll()`方法来控制线程间的协调。
- **锁条（Lock Striping）**：将一个大的锁分解成多个小的锁，以提高并发性。
- **乐观锁**：通过版本号或时间戳来确保在读取数据后到更新数据这段时间内数据没有被其他线程修改。
- **避免锁**：尽可能设计无锁的并发算法，例如使用`CopyOnWriteArrayList`等。

### 同步代码块

#### synchronized

`synchronized` 是Java中的一个关键字，用于实现线程同步，确保多个线程在访问共享资源时能够以互斥的方式进行，从而避免多线程环境下的竞态条件和数据不一致问题。以下是 `synchronized` 的一些关键点：

1. **互斥锁**：`synchronized` 可以确保同一时刻只有一个线程可以执行特定代码段。

2. **可重入性**：如果一个线程已经拥有某个对象的锁，它可以再次请求同一个对象上的锁，而不会被阻塞。

3. **锁的获取和释放**：
   - 当一个线程访问一个对象的 `synchronized` 方法或代码块时，它会自动获取该对象的锁。
   - 当 `synchronized` 代码块执行完毕后，线程会自动释放锁，使得其他线程可以获取该锁。

4. **使用方式**：
   - 修饰方法：直接在方法声明前加上 `synchronized` 关键字，表示整个方法是同步的。
   - 修饰代码块：在需要同步的代码前使用 `synchronized`，指定锁的对象，并在大括号内编写同步代码。

5. **示例**：
   - 修饰方法：
     ```java
     public synchronized void myMethod() {
         // 同步代码
     }
     ```
   - 修饰代码块：
     ```java
     public void myMethod() {
         synchronized (this) {
             // 同步代码
         }
     }
     ```

6. **锁对象**：在代码块中，锁对象可以是任意对象，通常使用当前实例 `this` 或者一个特定的对象作为锁。

7. **死锁**：如果多个线程相互等待对方持有的锁，可能会导致死锁。

8. **性能问题**：`synchronized` 可能会导致性能下降，因为它涉及到线程的阻塞和唤醒，所以在高并发场景下需要谨慎使用。

9. **替代方案**：Java并发API提供了其他同步机制，如 `ReentrantLock`、`Semaphore`、`CountDownLatch` 等，它们提供了更灵活的线程同步控制。

10. **可见性**：`synchronized` 还确保了变量的可见性，即一个线程对共享变量的修改对其他线程是可见的。

使用 `synchronized` 时，需要考虑其对性能的影响以及可能导致的死锁问题。

#### Lock锁

Java `Lock` 锁是Java并发API中提供的一种锁机制，它提供了比 `synchronized` 更灵活的线程同步控制。`Lock` 接口位于 `java.util.concurrent.locks` 包中，最常用的实现是 `ReentrantLock`。

以下是使用 `Lock` 锁的一些关键点：

1. **显示锁获取与释放**：与 `synchronized` 自动获取和释放锁不同，`Lock` 需要显示地调用 `lock()` 方法来获取锁，并调用 `unlock()` 方法来释放锁。

2. **可中断的锁获取**：`Lock` 尝试获取锁时，线程可以选择响应中断，即线程在等待锁的过程中可以被中断。

3. **尝试非阻塞获取锁**：`Lock` 提供了 `tryLock()` 方法，允许线程尝试获取锁，如果锁不可用，线程可以不等待立即返回。

4. **超时获取锁**：`tryLock(long timeout, TimeUnit unit)` 方法允许线程在指定的时间内尝试获取锁，超过时间仍未获取到锁则返回 `false`。

5. **公平性**：`ReentrantLock` 可以通过构造函数设置公平性（`fair` 参数），公平性高的锁按照线程请求的顺序来分配，而非公平锁则允许线程抢占。

6. **可重入**：与 `synchronized` 一样，`ReentrantLock` 也是可重入的，即同一个线程可以多次获取同一把锁。

7. **条件变量**：`Lock` 支持条件变量（`Condition`），允许线程在某些条件下等待或唤醒，类似于 `synchronized` 中的 `wait()` 和 `notify()`。

8. **锁的实现**：`Lock` 的实现类 `ReentrantLock` 内部使用了一个同步器（`Sync` 类），它继承自 `AbstractQueuedSynchronizer`（AQS）。

9. **死锁检测**：`ReentrantLock` 可以提供死锁检测的诊断工具。

10. **示例**：
    ```java
    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;
    
    public class LockExample {
        private Lock lock = new ReentrantLock();
    
        public void performAction() {
            lock.lock();  // 获取锁
            try {
                // 执行一些操作
            } finally {
                lock.unlock();  // 释放锁
            }
        }
    
        public void anotherAction() {
            if (lock.tryLock()) {
                try {
                    // 执行一些操作
                } finally {
                    lock.unlock();
                }
            } else {
                // 处理无法获取锁的情况
            }
        }
    }
    ```

使用 `Lock` 锁时，必须在 `finally` 块中释放锁，以确保即使在获取锁后发生异常，锁也能被正确释放，避免死锁。

## 死锁

> 线程死锁是由于两个或多个线程互相持有对方所需资源，导致这些线程处于等待状态，无法继续执行

以下是Java中死锁的一些关键特点和解决方法：

### 死锁的特点

1. **互斥条件**：每个线程已经持有至少一个资源，但还需要额外的资源，只有获取到这些资源才能继续执行。
2. **占有和等待条件**：线程已经持有一个或多个资源，并且正在等待获取其他线程持有的资源。
3. **不可剥夺条件**：线程获得的资源在未使用完之前不能被其他线程剥夺，只能由持有者主动释放。
4. **循环等待条件**：存在一个线程资源的循环等待链，每个线程都在等待下一个线程所占有的资源。

### 死锁的识别
- 系统资源利用效率降低。
- 线程活动停止，处于阻塞状态。

### 死锁的解决方法
1. **避免死锁的策略**：
   - **破坏互斥条件**：设计算法使得资源可以被共享，但这通常很难实现。
   - **破坏占有和等待条件**：要求线程在执行前一次性申请所有需要的资源。
   - **破坏不可剥夺条件**：允许线程临时释放资源以避免死锁，但这可能影响程序逻辑。
   - **破坏循环等待条件**：通过规定资源的请求顺序来预防循环等待。

2. **死锁检测和恢复**：
   - **检测**：系统可以周期性地检测资源分配图是否存在循环等待。
   - **恢复**：一旦检测到死锁，可以通过终止线程、资源剥夺、资源重新分配等方法来恢复。

3. **使用锁超时机制**：在尝试获取锁时设置超时时间，如果超时仍未获取到锁，则释放已持有的锁并重试或进行其他操作。

4. **使用Java并发API**：
   - `ReentrantLock` 提供了尝试非阻塞获取锁的 `tryLock()` 方法。
   - `java.util.concurrent` 包中的其他同步辅助类，如 `Semaphore`、`CountDownLatch`、`CyclicBarrier` 和 `Exchanger`，可以帮助避免死锁。

5. **避免嵌套锁**：减少锁的使用，避免一个线程持有多个锁，特别是在不确定其他线程将如何使用这些锁的情况下。

6. **使用死锁检测工具**：Java提供了一些工具，如jconsole和jvisualvm，它们可以帮助检测和分析死锁。

7. **代码审查和测试**：通过代码审查和多线程测试来发现潜在的死锁问题。

## 生产者和消费者

> 消费者问题是一个经典的多线程同步问题，它描述了两个线程组（生产者和消费者）对共享资源（如队列）的访问。生产者的任务是生成数据并将其放入共享队列，而消费者的任务是从共享队列中取出数据并进行处理。为避免资源冲突和数据不一致，需要适当的同步机制

示例：

```java
public class ProducerConsumer {
    private final Queue<Integer> queue = new LinkedList<>();
    private final int capacity = 10;

    // 同步方法，确保生产者和消费者不会同时访问队列
    public synchronized void produce() {
        while (true) {
            // 等待队列有空间
            while (queue.size() == capacity) {
                System.out.println("队列已满，生产者等待");
                try {
                    wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            // 生产数据
            int product = (int) (Math.random() * 100);
            System.out.println("生产了：" + product);
            queue.add(product);
            notifyAll(); // 通知消费者队列中有数据
        }
    }

    public synchronized void consume() {
        while (true) {
            // 等待队列中有数据
            while (queue.isEmpty()) {
                System.out.println("队列为空，消费者等待");
                try {
                    wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            // 消费数据
            int consumed = queue.poll();
            System.out.println("消费了：" + consumed);
            notifyAll(); // 通知生产者队列有空间
        }
    }

    public static void main(String[] args) {
        ProducerConsumer pc = new ProducerConsumer();

        Thread producer = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                pc.produce();
            }
        });

        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                pc.consume();
            }
        });

        producer.start();
        consumer.start();
    }
}
```

### 等待和唤醒

|       方法名       |                             说明                             |
| :----------------: | :----------------------------------------------------------: |
|   `void wait()`    | 线程等待，直到另一个线程调用该对象的`notify()`方法或` notifyAll()`方法 |
|  `void notify()`   |               唤醒正在等待对象监视器的单个线程               |
| `void notifyAll()` |               唤醒正在等待对象监视器的所有线程               |

等待（wait）

- 当线程调用一个对象的 `wait()` 方法时，它会释放该对象的锁，并进入该对象的等待池（wait set）。
- 等待池中的线程不会执行，直到它们被其他线程唤醒。
- 等待的线程需要在同步代码块或同步方法中调用 `wait()`。

唤醒（notify/notifyAll）

- `notify()` 方法唤醒在该对象等待池中等待的某个线程。选择哪个线程唤醒是不确定的。
- `notifyAll()` 方法唤醒在该对象等待池中等待的所有线程。

等待和唤醒的使用场景

- 当一个线程需要等待某个条件成立时，可以使用 `wait()` 方法挂起。
- 当其他线程改变状态并希望通知等待的线程时，可以使用 `notify()` 或 `notifyAll()`。

示例代码

```java
public class WaitNotifyExample {
    private boolean condition = false;

    public synchronized void waitingMethod() {
        while (!condition) { // 循环等待条件满足
            try {
                wait(); // 调用wait()释放锁，并等待
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        // 条件满足后执行操作
        System.out.println("条件满足，执行后续操作");
    }

    public synchronized void notifyingMethod() {
        // 改变条件状态
        condition = true;
        notify(); // 唤醒等待的线程
    }

    public static void main(String[] args) {
        WaitNotifyExample example = new WaitNotifyExample();

        Thread waitingThread = new Thread(() -> example.waitingMethod());
        Thread notifyingThread = new Thread(() -> example.notifyingMethod());

        waitingThread.start();
        notifyingThread.start();
    }
}
```

注意事项

- 等待和唤醒必须在同步的上下文中使用，即必须在 `synchronized` 方法或同步代码块内调用。
- 使用 `wait()` 时通常配合循环使用，因为 `wait()` 可能因为伪唤醒（spurious wakeup）而返回，即使条件并未满足。
- `notify()` 只会唤醒一个线程，而 `notifyAll()` 会唤醒所有等待的线程，根据需要选择使用。
- 唤醒线程时，被唤醒的线程需要重新获得对象的锁才能继续执行。

通过合理使用等待和唤醒机制，可以协调多个线程的工作，实现复杂的线程同步逻辑。

示例：

```java
public class ProducerConsumerExample {
    private final int[] buffer;
    private int putIndex, takeIndex, count;

    public ProducerConsumerExample(int size) {
        buffer = new int[size];
        this.putIndex = 0;
        this.takeIndex = 0;
        this.count = 0;
    }

    // 生产者线程的方法
    public synchronized void produce(int value) {
        while (count == buffer.length) { // 如果没有空间，生产者等待
            System.out.println("生产者等待，缓冲区已满");
            wait();
        }
        buffer[putIndex] = value; // 生产产品
        putIndex = (putIndex + 1) % buffer.length; // 更新生产索引
        count++; // 增加产品计数
        System.out.println("生产了 " + value);
        notifyAll(); // 唤醒可能正在等待的消费者
    }

    // 消费者线程的方法
    public synchronized void consume() {
        while (count == 0) { // 如果没有产品，消费者等待
            System.out.println("消费者等待，缓冲区为空");
            wait();
        }
        int value = buffer[takeIndex]; // 消费产品
        takeIndex = (takeIndex + 1) % buffer.length; // 更新消费索引
        count--; // 减少产品计数
        System.out.println("消费了 " + value);
        notifyAll(); // 唤醒可能正在等待的生产者
    }

    public static void main(String[] args) {
        ProducerConsumerExample pcExample = new ProducerConsumerExample(5);

        Thread producer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                pcExample.produce(i);
            }
        });

        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                pcExample.consume();
            }
        });

        producer.start();
        consumer.start();
    }
}
```

在这个示例中，我们创建了一个固定大小的数组 `buffer` 作为生产者和消费者共享的存储空间。`produce` 和 `consume` 方法都是同步的，它们分别被生产者和消费者线程调用：

- `produce` 方法首先检查 `count` 是否已经达到 `buffer` 的长度，如果是，则生产者线程将调用 `wait()` 方法进入等待状态。当 `count` 减小时，生产者线程会被唤醒。生产者线程然后生成一个值，将其放入 `buffer` 中，并调用 `notifyAll()` 方法来唤醒可能正在等待的消费者线程。
- `consume` 方法首先检查 `count` 是否为0，如果是，则消费者线程将调用 `wait()` 方法进入等待状态。当 `count` 增大时，消费者线程会被唤醒。消费者线程然后从 `buffer` 中取出一个值，并调用 `notifyAll()` 方法来唤醒可能正在等待的生产者线程。

使用 `notifyAll()` 是为了确保在缓冲区状态改变时，所有可能等待的线程都有机会被唤醒。在某些情况下，如果确定只有一个线程在等待，使用 `notify()` 也可以。

## 线程状态

Java线程有五种基本状态，这些状态分别对应线程在其生命周期中的不同点：

1. **新建（New）**:
   - 线程对象被创建，但尚未启动。处于这个状态的线程还没有开始执行。

2. **可运行（Runnable）**:
   - 线程已经调用了`start()`方法，此时线程处于就绪状态，等待JVM为其分配CPU时间。处于可运行状态的线程可能正在执行，也可能正在等待CPU时间。

3. **运行（Running）**:
   - 线程已经获得CPU时间，并正在执行其`run()`方法中的代码。

4. **阻塞（Blocked）**:
   - 线程等待某个锁或资源释放，不能继续执行。阻塞状态通常发生在线程试图获取一个已被其他线程持有的锁时。阻塞状态的线程不会消耗CPU资源。

5. **等待（Waiting）**:
   - 线程因为调用了`wait()`、`join()`或`LockSupport.park()`方法而进入等待状态。在这种状态下，线程需要被其他线程唤醒或等待的条件得到满足。

6. **超时等待（Timed Waiting）**:
   - 类似于等待状态，但有一个指定的等待时间。线程因为调用了带有超时参数的`sleep()`、`wait()`、`join()`或`LockSupport.parkNanos()`、`LockSupport.parkUntil()`方法而进入超时等待状态。

7. **终止（Terminated）**:
   - 线程的`run()`方法已经执行完成或者因为异常而终止，此后线程无法再运行。

Java线程状态转换图通常如下：![线程状态](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81.png)

在Java中，可以通过`Thread`类的`getState()`方法获取线程的当前状态，但这个方法返回的状态是线程状态的粗略表示，主要用于监控。实际线程状态的转换由JVM内部管理，并不直接暴露给开发者。

## 线程池

基本原理：![线程池原理](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/线程池原理.png)

1. 创建一个池子：创建`Executors`中的静态方法
2. 有任务需要执行的时候，才会创建线程对象，任务执行完毕后，线程归还：使用`submit()`方法，线程池会自动创建
3. 所有任务执行完毕，关闭连接池：使用`shutdown()`方法

### Executors类

> `Executors` 类是 Java 中 `java.util.concurrent` 包的一部分，它提供了一组静态工厂方法来创建不同类型的线程池。这些线程池可以用于并发执行任务，而不必手动管理线程的生命周期。`Executors` 类的目的是简化线程池的创建和管理

`Executors` 类提供的几种常用的线程池实现：

1. **`newCachedThreadPool()`**：创建一个可缓存的线程池，它会根据需要创建新线程，但当线程空闲时会回收它们。适用于执行大量短生命周期任务的情况。
2. **`newFixedThreadPool(int nThreads)`**：创建一个固定大小的线程池，其中包含固定数量的线程。适用于执行大量任务，但任务执行时间不确定的情况。
3. **`newSingleThreadExecutor()`**：创建一个单线程的线程池，它只创建一个线程，适用于需要按顺序执行任务的情况。
4. **`newScheduledThreadPool(int corePoolSize)`**：创建一个可调度的线程池，它可以安排任务在给定的延迟后运行或定期执行。
5. **`newWorkStealingPool(int parallelism)`**：创建一个工作窃取线程池，它使用多个队列，每个线程都有自己的队列，当一个线程的队列为空时，它可以从其他线程的队列中窃取任务。
6. **`newSingleThreadScheduledExecutor()`**：创建一个单线程的可调度线程池，它可以根据需要延迟执行任务或周期性执行任务。
7. `privilegedThreadFactory()`：创建一个线程工厂，用于创建具有特权的线程。
8. `defaultThreadFactory()`：创建一个默认的线程工厂，用于创建线程。

```java
import java.util.concurrent.*;

public class ExecutorsExample {
    public static void main(String[] args) {
        // 使用 Executors 创建不同类型的线程池
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(4);
        ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
        ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
        ExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(2);
        ExecutorService workStealingPool = Executors.newWorkStealingPool();

        // 提交任务到固定大小的线程池
        for (int i = 0; i < 10; i++) {
            fixedThreadPool.execute(new Task("Fixed Task " + i));
        }

        // 关闭线程池
        fixedThreadPool.shutdown();
        cachedThreadPool.shutdown();
        singleThreadExecutor.shutdown();
        scheduledThreadPool.shutdown();
        workStealingPool.shutdown();
    }
}

class Task implements Runnable {
    private String taskName;

    public Task(String taskName) {
        this.taskName = taskName;
    }

    @Override
    public void run() {
        System.out.println(taskName + " executed by " + Thread.currentThread().getName());
    }
}
```

### ThreadPoolExecutor构造函数

> `ThreadPoolExecutor` 是 Java 中 `java.util.concurrent` 包的一部分，它提供了一个非常灵活的线程池实现。与 `Executors` 类提供的静态工厂方法（如 `newCachedThreadPool()`）相比，`ThreadPoolExecutor` 允许开发者自定义线程池的许多参数，从而更精确地控制线程池的行为

**`ThreadPoolExecutor`** 的一些关键特性：

1. **核心线程数**：线程池中始终存活的线程数量。

2. **最大线程数**：线程池中允许的最大线程数量。

3. **工作队列**：用于存储等待执行的任务的队列。

4. **线程工厂**：用于创建新线程的工厂。

5. **拒绝策略**：当任务太多而不能立即执行，并且队列已满时，线程池将采用何种策略来处理新提交的任务。

   1. 什么时候拒绝任务：当提交的任务 **> **池子中最大线程数量 **+** 队列容量

   2. 如何拒绝：

      **任务拒绝策略**

      - **`ThreadPoolExecuto.AbortPolicy`**：默认拒绝策略。当任务被拒绝时，`ThreadPoolExecutor` 会抛出 `RejectedExecutionException`。
      - **`ThreadPoolExecuto.CallerRunsPolicy`**：调用者运行策略。如果任务无法被接受，那么调用 `execute` 方法的线程将尝试自己执行该任务。如果执行线程已经关闭或处于中断状态，则会抛出 `RejectedExecutionException`。
      - **`ThreadPoolExecuto.DiscardPolicy`**：丢弃策略。如果任务无法被接受，线程池将默默地丢弃该任务。
      - **`ThreadPoolExecuto.DiscardOldestPolicy`**：丢弃最老任务策略。如果任务无法被接受，线程池将丢弃工作队列中最前面的任务，然后尝试再次提交当前任务。

**`ThreadPoolExecutor`** 的构造函数如下：

```java
public ThreadPoolExecutor(int corePoolSize,
                           int maximumPoolSize,
                           long keepAliveTime,
                           TimeUnit unit,
                           BlockingQueue<Runnable> workQueue,
                           ThreadFactory threadFactory,
                         new CustomRejectedExecutionHandler())
```

参数说明：

- `corePoolSize`：核心线程数，不能小于 0
- `maximumPoolSize`：最大线程数，不能小于等于 0，最大数量>=核心线程数
- `keepAliveTime`：当线程数大于核心线程数时，多余的空闲线程能够存活的时间，不能小于 0
- `unit`：`keepAliveTime` 参数的时间单位。
- `workQueue`：用于存储任务的工作队列，不能为`NULL`
- `threadFactory`：用于创建新线程的工厂，不能为`NULL`
- 

使用**`ThreadPoolExecutor`**示例代码如下：

```java
import java.util.concurrent.*;

public class ThreadPoolExecutorExample {
    public static void main(String[] args) {
        // 创建一个固定大小的线程池
        int corePoolSize = 5;
        int maximumPoolSize = 10;
        long keepAliveTime = 1;
        TimeUnit unit = TimeUnit.MINUTES;
        BlockingQueue<Runnable> workQueue = new ArrayBlockingQueue<>(10);
        ThreadFactory threadFactory = Executors.defaultThreadFactory();
        RejectedExecutionHandler handler = new ThreadPoolExecutor.AbortPolicy();

        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                corePoolSize,
                maximumPoolSize,
                keepAliveTime,
                unit,
                workQueue,
                threadFactory,
                handler
        );

        for (int i = 0; i < 15; i++) {
            executor.execute(new Task("Task " + i));
        }

        // 关闭线程池
        executor.shutdown();
    }
}

class Task implements Runnable {
    private String taskName;

    public Task(String taskName) {
        this.taskName = taskName;
    }

    @Override
    public void run() {
        System.out.println(taskName + " executed by " + Thread.currentThread().getName());
    }
}
```

在这个例子中，创建了一个具有固定核心线程数和最大线程数的线程池，使用了一个有界队列来存储任务，并定义了一个拒绝策略，当任务太多而不能立即执行时，将抛出 `RejectedExecutionException`

