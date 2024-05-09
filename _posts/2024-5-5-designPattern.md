---
layout: post
title: 设计模式
description: 设计模式笔记。
author: MuZhi
date: 2024-5-5 11:31:59 +0800
categories: [设计模式]
tags: [设计模式,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-logo.png
  alt: 设计模式.
---

## 思维导图

![designPattern-xmind](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/designPattern-xmind.png)

## 1.设计模式相关内容介绍

### 1.1.软件设计模式的产生背景

软件设计模式（Software Design Pattern）的产生背景可以追溯到建筑设计领域。最初，“设计模式”这个术语并不是出现在软件设计中，而是被用于建筑领域的设计中。1977年，美国著名建筑大师、加利福尼亚大学伯克利分校环境结构中心主任克里斯托夫·亚历山大（Christopher Alexander）在他的著作《建筑模式语言：城镇、建筑、构造》中描述了一些常见的建筑设计问题，并提出了253种关于对城镇、邻里、住宅、花园和房间等进行设计的基本模式。

到了1990年，软件工程界开始研讨设计模式的话题，并召开了多次关于设计模式的研讨会。直到1995年，艾瑞克·伽马（Erich Gamma）、理査德·海尔姆（Richard Helm）、拉尔夫·约翰森（Ralph Johnson）、约翰·威利斯迪斯（John Vlissides）等4位作者合作出版了《设计模式：可复用面向对象软件的基础》一书，在此书中收录了23个设计模式，这是设计模式领域里程碑的事件，导致了软件设计模式的突破。这4位作者在软件开发领域里也以他们的“四人组”（Gang of Four，GoF）著称。

设计模式的引入，为软件工程提供了一种标准化、可复用的方法来解决在面向对象设计中遇到的常见问题，从而提高了软件的质量和开发效率。设计模式的本质是面向对象设计原则的实际运用，是对类的封装性、继承性和多态性以及类的关联关系和组合关系的充分理解。正确使用设计模式可以提高程序员的思维能力、编程能力和设计能力，使程序设计更加标准化、代码编制更加工程化，从而缩短软件的开发周期，并提高设计的代码的可重用性、可读性、可靠性、灵活性和可维护性。

### 1.2.软件设计模式的概念

软件设计模式是一套被广泛认可并反复使用的解决方案，它们针对软件设计过程中出现的一些常见问题提供了通用的结构和实现方式。设计模式代表了最佳的实践，它们是经过时间检验的，并且可以帮助开发者避免重复发明轮子，即避免重新解决已经被解决过的问题。

软件设计模式的核心概念包括：

1. **问题**：设计模式通常围绕一个特定的设计问题展开，这个问题在软件设计中反复出现。

2. **解决方案**：每个设计模式都提供了一种通用的方法或模板，用以解决上述问题。这包括一系列步骤和结构，指导如何组织代码。

3. **效果**：使用设计模式会有一定的效果，包括正面的（如提高代码的可维护性和可扩展性）和负面的（如可能增加设计的复杂性）。

4. **可重用性**：设计模式的目的是提高代码的重用性，使得在不同的项目中可以应用相同的解决方案。

5. **标准化**：设计模式提供了一种通用语言，使得开发者可以更容易地沟通和理解设计决策。

6. **面向对象**：设计模式通常在面向对象编程（OOP）的背景下讨论，因为它们依赖于OOP的概念，如类、对象、继承和多态。

7. **分类**：设计模式通常分为三大类：
   - **创建型模式**：涉及对象创建的各种机制，如单例、原型、工厂方法等。
   - **结构型模式**：关注类和对象的组合，如适配器、代理、组合等。
   - **行为型模式**：涉及对象之间的交互以及职责的分配，如策略、观察者、命令等。

### 1.3.学习设计模式的必要性

学习设计模式对于软件开发者来说具有多方面的必要性，以下是一些关键点：

1. **提高思维和设计能力**：设计模式体现了面向对象设计的深层次思考，通过学习这些模式，开发者可以提升自己的抽象思维能力，更好地理解和应用面向对象的设计原则。

2. **促进代码重用**：设计模式提供了解决特定问题的通用模板，使得开发者可以在不同项目中重用经过验证的设计和代码，减少重复劳动。

3. **增强代码的可读性和可维护性**：设计模式通常具有良好的结构，遵循一定的命名和组织规范，这使得使用设计模式编写的代码更易于理解和维护。

4. **标准化开发过程**：设计模式为开发者提供了一种共同的语言和思维方式，有助于团队成员之间的沟通，使得开发过程更加标准化和统一。

5. **提升软件质量**：正确应用设计模式可以减少设计中的错误，提高软件的可靠性和灵活性，从而提升整体的软件质量。

6. **加快开发速度**：由于设计模式是针对常见问题的成熟解决方案，使用它们可以避免从头开始设计解决方案，从而加快开发速度。

7. **适应性和扩展性**：设计模式考虑了软件的可扩展性，使得未来可以在不重大修改现有代码的情况下添加新功能。

8. **解决复杂问题**：设计模式提供了一种结构化的方法来解决复杂的设计问题，帮助开发者构建更加清晰和高效的软件架构。

9. **职业发展**：在许多技术面试中，对设计模式的了解是一个常见的考核点。掌握设计模式对于职业发展和获得更好的工作机会是有益的。

10. **系统重构的指导**：设计模式也可以作为系统重构的指南，帮助开发者识别现有系统中的问题，并提供改进的方向。

### 1.4.设计模式分类

设计模式通常分为三大类，每类关注解决不同领域的设计问题：

#### 1.4.1.创建型模式（Creational Patterns）:

- 这些模式专注于对象的创建过程，试图以最适合项目需求的方式创建对象。创建型模式将对象创建的逻辑封装起来，隐藏对象如何被创建和组合的细节。
- 常见的创建型模式包括：
  - 单例（Singleton）模式：确保一个类只有一个实例，并提供一个全局访问点。
  - 原型（Prototype）模式：通过复制现有的实例来创建新的实例。
  - 工厂方法（Factory Method）模式：定义一个创建对象的接口，但让子类决定要创建的对象类型。
  - 抽象工厂（Abstract Factory）模式：提供一个创建一系列相关或相互依赖对象的接口。
  - 建造者（Builder）模式：将复杂对象的构建过程封装起来，允许它分步骤创建。

#### 1.4.2.结构型模式（Structural Patterns）:

- 结构型模式处理类和对象的组合，形成更大的结构。它们描述如何将对象和类组装成较大的结构，同时保持结构的灵活和高效。
- 常见的结构型模式包括：
  - 适配器（Adapter）模式：允许将不兼容的接口转换为一个可以使用的兼容接口。
  - 桥接（Bridge）模式：将抽象部分与其实现部分分离，使它们可以独立地变化。
  - 组合（Composite）模式：允许将对象组合成树状结构，以表示“部分-整体”的层次结构。
  - 装饰（Decorator）模式：动态地为对象添加行为。
  - 外观（Facade）模式：为子系统中的一组接口提供一个统一的接口。
  - 享元（Flyweight）模式：以共享的方式高效地支持大量细粒度的对象。

#### 1.4.3.行为型模式（Behavioral Patterns）：

- 行为型模式主要关注对象之间的相互作用以及它们怎样相互分配职责。
- 常见的行为型模式包括：
  - 模板方法（Template Method）模式：定义算法的骨架，将一些步骤延迟到子类中实现。
  - 策略（Strategy）模式：定义一系列算法，并将每个算法封装起来，使它们可以互换。
  - 命令（Command）模式：将一个请求封装为一个对象，允许用户使用不同的请求。
  - 职责链（Chain of Responsibility）模式：将请求沿着处理者链传递，直到有处理者能够处理该请求。
  - 观察者（Observer）模式：当对象间存在一对多关系时，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。
  - 状态（State）模式：允许对象根据其内部状态改变其行为。
  - 迭代器（Iterator）模式：提供一种顺序访问聚合对象元素的方法，而不暴露其内部表示。

### 1.5.UML图

统一建模语言（Unified Modeling Language，UML）是一种标准化的建模语言，它被广泛用于软件工程领域，用于设计、分析和文档化软件系统。UML图提供了一套图形化的符号，用于创建软件系统的抽象模型。以下是UML中一些主要的图形化图表类型：

1. **用例图（Use Case Diagram）**：
   - 描述系统的功能以及与这些功能交互的外部实体（演员）。

2. **类图（Class Diagram）**：
   - 展示了系统的静态结构，包括类、它们的属性、操作以及类之间的关系（如继承、关联和依赖）。

3. **对象图（Object Diagram）**：
   - 类似于类图，但是展示了对象实例以及它们之间的关系。

4. **包图（Package Diagram）**：
   - 描述系统的分层结构，以及包（模块）之间的关系。

5. **序列图（Sequence Diagram）**：
   - 展示了对象之间交互的顺序，强调时间顺序和对象之间消息的交换。

6. **协作图（Collaboration Diagram）**：
   - 与序列图类似，但强调对象的结构和组织，而不是时间顺序。

7. **状态图（State Diagram）**：
   - 描述对象状态的变化以及触发这些变化的事件。

8. **活动图（Activity Diagram）**：
   - 展示了业务流程或工作流中的活动序列，以及活动之间的控制流。

9. **组件图（Component Diagram）**：
   - 描述系统中软件组件的组织以及它们之间的关系。

10. **部署图（Deployment Diagram）**：
    - 展示了系统的物理部署，包括硬件、节点以及它们上运行的软件组件。

11. **综合图（Composite Structure Diagram）**：
    - 类似于类图和对象图的组合，展示了类的组合结构以及它们之间的关系。

12. **时间图（Timing Diagram）**：
    - 展示了随时间变化的信号交互，常用于实时系统的设计。

13. **交互概览图（Interaction Overview Diagram）**：
    - 结合了序列图和协作图的元素，提供了一个高层次的交互概览。

### 1.6.软件设计原则

软件设计原则是指导软件设计和架构的一系列基本原则，它们帮助开发者创建出灵活、可维护、可扩展和可重用的软件系统。以下是一些核心的软件设计原则：

#### 1.6.1. 单一职责原则(Single Responsibility Principle, SRP)：

- 一个类应该只有一个引起它变化的原因，即一个类只负责一项职责。

##### 示例：

- 假设我们有一个名为 `Order` 的类，它负责处理订单的创建、支付、发货等业务逻辑。这个类可能如下所示：

  ```java
  public class Order {
      private double amount;
      private boolean paid;
      private boolean shipped;
  
      public Order(double amount) {
          this.amount = amount;
      }
  
      public void pay() {
          paid = true;
      }
  
      public void ship() {
          if (paid) {
              shipped = true;
          }
      }
  
      // 其他与订单相关的业务逻辑...
  }
  ```

##### 问题：

- 上述代码中`Order`类承担了多个职责：
  1. 订单创建和管理，
  2. 支付逻辑，
  3. 发货逻辑

- 当支付逻辑或发货逻辑发生变化时，可能会影响到整个 `Order` 类，这违反了单一职责原则。

##### 改进：

- 为了遵循单一职责原则，我们可以将 `Order` 类拆分成多个类，每个类负责一个单一职责：

  ```java
  // 订单管理类
  public class OrderManager {
      public void createOrder(double amount) {
          // 创建订单逻辑
      }
  
      // 其他与订单管理相关的业务逻辑...
  }
  
  // 支付服务类
  public class PaymentService {
      public void pay(Order order) {
          order.pay();
      }
  
      // 其他与支付相关的业务逻辑...
  }
  
  // 发货服务类
  public class ShippingService {
      public void ship(Order order) {
          if (order.isPaid()) {
              order.ship();
          }
      }
  
      // 其他与发货相关的业务逻辑...
  }
  
  public class Order {
      private double amount;
      private boolean paid;
      private boolean shipped;
  
      // 构造函数、getter和setter方法...
  }
  ```

##### 改进后：

- `OrderManager` 类负责订单的创建和管理。
- `PaymentService` 类负责处理支付逻辑。
- `ShippingService` 类负责处理发货逻辑。
- `Order` 类只负责订单数据的封装和状态的管理。

- 通过这种方式，每个类都只有一个单一的职责，当支付或发货逻辑需要改变时，我们只需要修改对应的服务类，而不会影响到其他部分。这样的设计更加灵活，易于维护和扩展。

#### 1.6.2. 开闭原则(Open-Closed Principle, OCP)：

- 软件实体应当对扩展开放，对修改封闭。这意味着设计时应当使软件模块易于扩展，但是不需要修改现有代码。

##### 示例：

- 假设我们有一个形状（Shape）接口，以及实现了该接口的矩形（Rectangle）、圆形（Circle）等类。现在我们需要计算各种形状的面积。

- 初始设计：

  ```java
  interface Shape {
      double getArea();
  }
  
  class Rectangle implements Shape {
      private double width;
      private double height;
  
      public Rectangle(double width, double height) {
          this.width = width;
          this.height = height;
      }
  
      public double getArea() {
          return width * height;
      }
  }
  
  class Circle implements Shape {
      private double radius;
  
      public Circle(double radius) {
          this.radius = radius;
      }
  
      public double getArea() {
          return Math.PI * radius * radius;
      }
  
      // 其他形状类...
  }
  
  class ShapeCalculator {
      public double calculateArea(Shape shape) {
          return shape.getArea();
      }
  }
  ```

##### 问题：

- 如果我们需要添加新的形状，比如三角形（Triangle），我们可能会直接修改 `ShapeCalculator` 类，这违反了开闭原则。

##### 改进：

- 为了遵循开闭原则，我们不应该修改 `ShapeCalculator` 类，而是添加一个新的形状类，如下：

  ```java
  class Triangle implements Shape {
      private double base;
      private double height;
  
      public Triangle(double base, double height) {
          this.base = base;
          this.height = height;
      }
  
      public double getArea() {
          return 0.5 * base * height;
      }
  }
  ```

- 现在，我们可以在不修改 `ShapeCalculator` 的情况下使用新的形状：

  ```java
  ShapeCalculator calculator = new ShapeCalculator();
  Shape triangle = new Triangle(5, 10);
  double triangleArea = calculator.calculateArea(triangle);
  ```

- 通过这种方式，`ShapeCalculator` 类对扩展开放（可以计算新形状的面积），但是对修改封闭（不需要改变 `ShapeCalculator` 的代码）。

#### 1.6.3. 里氏代换原则(Liskov Substitution Principle, LSP)：

- 基类的对象能够被其子类的对象所替代，子类应当可以被子类对象所替代，而程序的行为不应该发生变化。

##### 错误示例：

```java
class Bird {
    public void fly() {
        System.out.println("The bird is flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new RuntimeException("I can't fly");
    }
}

class AnimalControl {
    public void takeCareOf(Bird bird) {
        // 假设这里有些照顾鸟儿的逻辑
        bird.fly(); // 调用飞的方法
    }
}

public class Main {
    public static void main(String[] args) {
        AnimalControl center = new AnimalControl();
        Bird penguin = new Penguin();
        center.takeCareOf(penguin); // 运行时错误
    }
}
```

在这个例子中，`Penguin` 类继承自 `Bird` 类，但是它重写了 `fly` 方法，并在其中抛出了一个异常。当我们尝试使用 `AnimalControl` 类来照顾所有的 `Bird` 对象时，如果传入一个 `Penguin` 对象，程序会在运行时抛出异常，因为 `Penguin` 不能飞行。

##### 正确示例：

为了遵循里氏代换原则，我们可以将能够飞行的行为定义在一个单独的接口中，然后让那些能够飞行的 `Bird` 类实现这个接口：

```java
interface Flyable {
    void fly();
}

class Bird {
    // 其他鸟类行为...
}

class FlyingBird extends Bird implements Flyable {
    public void fly() {
        System.out.println("The bird is flying");
    }
}

class Penguin extends Bird {
    // 企鹅不会飞，不实现Flyable接口
    public void fly() {
        System.out.println("The penguin can't fly");
    }
}

class AnimalControl {
    public void takeCareOf(Flyable flyable) {
        // 假设这里有些照顾能飞动物的逻辑
        flyable.fly(); // 现在安全调用fly方法
    }
}

public class Main {
    public static void main(String[] args) {
        AnimalControl center = new AnimalControl();
        FlyingBird flyingBird = new FlyingBird();
        center.takeCareOf(flyingBird); // 正常运行

        Bird penguin = new Penguin();
        // center.takeCareOf(penguin); // 现在这行代码不再合法，避免了运行时错误
    }
}
```

在这个修正后的例子中，`Flyable` 接口用于定义飞行行为，只有那些能够飞行的鸟类才实现这个接口。`Penguin` 类不再实现 `Flyable` 接口，因此 `AnimalControl` 类只能接受能够飞行的鸟类。这样，当我们尝试将一个 `Penguin` 对象传递给 `AnimalControl` 时，编译器会报错，避免了运行时错误。

通过这种方式，我们确保了 `Flyable` 对象可以在不引起任何异常的情况下替换 `Bird` 对象，而不会破坏 `AnimalControl` 类的逻辑，符合里氏代换原则。

#### 1.6.4. 依赖倒置原则(Dependency Inversion Principle, DIP)：

- 高层模块不应依赖于低层模块，两者都应该依赖于抽象；抽象不应依赖于细节，细节应依赖于抽象。
- 依赖倒置原则的要点：
  1. **高层模块不依赖于低层模块**：高层模块是指贴近业务逻辑的模块，低层模块是指实现具体功能的模块。
  2. **依赖于抽象**：模块间的依赖应该依赖于抽象的接口或者抽象的类，而不是具体的实现类。
  3. **抽象不应依赖于细节**：接口或抽象类不依赖于具体的实现细节。
  4. **细节应依赖于抽象**：实现类需要依赖于接口或者抽象类定义的抽象。

##### 错误示例：

```java
class Device {
    public void on() {
        System.out.println("Device is on");
    }

    public void off() {
        System.out.println("Device is off");
    }
}

class DeviceController {
    private Device device;

    public DeviceController(Device device) {
        this.device = device;
    }

    public void activateDevice() {
        device.on();
    }

    public void deactivateDevice() {
        device.off();
    }
}

public class Main {
    public static void main(String[] args) {
        Device myDevice = new Device();
        DeviceController controller = new DeviceController(myDevice);
        controller.activateDevice(); // 启动设备
        controller.deactivateDevice(); // 关闭设备
    }
}
```

在这个例子中，`DeviceController` 直接依赖于 `Device` 类的具体实现，违反了依赖倒置原则。

##### 正确示例：

为了遵循DIP，我们可以定义一个抽象的接口，让 `DeviceController` 依赖于这个抽象接口，而不是任何具体的实现。

```java
interface IDevice {
    void on();
    void off();
}

class Device implements IDevice {
    public void on() {
        System.out.println("Device is on");
    }

    public void off() {
        System.out.println("Device is off");
    }
}

class DeviceController {
    private IDevice device;

    public DeviceController(IDevice device) {
        this.device = device;
    }

    public void activateDevice() {
        device.on();
    }

    public void deactivateDevice() {
        device.off();
    }
}

public class Main {
    public static void main(String[] args) {
        IDevice myDevice = new Device();
        DeviceController controller = new DeviceController(myDevice);
        controller.activateDevice(); // 启动设备
        controller.deactivateDevice(); // 关闭设备
    }
}
```

在这个修正后的例子中，`DeviceController` 现在依赖于 `IDevice` 接口，而不是 `Device` 类的具体实现。这样，如果将来有新的设备实现，如 `Smartphone` 或 `Laptop`，只需要实现 `IDevice` 接口即可，无需修改 `DeviceController` 的代码，系统具有更好的扩展性和灵活性。

通过这种方式，实现了高层模块（`DeviceController`）对低层模块（`Device`）的依赖通过抽象（`IDevice`）来实现，满足了依赖倒置原则。

#### 1.6.5. 接口隔离原则(Interface Segregation Principle, ISP)：

- 客户端不应该依赖它不需要的接口，一个类对另一个类的依赖应该建立在最小的接口上。

##### 示例：

假设我们有一个与车辆相关的系统，该系统有不同的车辆类型，如汽车和摩托车。我们首先定义一个通用的 `Vehicle` 接口，然后提供两个具体的实现。

##### 错误的示例：

```java
interface Vehicle {
    void start();
    void stop();
    void refuel();
    void maintainEngine();
}

class Car implements Vehicle {
    public void start() { System.out.println("Car starts"); }
    public void stop() { System.out.println("Car stops"); }
    public void refuel() { System.out.println("Car refuels"); }
    public void maintainEngine() { System.out.println("Car maintains engine"); }
}

class Motorcycle implements Vehicle {
    public void start() { System.out.println("Motorcycle starts"); }
    public void stop() { System.out.println("Motorcycle stops"); }
    public void refuel() { System.out.println("Motorcycle refuels"); }
    public void maintainEngine() {
        // 错误：摩托车不需要维护引擎
        System.out.println("Motorcycle maintains engine");
    }
}

// 使用Vehicle接口
class VehicleController {
    public void performMaintenance(Vehicle vehicle) {
        vehicle.maintainEngine(); // 错误：并非所有车辆都需要此操作
    }
}
```

在这个例子中，`Vehicle` 接口强迫所有的实现类都实现 `maintainEngine` 方法，即使不是所有的车辆都需要这个操作，如摩托车。这违反了接口隔离原则。

##### 正确示例：

为了遵循ISP，我们可以将 `Vehicle` 接口拆分成更具体的接口，以满足不同车辆类型的需求。

```java
interface Vehicle {
    void start();
    void stop();
    void refuel();
}

interface Maintainable {
    void maintainEngine();
}

class Car implements Vehicle, Maintainable {
    public void start() { System.out.println("Car starts"); }
    public void stop() { System.out.println("Car stops"); }
    public void refuel() { System.out.println("Car refuels"); }
    public void maintainEngine() { System.out.println("Car maintains engine"); }
}

class Motorcycle implements Vehicle {
    public void start() { System.out.println("Motorcycle starts"); }
    public void stop() { System.out.println("Motorcycle stops"); }
    public void refuel() { System.out.println("Motorcycle refuels"); }
    // 不实现maintainEngine，因为摩托车不需要此操作
}

// 使用接口
class VehicleController {
    public void performMaintenance(Maintainable maintainable) {
        maintainable.maintainEngine();
    }
    
    public void drive(Vehicle vehicle) {
        vehicle.start();
        // 执行其他与车辆行驶相关的操作
        vehicle.stop();
    }
}
```

在这个修正后的例子中，我们将 `maintainEngine` 方法移动到了一个新的 `Maintainable` 接口中。现在，只有那些需要维护引擎的车辆（如汽车）才实现 `Maintainable` 接口。`Motorcycle` 类只实现 `Vehicle` 接口，因为它不需要维护引擎。`VehicleController` 类现在根据需要调用不同的接口，这样做既满足了接口隔离原则，也使得代码更加清晰和灵活。

#### 1.6.6. 合成复用原则(Composite Reuse Principle, CRP)：

- 尽量使用对象的组合/聚合，而不是通过继承来达到复用的目的。

##### 示例：

假设我们有一个关于图形界面组件的系统，包括按钮（Button）和文本框（TextBox）等组件，以及一个容器（Container）类，该类可以包含多个组件。

##### 错误示例(过度依赖继承)：

```java
class Component {
    public void render() {
        // 渲染组件的代码
    }
}

class Button extends Component {
    // 按钮特有的代码
}

class TextBox extends Component {
    // 文本框特有的代码
}

// 容器类，依赖于继承
class Container extends Component {
    private List<Component> components;

    public void addComponent(Component component) {
        components.add(component);
    }

    public void render() {
        for (Component component : components) {
            component.render();
        }
        // 渲染容器的代码
    }
}
```

在这个例子中，`Container` 类通过继承 `Component` 类来实现组合。这种方法在某些情况下是可行的，但它过度依赖继承，限制了容器的灵活性。

##### 正确的示例（使用组合）:

为了遵循合成复用原则，我们可以通过组合而不是继承来实现相同的功能。

```java
interface Component {
    void render();
}

class Button implements Component {
    // 按钮特有的代码
    public void render() {
        System.out.println("Rendering Button");
    }
}

class TextBox implements Component {
    // 文本框特有的代码
    public void render() {
        System.out.println("Rendering TextBox");
    }
}

// 容器类，使用组合
class Container {
    private List<Component> components;

    public void addComponent(Component component) {
        components.add(component);
    }

    public void render() {
        for (Component component : components) {
            component.render();
        }
        // 渲染容器的代码
    }
}
```

在这个修正后的例子中，`Container` 类不再继承自 `Component`，而是通过一个 `Component` 对象的列表来持有这些组件。这样，`Container` 就可以包含任何实现了 `Component` 接口的对象，而不需要它们继承自特定的类。这使得系统更加灵活，易于扩展。

##### 总结：

合成复用原则鼓励使用对象组合来实现代码复用，而不是仅仅依赖于继承。组合提供了更大的灵活性，因为它允许系统通过组合不同的对象来扩展功能，而不需要修改现有类。此外，它还有助于降低类之间的耦合度，使得每个类都可以独立于其他类进行开发和维护。

#### 1.6.7. 迪米特法则(Law of Demeter, LoD)：

- 一个对象应该对其他对象有最少的了解，只与它的直接朋友通信，不与“陌生人”说话。在迪米特法则中，“直接朋友”是指以下两类对象：
  1. 当前对象本身（self）
  2. 以参数形式传入当前对象方法的输入对象（input）
  3. 当前对象的成员对象（current object's own fields）
  4. 当前对象创建的对象（created by current object）
  5. 与当前对象同一个集合或数组中的其他对象（siblings）

##### 示例：

假设我们有一个公司（Company）类，一个部门（Department）类，和一个员工（Employee）类。员工属于某个部门，部门属于某个公司。

##### 错误示例：

```java
class Company {
    private List<Department> departments;

    public Department getDepartment(String departmentName) {
        for (Department department : departments) {
            if (department.getName().equals(departmentName)) {
                return department;
            }
        }
        return null;
    }
}

class Department {
    private String name;
    private List<Employee> employees;

    public Employee getEmployee(String employeeName) {
        for (Employee employee : employees) {
            if (employee.getName().equals(employeeName)) {
                return employee;
            }
        }
        return null;
    }
}

class Employee {
    private String name;
    // 其他属性和方法...
}

public class Main {
    public static void main(String[] args) {
        Company company = new Company();
        Department salesDept = company.getDepartment("Sales");
        Employee employee = salesDept.getEmployee("John Doe");
        // 现在我们可以访问employee对象
    }
}
```

在这个例子中，`Company` 类直接与 `Department` 类通信，`Department` 类又直接与 `Employee` 类通信。这违反了迪米特法则，因为 `Company` 对象应该知道的只是 `Department`，而不应该知道 `Employee` 的任何信息。

##### 正确示例：

为了遵循迪米特法则，我们可以修改 `Department` 类，使其不直接将 `Employee` 对象暴露给 `Company` 类。

```java
class Company {
    private List<Department> departments;

    public Department getDepartment(String departmentName) {
        // ... 省略其他代码
    }
}

class Department {
    private String name;
    // 不再直接存储Employee对象
    public Employee getEmployee(String employeeName) {
        // ... 省略其他代码
    }
    
    // 新增的方法，用于与Employee交互
    public void performEvaluation(String employeeName) {
        Employee employee = getEmployee(employeeName);
        if (employee != null) {
            // 执行评估逻辑
        }
    }
}

class Employee {
    private String name;
    // 其他属性和方法...
    
    // 评价员工的方法
    public void beEvaluated() {
        // 被评估时执行的逻辑
    }
}

public class Main {
    public static void main(String[] args) {
        Company company = new Company();
        Department salesDept = company.getDepartment("Sales");
        salesDept.performEvaluation("John Doe"); // 部门执行对员工的评估
    }
}
```

在这个修正后的例子中，`Company` 类只与 `Department` 类通信，而 `Department` 类负责与 `Employee` 类通信。`Company` 类不直接与 `Employee` 类交互。此外，`Department` 类新增了 `performEvaluation` 方法，用于封装与 `Employee` 相关的操作，这样就降低了 `Company` 类与 `Employee` 类之间的耦合度，符合迪米特法则。

通过这种方式，每个类只需要了解与它直接相关的类，从而降低了类之间的耦合度，提高了系统的可维护性和可扩展性。

## 2.创建者模式

### 2.1.单例模式

单例模式（Singleton Pattern）是一种常用的软件设计模式，其核心思想是确保一个类只有一个实例，并提供一个全局访问点。单例模式常用于管理共享资源，如配置信息、线程池、缓存等。

#### 单例模式的要点：

1. **唯一性**：确保一个类只有一个实例。
2. **全局访问点**：提供一个全局方法来访问这个唯一的实例。
3. **线程安全**：在多线程环境中，需要确保单例的实现是线程安全的。

#### 示例：

##### 懒汉式（线程不安全）：

```java
public class Singleton {
    // 私有构造函数，防止外部通过new创建实例
    private Singleton() {}

    // 静态私有实例
    private static Singleton instance;

    // 公有的静态方法，供外部访问实例
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 懒汉式（线程安全）：

```java
public class Singleton {
    private Singleton() {}

    private static Singleton instance;

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

在这个线程安全的实现中，我们使用了 `synchronized` 关键字来确保 `getInstance()` 方法是线程安全的。但是，这会影响性能，因为每次调用 `getInstance()` 时都会进行同步。

#### 饿汉式：

```java
public class Singleton {
    // 私有构造函数
    private Singleton() {}

    // 静态公有实例，在类加载时就已创建实例
    public static final Singleton INSTANCE = new Singleton();

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

饿汉式在类加载时就创建了实例，因此是线程安全的，并且简单。但是，它可能导致不必要的对象提前创建，浪费资源。

#### 双重检查锁定（Double-Checked Locking，DCL）：

```java
public class Singleton {
    private volatile static Singleton instance;

    public Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

DCL是一种在多线程环境下实现线程安全的单例模式的方法。它首先检查实例是否已创建，如果没有，则进行同步。在同步块中再次检查实例是否已创建，以确保只有一个实例被创建。使用 `volatile` 关键字可以防止指令重排，进一步提高线程安全性。

##### 注意事项：

- 单例模式并不总是必要的，有时它会导致代码难以测试和维护。
- 在某些特定情况下，如需要全局状态管理时，单例模式是有用的。
- 单例模式的实现需要考虑线程安全、性能和序列化等问题。

### 2.2.原型模式

用于通过复制现有对象来创建新对象，而不是通过新建一个实例的方式。这种模式特别适用于对象初始化成本较高或者初始化较复杂的情况。

#### 原型模式的要点：

1. **克隆过程**：通过复制一个已有对象来生成新的实例，而不是通过新建一个实例。
2. **深拷贝与浅拷贝**：
   - **浅拷贝**：创建对象的克隆时，只复制对象本身，而不复制对象引用的其他对象。
   - **深拷贝**：创建对象的克隆时，递归复制对象以及对象引用的所有对象。
3. **克隆接口**：通常实现一个克隆接口，如 `Cloneable` 接口，提供克隆自身的方法。

#### 示例：

Java 提供了一个 `Cloneable` 接口，允许一个类创建一个自己的副本。下面是一个简单的原型模式示例：

```java
// 定义一个可克隆的类，实现Cloneable接口
class Prototype implements Cloneable {
    private int id;

    public Prototype(int id) {
        this.id = id;
    }

    // 实现clone方法，创建对象的副本
    @Override
    protected Object clone() throws CloneNotSupportedException {
        try {
            return super.clone(); // 调用Object的clone方法
        } catch (CloneNotSupportedException e) {
            // 通常不会发生，因为实现了Cloneable接口
            throw new AssertionError();
        }
    }
}

public class PrototypePatternDemo {
    public static void main(String[] args) throws CloneNotSupportedException {
        Prototype original = new Prototype(1);
        Prototype cloned = original.clone();

        System.out.println("Original ID: " + original.id);
        System.out.println("Cloned ID: " + cloned.id);
    }
}
```

在这个示例中，`Prototype` 类实现了 `Cloneable` 接口，并重写了 `clone()` 方法以提供创建对象副本的能力。在 `main` 方法中，我们首先创建了一个 `Prototype` 对象，然后克隆它，得到了一个新的对象，这个新对象是原始对象的一个副本。

#### 注意事项：

- **性能**：原型模式可以减少创建对象的开销，特别是当创建对象需要大量资源时。
- **深拷贝与浅拷贝**：在实现克隆时，需要考虑是使用深拷贝还是浅拷贝，这取决于对象的使用场景。
- **线程安全**：原型模式本身不涉及线程安全问题，但如果克隆的对象包含对共享资源的引用，需要考虑线程安全问题。

原型模式在创建新对象时不需要知道对象的具体类型，这使得它可以在运行时动态地创建不同类型的对象，提高了系统的灵活性和可扩展性。

### 2.3.抽象工厂模式

它提供了一种方式，可以生成一系列相关或相互依赖的对象，而不需要指定它们的具体类。这种模式通常用于当需要创建一组对象，而这些对象的创建逻辑满足以下条件时：

1. **家族对象**：对象是相互关联或相互依赖的，它们通常属于同一个产品系列。
2. **抽象接口**：存在一个抽象接口，定义了创建对象的方法，而不指定具体类。

抽象工厂模式的目的是允许系统在不指定具体类的情况下，使用特定的工厂实现来创建一组相关对象。

#### 抽象工厂模式的结构：

- **AbstractFactory**：定义了一个创建产品的接口。
- **ConcreteFactory**：具体的工厂实现，它继承自抽象工厂，并具体化了创建产品的方法。
- **AbstractProduct**：定义了产品的接口。
- **ConcreteProduct**：具体的产品实现，由具体的工厂实例创建。

#### 示例：

假设我们有一个电子产品系列，包括笔记本电脑和智能手机，每种产品都有不同的品牌。

```java
// 抽象产品：电子产品
interface ElectronicProduct {
    void use();
}

// 具体产品：Dell 笔记本电脑
class DellLaptop implements ElectronicProduct {
    public void use() {
        System.out.println("Using Dell Laptop");
    }
}

// 具体产品：Apple 智能手机
class ApplePhone implements ElectronicProduct {
    public void use() {
        System.out.println("Using Apple Phone");
    }

    // 其他品牌和类型的电子产品...
}

// 抽象工厂：电子商品工厂
interface ElectronicsFactory {
    ElectronicProduct createProduct(String type);
}

// 具体工厂：Dell 工厂
class DellFactory implements ElectronicsFactory {
    public ElectronicProduct createProduct(String type) {
        if ("laptop".equals(type)) {
            return new DellLaptop();
        }
        // 可以添加更多Dell产品
        return null;
    }
}

// 具体工厂：Apple 工厂
class AppleFactory implements ElectronicsFactory {
    public ElectronicProduct createProduct(String type) {
        if ("phone".equals(type)) {
            return new ApplePhone();
        }
        // 可以添加更多Apple产品
        return null;
    }
}

public class AbstractFactoryDemo {
    public static void main(String[] args) {
        ElectronicsFactory dellFactory = new DellFactory();
        ElectronicProduct dellProduct = dellFactory.createProduct("laptop");
        dellProduct.use(); // 输出: Using Dell Laptop

        ElectronicsFactory appleFactory = new AppleFactory();
        ElectronicProduct appleProduct = appleFactory.createProduct("phone");
        appleProduct.use(); // 输出: Using Apple Phone
    }
}
```

在这个示例中，我们定义了电子产品的接口 `ElectronicProduct` 和两个具体的产品实现 `DellLaptop` 和 `ApplePhone`。我们还定义了 `ElectronicsFactory` 抽象工厂接口，以及两个具体的工厂实现 `DellFactory` 和 `AppleFactory`。

客户端代码通过具体的工厂来创建产品，而不需要知道具体的产品类。当需要添加新的品牌或产品类型时，只需添加相应的工厂和产品实现即可，无需修改现有代码，符合开闭原则。

#### 注意事项：

- **扩展性**：抽象工厂模式适合当产品族较大且相互依赖时使用，它能够提供很好的扩展性。
- **耦合度**：通过抽象工厂模式，可以减少客户端代码与具体产品实现的耦合度。
- **复杂性**：抽象工厂模式可能会使系统更加复杂，因为每增加一个产品类别，都需要在所有工厂类中添加相应的创建方法。

抽象工厂模式是一种高层次的设计模式，它关注的是对象的创建，而不是对象的行为。在使用抽象工厂模式时，需要仔细考虑系统的需求和结构，以确保它适合解决当前面临的问题。

### 2.4.建造者模式

用于将复杂对象的构建与其表示分离开来。这样，同一个构建过程可以创建不同的表示，而且构建逻辑可以独立于表示对象。

建造者模式特别适用于以下情况：

1. 当创建复杂对象的算法应该独立于代表操作的实现，并且由用户提供。
2. 当需要创建的复杂对象的构建过程允许灵活地修改，而不会影响其表示。

#### 建造者模式的结构：

- **Builder**：一个接口，用于创建一个产品的各个部件。
- **ConcreteBuilder**：实现Builder接口，具体构建复杂对象的各个部件，并提供返回最终产品的接口。
- **Product**：一个类，表示被构建的复杂对象。
- **Director**：一个指挥者类，它使用Builder接口来构建产品，不依赖于具体Builder实现。

#### 示例：

假设我们有一个用于创建汉堡的系统，汉堡由多个部件组成，如面包、肉饼、生菜等。

```java
// 产品类：汉堡
class Burger {
    private String bread;
    private String meat;
    private String lettuce;
    
    // Builder模式允许我们隐藏这些setter方法
    public Burger(String bread, String meat, String lettuce) {
        this.bread = bread;
        this.meat = meat;
        this.lettuce = lettuce;
    }
    
    // 一个方法，用于展示汉堡的构造
    public void show() {
        System.out.println("Burger with " + bread + ", " + meat + ", and " + lettuce);
    }
}

// 建造者接口
interface BurgerBuilder {
    void setBread(String bread);
    void setMeat(String meat);
    void setLettuce(String lettuce);
    Burger getBurger();
}

// 具体建造者
class DeluxeBurgerBuilder implements BurgerBuilder {
    private Burger burger;
    
    public DeluxeBurgerBuilder() {
        burger = new Burger("", "", "");
    }
    
    public void setBread(String bread) {
        burger.bread = bread;
    }
    
    public void setMeat(String meat) {
        burger.meat = meat;
    }
    
    public void setLettuce(String lettuce) {
        burger.lettuce = lettuce;
    }
    
    public Burger getBurger() {
        return burger;
    }
}

// 指挥者
class BurgerDirector {
    public void constructBurger(BurgerBuilder builder) {
        builder.setBread("Deluxe Bread");
        builder.setMeat("Gourmet Meat");
        builder.setLettuce("Fresh Lettuce");
    }
}

public class BuilderPatternDemo {
    public static void main(String[] args) {
        BurgerDirector director = new BurgerDirector();
        BurgerBuilder builder = new DeluxeBurgerBuilder();
        
        director.constructBurger(builder);
        Burger burger = builder.getBurger();
        burger.show(); // 输出: Burger with Deluxe Bread, Gourmet Meat, and Fresh Lettuce
    }
}
```

在这个示例中，`Burger` 类表示我们要构建的复杂对象。`BurgerBuilder` 接口定义了创建 `Burger` 对象的方法，而 `DeluxeBurgerBuilder` 是一个具体的建造者，实现了 `BurgerBuilder` 接口，并定义了如何构建一个豪华汉堡。`BurgerDirector` 类是一个指挥者，它使用建造者对象来构建产品。

建造者模式允许我们通过 `Director` 类来控制对象的创建过程，同时将对象的构建过程和表示分离，使得我们可以独立地改变构建过程或表示，而不会影响到彼此。

#### 注意事项：

- **解耦**：建造者模式将对象的创建过程和表示分离，有助于降低系统的耦合度。
- **复杂性**：如果产品对象非常简单，使用建造者模式可能会增加不必要的复杂性。
- **灵活性**：建造者模式允许用户通过定义新的建造者类来定制产品，而无需修改现有代码。

## 3.结构型模式

主要关注于如何将对象和类组装成更大的结构，以及如何定义它们之间的关联关系。结构型模式描述了如何将类或对象组合成更大的结构以便于合理地划分责任和实现易于管理的设计。

### 3.1.以下是一些常见的结构型设计模式：

#### 3.1.1.适配器模式（Adapter Pattern）：

用于使不兼容的接口协同工作，它通常用于实现类之间的兼容处理。适配器模式可以是对象适配器，也可以是类适配器，但它们的核心思想是相同的：提供一个中间层来解决接口不兼容的问题。

##### 适配器模式的组成：

- **Target**：目标接口，定义客户端使用的特定领域相关的接口。
- **Adaptee**：被适配者，一个已经存在的类，需要适配的类，它不符合目标接口。
- **Adapter**：适配器，通过在内部包装一个Adaptee对象，把源接口转换成目标接口。

##### 示例：对象适配器

假设我们有一个星形图形接口 `StarShape` 和一个矩形图形接口 `Rectangle`，现在我们想要使用矩形来模拟星形图形。

- 星形图形接口和实现：

  ```java
  interface StarShape {
      void draw();
  }
  
  class Star implements StarShape {
      public void draw() {
          System.out.println("Drawing a star");
      }
  }
  ```

- 矩形图形接口和实现：

  ```java
  interface Rectangle {
      void printArea();
  }
  
  class RectangleImpl implements Rectangle {
      private int width;
      private int height;
  
      public RectangleImpl(int width, int height) {
          this.width = width;
          this.height = height;
      }
  
      public void printArea() {
          System.out.println("Area of rectangle: " + (width * height));
      }
  }
  ```

- 适配器实现：

  ```java
  class RectangleAdapter implements StarShape {
      private Rectangle rectangle;
  
      public RectangleAdapter(Rectangle rectangle) {
          this.rectangle = rectangle;
      }
  
      public void draw() {
          // 适配过程：将矩形的面积打印适配成星形的绘制
          rectangle.printArea();
      }
  }
  ```

- 客户端代码：

  ```java
  public class AdapterPatternDemo {
      public static void main(String[] args) {
          Star star = new Star();
          star.draw(); // 正常使用星形图形
  
          Rectangle rectangle = new RectangleImpl(5, 10);
          StarShape rectangleAsStar = new RectangleAdapter(rectangle);
          rectangleAsStar.draw(); // 使用矩形模拟星形图形
      }
  }
  ```

在这个示例中，`RectangleAdapter` 是适配器，它实现了 `StarShape` 接口，内部持有一个 `Rectangle` 对象。`draw()` 方法调用了 `Rectangle` 对象的 `printArea()` 方法，实现了适配过程。

通过适配器模式，我们可以使用 `Rectangle` 类来模拟 `StarShape` 接口的行为，即使它们的接口不兼容。这样，我们就可以复用现有的 `Rectangle` 类，而不需要修改它的代码。

##### 注意事项：

- **灵活性**：适配器模式提高了系统的灵活性，通过引入一个中间层来解决接口不兼容的问题。
- **代码可读性**：适配器模式可能会使代码的可读性降低，特别是在过度使用适配器时。
- **性能**：适配器模式可能会带来一些性能开销，因为适配过程可能涉及到额外的间接调用。

#### 3.1.2.桥接模式（Bridge Pattern）：

用于将抽象部分与其实现部分分离，使它们可以独立地变化。桥接模式主要解决的是对象间的多维度变化问题，通过使用抽象作为桥接，将变化维度隔离开来。

### 桥接模式的组成：

- **Abstraction**：抽象类，定义了客户端使用的具体业务逻辑，维持一个指向Implementor接口的引用。
- **Implementor**：实现类接口，定义了抽象类的实现细节。
- **ConcreteAbstraction**：具体抽象类，实现Abstraction接口，并通过其Implementor对象来实现具体业务逻辑。
- **ConcreteImplementor**：具体实现类，实现了Implementor接口，提供具体的实现细节。

##### 示例：

假设我们有一个图形系统，其中包含形状（Shape）的抽象定义和颜色（Color）的抽象定义。形状和颜色都可以独立变化，我们希望建立一个桥接结构，使得形状和颜色可以独立扩展。

- 颜色接口和实现：

  ```java
  interface Color {
      void applyColor();
  }
  
  class RedColor implements Color {
      public void applyColor() {
          System.out.println("Applying Red color");
      }
  }
  
  class BlueColor implements Color {
      public void applyColor() {
          System.out.println("Applying Blue color");
      }
  }
  ```

- 形状接口和实现

  ```java
  abstract class Shape {
      protected Color color; // 引用颜色接口
  
      public Shape(Color color) {
          this.color = color;
      }
  
      public abstract void draw();
  }
  
  class Circle extends Shape {
      public Circle(Color color) {
          super(color);
      }
  
      public void draw() {
          color.applyColor();
          System.out.println("Inside a Circle");
      }
  }
  
  class Square extends Shape {
      public Square(Color color) {
          super(color);
      }
  
      public void draw() {
          color.applyColor();
          System.out.println("Inside a Square");
      }
  }
  ```

- 客户端代码

  ```java
  public class BridgePatternDemo {
      public static void main(String[] args) {
          Color red = new RedColor();
          Shape circle = new Circle(red);
          circle.draw(); // 输出: Applying Red color, Inside a Circle
  
          Color blue = new BlueColor();
          Shape square = new Square(blue);
          square.draw(); // 输出: Applying Blue color, Inside a Square
      }
  }
  ```

在这个示例中，`Shape` 是抽象类，它包含一个`Color`对象的引用。`Circle` 和 `Square` 是具体抽象类，它们实现了 `Shape` 接口，并定义了如何绘制形状。`Color` 是实现类接口，而 `RedColor` 和 `BlueColor` 是具体实现类。

客户端代码通过创建形状对象和颜色对象，并将颜色对象传递给形状对象的构造函数，来实现形状和颜色的组合。这样，形状和颜色就可以独立变化，满足桥接模式的需要。

##### 注意事项：

- **解耦**：桥接模式使抽象和实现分离，从而减少了它们之间的耦合。
- **扩展性**：桥接模式提高了系统的可扩展性，可以独立扩展形状和颜色。
- **复杂性**：桥接模式可能会使系统变得更加复杂，因为需要维护两个维度的类层次。

#### 3.1.3.组合模式（Composite Pattern）：

它允许将对象组合成树状结构以表示“部分-整体”的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。这种一致性允许客户端以统一的方式处理单个对象和组合对象，使得客户端不需要关心面对的是一个单独的叶节点还是一个组合节点。

##### 组合模式的组成：

- **Component**：组件接口，定义了组合中所有对象的一致操作方式。
- **Leaf**：叶节点，实现Component接口，不包含子节点，表示组合中的末端对象。
- **Composite**：组合节点，也实现Component接口，并包含子节点，子节点可以是Leaf或其他Composite对象。
- **Client**：客户端，通过Component接口与组合中的对象交互。

##### 示例：

假设我们有一个文件系统，其中包含文件（File）和文件夹（Folder），文件夹可以包含文件和其他文件夹。

- 组件接口和叶节点：

  ```java
  // 组件接口
  interface FileComponent {
      void operation();
  }
  
  // 文件（叶节点）
  class File implements FileComponent {
      private String name;
  
      public File(String name) {
          this.name = name;
      }
  
      public void operation() {
          System.out.println("File: " + name);
      }
  }
  ```

- 组合节点：

  ```java
  // 文件夹（组合节点）
  class Folder implements FileComponent {
      private List<FileComponent> children = new ArrayList<>();
  
      private String name;
  
      public Folder(String name) {
          this.name = name;
      }
  
      public void add(FileComponent child) {
          children.add(child);
      }
  
      public void remove(FileComponent child) {
          children.remove(child);
      }
  
      public List<FileComponent> getChildren() {
          return children;
      }
  
      public void operation() {
          System.out.println("Folder: " + name);
          for (FileComponent child : children) {
              child.operation();
          }
      }
  }
  ```

- 客户端代码

  ```java
  public class CompositePatternDemo {
      public static void main(String[] args) {
          // 创建文件系统
          Folder root = new Folder("root");
          Folder photos = new Folder("photos");
          Folder docs = new Folder("docs");
          File image1 = new File("image1.png");
          File image2 = new File("image2.png");
          File doc1 = new File("doc1.txt");
  
          // 构建文件系统结构
          root.add(photos);
          root.add(docs);
          photos.add(image1);
          photos.add(image2);
          docs.add(doc1);
  
          // 执行操作
          root.operation();
      }
  }
  ```

在这个示例中，`FileComponent` 是组件接口，定义了操作（`operation`）。`File` 是叶节点，实现了 `FileComponent` 接口。`Folder` 是组合节点，也实现了 `FileComponent` 接口，并包含了一系列 `FileComponent` 对象（可以是 `File` 或 `Folder`）。

客户端代码通过构建文件夹和文件的层次结构，并调用根节点的 `operation` 方法来遍历整个文件系统结构，执行操作。由于客户端通过 `FileComponent` 接口与对象交互，因此客户端不需要关心对象是文件还是文件夹。

##### 注意事项：

- **透明性**：在组合模式中，叶节点和组合节点对客户端来说应该是透明的，即客户端可以不加区分地使用它们。
- **安全性**：在某些情况下，可能需要确保组合节点的特定行为不会对叶节点产生影响，这可能需要在组合节点中实现额外的保护逻辑。
- **递归遍历**：在组合模式中，经常需要递归地遍历组合结构，这在设计时需要考虑性能和避免无限递归的问题。

#### 3.1.4.装饰器模式（Decorator Pattern）：

它允许在不修改现有类代码的情况下，通过创建一个包装类来给对象添加新的功能或行为。装饰器模式提供了一种灵活的、可扩展的方式来扩展对象的功能。

##### 装饰器模式的组成：

- **Component**：定义了被装饰对象的接口，是装饰器和被装饰对象共有的抽象接口。
- **ConcreteComponent**：实现了Component接口的具体类，即被装饰的具体对象。
- **Decorator**：装饰器抽象类，持有一个Component对象，并实现与Component接口一致的操作。
- **ConcreteDecorator**：具体的装饰器类，继承自Decorator类，通过在Component对象的操作前后添加额外的行为来扩展功能。

##### 示例：

假设我们有一个简单的咖啡店，提供不同类型的咖啡，每种咖啡都有一个基本价格。我们希望在不修改现有咖啡类的情况下，通过添加不同的调料来改变咖啡的价格和描述。

- 咖啡（Component）

  ```java
  interface Coffee {
      double cost();
      String getDesc();
  }
  ```

- 具体咖啡（ConcreteComponent）

  ```java
  class SimpleCoffee implements Coffee {
      public double cost() {
          return 10;
      }
  
      public String getDesc() {
          return "Simple Coffee";
      }
  }
  ```

- 装饰器（Decorator）

  ```java
  abstract class CoffeeDecorator implements Coffee {
      protected Coffee coffee;
  
      public CoffeeDecorator(Coffee coffee) {
          this.coffee = coffee;
      }
  
      @Override
      public double cost() {
          return coffee.cost();
      }
  
      @Override
      public String getDesc() {
          return coffee.getDesc();
      }
  }
  ```

- 具体装饰器（ConcreteDecorator）

  ```java
  class MilkCoffee extends CoffeeDecorator {
      public MilkCoffee(Coffee coffee) {
          super(coffee);
      }
  
      @Override
      public double cost() {
          return super.cost() + 2; // 牛奶咖啡比普通咖啡贵2元
      }
  
      @Override
      public String getDesc() {
          return "Milk added to " + coffee.getDesc();
      }
  }
  
  class WhipCoffee extends CoffeeDecorator {
      public WhipCoffee(Coffee coffee) {
          super(coffee);
      }
  
      @Override
      public double cost() {
          return super.cost() + 3; // 奶油咖啡比普通咖啡贵3元
      }
  
      @Override
      public String getDesc() {
          return "Whip added to " + coffee.getDesc();
      }
  }
  ```

- 客户端代码

  ```java
  public class DecoratorPatternDemo {
      public static void main(String[] args) {
          Coffee simpleCoffee = new SimpleCoffee();
          System.out.println(simpleCoffee.getDesc() + " $" + simpleCoffee.cost());
  
          Coffee milkCoffee = new MilkCoffee(simpleCoffee);
          System.out.println(milkCoffee.getDesc() + " $" + milkCoffee.cost());
  
          Coffee whipMilkCoffee = new WhipCoffee(milkCoffee);
          System.out.println(whipMilkCoffee.getDesc() + " $" + whipMilkCoffee.cost());
      }
  }
  ```

在这个示例中，`Coffee` 是组件接口，定义了咖啡的描述和价格。`SimpleCoffee` 是具体组件，实现了 `Coffee` 接口。`CoffeeDecorator` 是装饰器抽象类，它持有一个 `Coffee` 对象，并转发 `cost` 和 `getDesc` 方法。`MilkCoffee` 和 `WhipCoffee` 是具体装饰器，它们在 `cost` 方法中添加了额外的费用，并在 `getDesc` 方法中添加了描述。

客户端代码通过创建一个简单的咖啡对象，然后逐层添加装饰器来创建不同口味的咖啡，并打印出每种咖啡的描述和价格。

##### 注意事项：

- **灵活性**：装饰器模式允许在运行时动态地添加或去除装饰器，提高了系统的灵活性。
- **复杂性**：过度使用装饰器模式可能会导致系统变得复杂，难以理解装饰器的完整效果。
- **性能**：装饰器模式可能会带来一些性能开销，因为每次装饰器调用都会涉及额外的包装对象。

#### 3.1.5.外观模式（Facade Pattern）：

它为一个复杂系统的子系统提供一组统一的接口。外观模式定义了一个高层接口，这个接口使得这个子系统更加容易使用。

##### 外观模式的组成：

- **Facade**：外观类，它为子系统提供简单的接口。
- **SubSystem**：子系统类，实现系统的功能，外观类会与这些子系统对象交互。

##### 示例：

假设我们有一个家庭影院系统，它包括投影仪（Projector）、音响系统（AudioSystem）和视频播放设备（Player）。我们希望使用一个简单的操作来启动家庭影院系统。

- 子系统类：

  ```java
  class Projector {
      public void on() {
          System.out.println("Projector on");
      }
  
      public void off() {
          System.out.println("Projector off");
      }
  }
  
  class AudioSystem {
      public void on() {
          System.out.println("AudioSystem on");
      }
  
      public void off() {
          System.out.println("AudioSystem off");
      }
  }
  
  class Player {
      public void on() {
          System.out.println("Player on");
      }
  
      public void off() {
          System.out.println("Player off");
      }
  
      public void play(String movie) {
          System.out.println("Playing " + movie);
      }
  }
  ```

- 外观类：

  ```java
  class HomeTheaterFacade {
      private Projector projector;
      private AudioSystem audioSystem;
      private Player player;
  
      public HomeTheaterFacade() {
          projector = new Projector();
          audioSystem = new AudioSystem();
          player = new Player();
      }
  
      public void watchMovie(String movie) {
          projector.on();
          audioSystem.on();
          player.on();
          player.play(movie);
      }
  
      public void endMovie() {
          player.off();
          audioSystem.off();
          projector.off();
      }
  }
  ```

- 客户端代码：

  ```java
  public class FacadePatternDemo {
      public static void main(String[] args) {
          HomeTheaterFacade homeTheater = new HomeTheaterFacade();
          homeTheater.watchMovie("Interstellar");
          // 电影结束
          homeTheater.endMovie();
      }
  }
  ```

在这个示例中，`Projector`、`AudioSystem` 和 `Player` 是子系统类，它们分别实现了家庭影院系统中的一部分功能。`HomeTheaterFacade` 是外观类，它提供了 `watchMovie` 和 `endMovie` 两个方法，封装了启动和关闭家庭影院系统的复杂操作。

客户端代码通过 `HomeTheaterFacade` 类来与家庭影院系统交互，而不需要了解投影仪、音响系统和播放设备的复杂细节。

##### 注意事项：

- **简化接口**：外观模式通过提供一个简化的接口，使得客户端代码更加简洁。
- **松耦合**：外观模式减少了客户端与子系统之间的耦合度，提高了子系统的独立性。
- **灵活性**：外观模式可以在不影响客户端的情况下，更换子系统的实现。

#### 3.1.6.享元模式（Flyweight Pattern）：

它主要用于减少创建对象的数量，从而提高程序性能和减少内存消耗。享元模式通过共享对象来实现这一目的，确保共享对象是细粒度的，并且被多客户端使用。

##### 享元模式的组成：

- **Flyweight**：享元接口，定义了享元对象的接口。
- **ConcreteFlyweight**：具体享元类，实现了Flyweight接口，并包含内部状态。
- **FlyweightFactory**：享元工厂类，负责创建和管理Flyweight对象，确保Flyweight对象可以被共享。
- **UnsharedConcreteFlyweight**：非共享具体享元类（如果需要），它不共享Flyweight对象。

##### 示例：

假设我们有一个表示连接字符串的系统，每个连接字符串可能被多次使用。为了节省内存，我们希望共享相同的字符串对象。

- 享元接口和具体实现：

  ```java
  interface StringFlyweight {
      void printString(String exterior);
  }
  
  class ConcreteStringFlyweight implements StringFlyweight {
      private String intrinsicState; // 内部状态
  
      public ConcreteStringFlyweight(String intrinsicState) {
          this.intrinsicState = intrinsicState;
      }
  
      public void printString(String exterior) {
          System.out.println(intrinsicState + " " + exterior);
      }
  }
  ```

- 享元工厂

  ```java
  class StringFlyweightFactory {
      private static Map<String, StringFlyweight> flyweights = new HashMap<>();
  
      public static StringFlyweight getFlyweight(String key) {
          if (!flyweights.containsKey(key)) {
              flyweights.put(key, new ConcreteStringFlyweight(key));
          }
          return flyweights.get(key);
      }
  }
  ```

- 客户端代码

  ```java
  public class FlyweightPatternDemo {
      public static void main(String[] args) {
          // 获取享元对象，并使用外部状态
          StringFlyweight sf1 = StringFlyweightFactory.getFlyweight("Hello");
          sf1.printString("World!"); // 输出: Hello World!
  
          StringFlyweight sf2 = StringFlyweightFactory.getFlyweight("Hello");
          sf2.printString("Kimi!"); // 输出: Hello Kimi!
      }
  }
  ```

在这个示例中，`StringFlyweight` 是享元接口，`ConcreteStringFlyweight` 是具体享元类，它包含一个内部状态（字符串）。`StringFlyweightFactory` 是享元工厂类，它管理着享元对象的创建和共享。

客户端代码通过享元工厂获取享元对象，并使用不同的外部状态来打印字符串。由于享元对象是共享的，因此相同的字符串对象被多次利用，减少了内存消耗。

##### 注意事项

- **细粒度**：享元模式适用于对象数量多且对象可以被共享的情况。
- **内部与外部状态**：享元对象的内部状态应该保持不变，而外部状态可以变化，以支持享元对象的多客户端使用。
- **延迟初始化**：享元工厂可以采用懒加载的方式创建享元对象，以提高性能。

#### 3.1.7.代理模式（Proxy Pattern）：

它为其他对象提供一个代理或占位符，以控制对这个对象的访问。代理模式可以在不改变对象的代码的情况下，为对象添加额外的功能，例如延迟初始化、访问控制、日志记录、缓存等。

##### 代理模式的组成：

- **Subject**：主题接口，定义了真实对象和代理对象共有的接口。
- **RealSubject**：真实主题类，实现了主题接口的具体业务逻辑。
- **Proxy**：代理类，也实现了主题接口，并包含对真实主题的引用。代理对象在内部维护了对真实对象的引用，从而可以控制对真实对象的访问。
- **Client**：客户端，通过主题接口与代理对象交互。

##### 示例：

假设我们有一个远程代理服务器，它负责处理对远程对象的访问。

- 主题接口和真实主题：

  ```java
  interface Server {
      void start();
      void stop();
  }
  
  class RealServer implements Server {
      public void start() {
          System.out.println("Real server is starting...");
          // 启动服务器的逻辑
      }
  
      public void stop() {
          System.out.println("Real server is stopping...");
          // 停止服务器的逻辑
      }
  }
  ```

- 代理类：

  ```java
  class ServerProxy implements Server {
      private RealServer realServer;
      private boolean isStarted;
  
      public void start() {
          if (!isStarted) {
              System.out.println("Starting the proxy server...");
              // 代理服务器的启动逻辑
              if (realServer == null) {
                  realServer = new RealServer();
              }
              realServer.start();
              isStarted = true;
          }
      }
  
      public void stop() {
          if (isStarted) {
              realServer.stop();
              System.out.println("Proxy server is stopping...");
              // 代理服务器的停止逻辑
              isStarted = false;
          }
      }
  }
  ```

- 客户端代码：

  ```java
  public class ProxyPatternDemo {
      public static void main(String[] args) {
          Server proxy = new ServerProxy();
          proxy.start();
          // 使用服务器...
          proxy.stop();
      }
  }
  ```

在这个示例中，`Server` 是主题接口，`RealServer` 是实现了主题接口的真实主题类。`ServerProxy` 是代理类，它实现了 `Server` 接口，并在内部维护了对 `RealServer` 的引用。客户端通过代理对象来控制对真实服务器的访问。

##### 注意事项：

- **控制访问**：代理模式可以控制对真实对象的访问，实现延迟初始化、访问控制等功能。
- **性能开销**：代理模式可能会带来一些性能开销，因为每次访问真实对象都需要通过代理对象。
- **透明度**：理想情况下，代理模式应该是透明的，即客户端不需要知道它是在与代理对象还是真实对象交互。

#### 3.1.8.私有类模式（Protected Variant Pattern）：

它并不是一个传统意义上的设计模式，它更多的是一种设计技巧，用于在继承中保护或隐藏类的某些实现细节，确保类的封装性不被破坏。

##### 私有类模式的核心思想是：

1. **封装性**：确保类的内部实现细节不被外部直接访问。
2. **继承限制**：限制不希望被继承的类的行为，避免恶意或错误的继承导致的潜在问题。
3. **保护变异**：在类的内部实现某些变异，这些变异是受到保护的，外部无法直接访问。

##### 示例：

假设我们有一个银行账户类 `BankAccount`，我们希望保护其内部实现，防止外部直接修改账户余额。

- 原始的 `BankAccount` 类：

  ```java
  class BankAccount {
      protected double balance;
  
      public BankAccount(double balance) {
          this.balance = balance;
      }
  
      // 允许外部直接访问余额
      public double getBalance() {
          return balance;
      }
  
      // 允许外部直接修改余额
      public void setBalance(double balance) {
          this.balance = balance;
      }
  }
  ```

- 应用私有类模式：
  为了保护 `BankAccount` 类的内部实现，我们可以将其变异为私有类，并且提供公共的静态方法来创建和修改账户。

  ```java
  class BankAccount {
      private double balance;
  
      private BankAccount(double balance) {
          this.balance = balance;
      }
  
      // 私有方法，外部无法直接访问
      private void deposit(double amount) {
          if (amount > 0) {
              balance += amount;
          }
      }
  
      // 私有方法，外部无法直接访问
      private void withdraw(double amount) {
          if (amount > 0 && balance >= amount) {
              balance -= amount;
          }
      }
  
      // 公共的静态方法，用于创建账户
      public static BankAccount createAccount(double initialBalance) {
          return new BankAccount(initialBalance);
      }
  
      // 公共的静态方法，用于存款
      public static void deposit(BankAccount account, double amount) {
          account.deposit(amount);
      }
  
      // 公共的静态方法，用于取款
      public static boolean withdraw(BankAccount account, double amount) {
          account.withdraw(amount);
          // 返回操作成功与否的状态
          return account.balance < amount;
      }
  }
  
  public class ProtectedVariantPatternDemo {
      public static void main(String[] args) {
          BankAccount account = BankAccount.createAccount(1000);
          BankAccount.deposit(account, 500);
          boolean success = BankAccount.withdraw(account, 300);
          System.out.println("Withdrawal successful: " + success);
          System.out.println("Current balance: " + account.getBalance());
      }
  }
  ```

在这个示例中，`BankAccount` 类的构造函数和余额操作都被设置为私有，外部代码无法直接创建 `BankAccount` 对象或修改余额。相反，我们提供了公共的静态方法来安全地创建账户、存款和取款。这样，我们就保护了类的内部实现细节，同时提供了一种安全的操作方式。

##### 注意事项：

- **封装性**：私有类模式强调封装性，隐藏内部实现细节，只暴露必要的操作接口。
- **继承限制**：通过限制继承和直接访问，可以防止错误的使用方式，保护类的完整性。
- **易用性**：虽然私有类模式限制了类的直接使用，但通过提供清晰的操作接口，仍然可以保持良好的易用性。

## 4.行为型模式

主要关注对象之间的相互作用以及它们怎样相互分配职责。行为型模式不仅仅关注类和对象是怎么构建的，而且关注它们之间的交流以及职责如何分配。以下是一些常见的行为型设计模式：

### 4.1.责任链模式（Chain of Responsibility）：

它允许将多个处理程序（对象）连接成一条链，每个处理程序都可以对请求进行处理，或者将请求转发给链中的下一个处理程序。这种模式的主要目的是消除请求发送者和接收者之间的耦合关系，让多个处理者有机会处理同一个请求，从而使得系统更加灵活。

#### 责任链模式的主要组成元素：

1. **Handler（处理者）**：定义了一个处理请求的方法，并且持有对下一条链的引用。
2. **ConcreteHandler（具体处理者）**：实现了 `Handler` 接口，并对请求进行处理。每个具体处理者都指向链中的下一个处理者。

#### 示例：

假设我们有一个处理各种购买请求的系统，不同的购买请求需要不同级别的经理进行审批。

- 定义处理者接口：

  ```java
  interface Approver {
      void processRequest(PurchaseRequest request);
  }
  ```

- 具体处理者实现：

  ```java
  class Manager implements Approver {
      private Approver nextApprover;
  
      public void setNextApprover(Approver nextApprover) {
          this.nextApprover = nextApprover;
      }
  
      public void processRequest(PurchaseRequest request) {
          if ("Manager".equals(request.getLevel())) {
              System.out.println("Manager approved request ID: " + request.getId());
          } else {
              if (nextApprover != null) {
                  nextApprover.processRequest(request);
              } else {
                  System.out.println("No approver available for request ID: " + request.getId());
              }
          }
      }
  }
  
  // Director 和 CEO 类似于 Manager，但处理不同的请求级别
  ```

- 购买请求类：

  ```java
  class PurchaseRequest {
      private String id;
      private String level; // 可以是 "Manager", "Director", "CEO"
  
      public PurchaseRequest(String id, String level) {
          this.id = id;
          this.level = level;
      }
  
      // 省略getter和setter方法...
  }
  ```

- 客户端代理：

  ```java
  public class ChainOfResponsibilityPatternDemo {
      public static void main(String[] args) {
          Approver manager = new Manager();
          Approver director = new Director();
          Approver ceo = new CEO();
  
          manager.setNextApprover(director);
          director.setNextApprover(ceo);
  
          // 创建购买请求并提交
          manager.processRequest(new PurchaseRequest("001", "CEO"));
          manager.processRequest(new PurchaseRequest("002", "Director"));
          manager.processRequest(new PurchaseRequest("003", "Manager"));
      }
  }
  ```

在这个示例中，我们定义了三个审批级别：经理（Manager）、主管（Director）和首席执行官（CEO）。每个审批者都实现了 `Approver` 接口，并负责处理特定级别的请求。如果一个审批者无法处理请求，它会将请求转发给链中的下一个审批者。客户端代码通过构建责任链并提交购买请求来启动审批流程。

#### 注意事项：

- **松耦合**：责任链模式通过将请求发送者和请求处理者解耦，提高了系统的灵活性。
- **动态性**：链中的对象可以动态地添加或删除，而无需修改链中现有的对象。
- **请求的传递**：请求可能会在链中传递直到被处理，或者直到链的末端仍未被处理。
- **终止条件**：通常在链的末端设置一个终止条件，以确保请求最终被处理。

### 4.2.命令模式（Command）：

它将一个请求封装为一个对象，从而允许用户使用不同的请求。命令模式也支持可撤销的操作。它通常用于解耦请求的发送者和接收者，将它们之间通过命令对象进行间接的联系。

#### 命令模式的主要组成元素：

1. **Command（命令）**：定义了执行操作的接口。
2. **ConcreteCommand（具体命令）**：Command的一个实现类，它将一个接收者对象绑定到一个动作上。
3. **Client（客户端）**：创建具体的命令对象，并指定需要执行的操作。
4. **Invoker（调用者）**：要求命令对象执行请求。
5. **Receiver（接收者）**：知道如何实施与执行一个请求相关的操作。

#### 示例：

假设我们有一个简单的电灯控制程序，用户可以发送不同的命令来控制电灯的开关。

- 定义命令接口：

  ```java
  interface Command {
      void execute();
  }
  ```

- 具体命令实现：

  ```java
  class LightOnCommand implements Command {
      private Light light;
  
      public LightOnCommand(Light light) {
          this.light = light;
      }
  
      public void execute() {
          light.on();
      }
  }
  
  class LightOffCommand implements Command {
      private Light light;
  
      public LightOffCommand(Light light) {
          this.light = light;
      }
  
      public void execute() {
          light.off();
      }
  }
  ```

- 接收者类：

  ```java
  class Light {
      public void on() {
          System.out.println("The light is on");
      }
  
      public void off() {
          System.out.println("The light is off");
      }
  }
  ```

- 调用者类:

  ```java
  class RemoteControl {
      private Command command;
  
      public void setCommand(Command command) {
          this.command = command;
      }
  
      public void pressButton() {
          command.execute();
      }
  }
  ```

- 客户端代码：

  ```java
  public class CommandPatternDemo {
      public static void main(String[] args) {
          Light light = new Light();
          Command lightOn = new LightOnCommand(light);
          Command lightOff = new LightOffCommand(light);
  
          RemoteControl remoteControl = new RemoteControl();
          remoteControl.setCommand(lightOn);
          remoteControl.pressButton(); // 输出: The light is on
  
          remoteControl.setCommand(lightOff);
          remoteControl.pressButton(); // 输出: The light is off
      }
  }
  ```

在这个示例中，我们定义了一个电灯 `Light` 和两个命令 `LightOnCommand` 和 `LightOffCommand`，它们都实现了 `Command` 接口。`RemoteControl` 是调用者，它持有一个命令对象，并通过调用 `execute()` 方法来执行命令。客户端代码通过创建命令对象和调用者对象，并设置命令，来控制电灯的开关。

#### 注意事项：

- **解耦**：命令模式将调用操作的对象与知道如何执行该操作的对象解耦。
- **扩展性**：可以很容易地扩展新的命令类，而无需修改调用者或接收者。
- **可撤销性**：命令模式允许操作的撤销和重做，通过维护命令历史记录。
- **单一职责**：每个命令类只关心一个请求，符合单一职责原则。

### 4.3.解释器模式（Interpreter）：

用于定义一个语言的语法规则，并建立一个解释器来解释和执行该语言中的句子。这种模式通常应用于解析表达式或指令，如计算表达式、解析SQL语句、解析配置文件等。

#### 解释器模式的主要组成元素：

1. **AbstractExpression（抽象表达式）**：定义了一个抽象的解释接口，通常包含一个 `interpret()` 方法。
2. **TerminalExpression（终结符表达式）**：实现了 `AbstractExpression`，代表语法规则中的最小元素，如数字、变量等。
3. **NonTerminalExpression（非终结符表达式）**：也实现了 `AbstractExpression`，代表由其他表达式组成的复合语法规则。
4. **Context（上下文）**：包含解释器需要的共享数据，如变量存储或解析状态。
5. **Client（客户端）**：构建语法树，并调用解释器。

#### 示例：

假设我们定义了一个简单的表达式语言，用于计算加法和乘法。

- 抽象表达式：

  ```java
  abstract class Expression {
      public abstract int interpret(Context context);
  }
  ```

- 终结符表达式：

  ```java
  class TerminalExpression extends Expression {
      private int number;
  
      public TerminalExpression(int number) {
          this.number = number;
      }
  
      public int interpret(Context context) {
          return number;
      }
  }
  
  class VariableExpression extends Expression {
      private String variable;
  
      public VariableExpression(String variable) {
          this.variable = variable;
      }
  
      public int interpret(Context context) {
          return context.lookupVariable(variable);
      }
  }
  ```

- 非终结符表达式：

  ```java
  class OperationExpression extends Expression {
      private Expression left;
      private Expression right;
      private String operation;
  
      public OperationExpression(Expression left, Expression right, String operation) {
          this.left = left;
          this.right = right;
          this.operation = operation;
      }
  
      public int interpret(Context context) {
          int leftValue = left.interpret(context);
          int rightValue = right.interpret(context);
  
          switch (operation) {
              case "+":
                  return leftValue + rightValue;
              case "*":
                  return leftValue * rightValue;
              default:
                  throw new RuntimeException("Unsupported operation: " + operation);
          }
      }
  }
  ```

- 上下文：

  ```java
  class Context {
      private Map<String, Integer> variables = new HashMap<>();
  
      public int lookupVariable(String name) {
          return variables.get(name);
      }
  
      public void defineVariable(String name, int value) {
          variables.put(name, value);
      }
  }
  ```

- 客户端代码：

  ```java
  public class InterpreterPatternDemo {
      public static void main(String[] args) {
          Context context = new Context();
          context.defineVariable("A", 2);
          context.defineVariable("B", 3);
  
          Expression expression = new OperationExpression(
                  new VariableExpression("A"),
                  new OperationExpression(
                          new TerminalExpression(1),
                          new VariableExpression("B"),
                          "*"
                  ),
                  "+"
          );
  
          int result = expression.interpret(context);
          System.out.println("Result: " + result); // 输出: Result: 7
      }
  }
  ```

在这个示例中，我们定义了一个简单的表达式语言，其中包含终结符表达式 `TerminalExpression` 和 `VariableExpression`，以及非终结符表达式 `OperationExpression`。`Context` 用于存储变量和值。客户端代码通过构建表达式并解释执行来计算结果。

#### 注意事项：

- **复杂性**：解释器模式可以变得相当复杂，特别是当语法规则变得复杂时。
- **性能**：对于大型或复杂的表达式，解释器模式可能不是性能最优的选择。
- **可扩展性**：通过扩展抽象表达式和上下文，可以轻松添加新的语法规则。

### 4.4.迭代器模式（Iterator）：

它允许一个程序顺序访问聚合对象的元素，而不需要暴露聚合对象的内部表示。迭代器模式提供了一种按顺序访问聚合对象元素的方法，而不依赖于具体的存储方式。

#### 迭代器模式的主要组成元素：

1. **Iterator（迭代器）**：定义了访问和遍历聚合对象元素的接口。
2. **ConcreteIterator（具体迭代器）**：实现了 `Iterator` 接口，跟踪当前遍历的位置。
3. **Aggregate（聚合对象）**：定义了创建迭代器对象的接口。
4. **ConcreteAggregate（具体聚合对象）**：实现了 `Aggregate` 接口，返回一个与该聚合相关联的迭代器对象。

#### 示例：

假设我们有一个图书集合，需要提供一种方法来遍历集合中的所有图书。

- 迭代器接口：

  ```java
  interface Iterator {
      boolean hasNext(); // 是否有下一个元素
      Object next(); // 返回下一个元素
      void remove(); // 移除当前元素
  }
  ```

- 具体迭代器：

  ```java
  class BookCollectionIterator implements Iterator {
      private List<Book> items;
      private int position;
  
      public BookCollectionIterator(List<Book> items) {
          this.items = items;
          this.position = 0;
      }
  
      public boolean hasNext() {
          return position < items.size();
      }
  
      public Object next() {
          return items.get(position++);
      }
  
      public void remove() {
          items.remove(--position);
      }
  }
  ```

- 聚合对象接口：

  ```java
  interface Aggregate {
      Iterator createIterator();
  }
  ```

- 具体聚合对象：

  ```java
  class BookCollection implements Aggregate {
      private List<Book> books;
  
      public BookCollection() {
          books = new ArrayList<>();
      }
  
      public void addBook(Book book) {
          books.add(book);
      }
  
      public Iterator createIterator() {
          return new BookCollectionIterator(books);
      }
  }
  ```

- 客户端代码：

  ```java
  public class IteratorPatternDemo {
      public static void main(String[] args) {
          Aggregate aggregate = new BookCollection();
          aggregate.addBook(new Book("Design Patterns"));
          aggregate.addBook(new Book("Clean Code"));
          aggregate.addBook(new Book("Refactoring"));
  
          Iterator iterator = aggregate.createIterator();
          while (iterator.hasNext()) {
              Book book = (Book) iterator.next();
              System.out.println(book.getTitle());
          }
      }
  }
  
  class Book {
      private String title;
  
      public Book(String title) {
          this.title = title;
      }
  
      public String getTitle() {
          return title;
      }
  }
  ```

在这个示例中，我们定义了一个图书集合 `BookCollection` 和一个图书 `Book` 类。`BookCollection` 实现了 `Aggregate` 接口，它管理图书的集合，并提供一个创建迭代器 `createIterator()` 的方法。`BookCollectionIterator` 是具体迭代器，实现了 `Iterator` 接口，它能够遍历图书集合中的所有图书。

客户端代码通过创建聚合对象，添加图书，并使用迭代器来遍历图书集合。

#### 注意事项：

- **解耦**：迭代器模式将集合的遍历操作从聚合对象中分离出来，使得遍历方式可以独立于聚合对象变化。
- **扩展性**：可以为不同类型的聚合对象定义不同的迭代器，而不需要修改聚合对象本身。
- **性能**：迭代器模式的实现可能会对性能有一定影响，特别是在需要支持线程安全的情况下。

### 4.5.中介者模式（Mediator）：

它定义了一个中介对象，用于简化多个对象之间的交互和通信。中介者模式通过使用一个中介者对象来封装多个对象之间的交互逻辑，从而使得这些对象之间不必相互了解，降低系统的耦合度。

#### 中介者模式的主要组成元素：

1. **Mediator（中介者）**：定义了同事对象之间进行通信的接口。
2. **ConcreteMediator（具体中介者）**：实现了中介者接口，通过具体实现来控制和协调同事对象之间的交互。
3. **Colleague（同事类）**：定义了中介者模式中对象的接口，不直接与其他同事对象通信，而是通过中介者对象来进行通信。

#### 示例：

假设我们有一个聊天室系统，用户（User）可以发送消息，所有其他用户都需要接收到这些消息。

- 中介者接口：

  ```java
  interface Mediator {
      void registerUser(User user);
      void notifyUsers(String message, User user);
  }
  ```

- 具体中介者：

  ```java
  class ChatRoom implements Mediator {
      private List<User> users = new ArrayList<>();
  
      public void registerUser(User user) {
          users.add(user);
      }
  
      public void notifyUsers(String message, User user) {
          for (User recipient : users) {
              if (recipient != user) {
                  recipient.receiveMessage(message);
              }
          }
      }
  }
  ```

- 同事类：

  ```java
  interface User {
      void receiveMessage(String message);
  }
  
  class UserImpl implements User {
      private String name;
      private Mediator mediator;
  
      public UserImpl(String name, Mediator mediator) {
          this.name = name;
          this.mediator = mediator;
          mediator.registerUser(this);
      }
  
      public void sendMessage(String message) {
          mediator.notifyUsers(message, this);
      }
  
      public void receiveMessage(String message) {
          System.out.println(name + " received message: " + message);
      }
  }
  ```

- 客户端代码：

  ```java
  public class MediatorPatternDemo {
      public static void main(String[] args) {
          Mediator mediator = new ChatRoom();
          User user1 = new UserImpl("User1", mediator);
          User user2 = new UserImpl("User2", mediator);
  
          user1.sendMessage("Hello, how are you?");
          user2.sendMessage("I'm fine, thanks!");
      }
  }
  ```

在这个示例中，`ChatRoom` 是具体中介者，它管理了所有注册的用户，并负责在用户之间转发消息。`UserImpl` 是同事类，它实现了 `User` 接口，并通过中介者来发送和接收消息。

客户端代码通过创建中介者对象和多个用户对象，模拟了一个简单的聊天室系统。

#### 注意事项：

- **解耦**：中介者模式通过中介者对象来封装对象之间的交互，降低了对象之间的耦合度。
- **可扩展性**：当需要增加新的对象时，只需要实现相应的同事类，并注册到中介者中即可。
- **中介者的责任**：中介者对象可能会变得复杂，因为它需要处理所有同事对象之间的交互逻辑。

### 4.6.备忘录模式（Memento）：

用于在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后可以恢复到原先保存的状态。

#### 备忘录模式的主要组成元素：

1. **Originator（发起人）**：负责创建一个备忘录Memento，用以记录当前时刻的内部状态，并可以使用该备忘录恢复状态。
2. **Memento（备忘录）**：存储Originator的内部状态，并提供一个接口供Originator使用，以便恢复状态。Memento对其他对象是不可访问的。
3. **Caretaker（管理者）**：负责保存好备忘录Memento，可将Memento视为一个普通对象，但它不能对Memento进行任何操作。
4. **Observer（观察者）**（可选）：在某些实现中，Originator可能作为观察者，当其状态改变时，自动更新备忘录。

#### 示例：

假设我们有一个文本编辑器，需要提供撤销和重做功能。

- 发起人：

  ```java
  class Editor {
      private String content;
  
      public String getContent() {
          return content;
      }
  
      public void setContent(String content) {
          this.content = content;
      }
  
      // 创建备忘录
      public Memento saveStateToMemento() {
          return new Memento(content);
      }
  
      // 从备忘录恢复状态
      public void getStateFromMemento(Memento memento) {
          content = memento.getState();
      }
  
      // 省略其他编辑器功能...
  }
  
  class Memento {
      private String state;
  
      public Memento(String state) {
          this.state = state;
      }
  
      public String getState() {
          return state;
      }
  }
  ```

- 管理者类：

  ```java
  import java.util.Stack;
  
  class EditorHistory {
      private Stack<Memento> history = new Stack<>();
  
      public void push(Memento state) {
          history.push(state);
      }
  
      public Memento pop() {
          return history.pop();
      }
  
      // 省略其他历史管理功能...
  }
  ```

- 客户端代码

  ```java
  public class MementoPatternDemo {
      public static void main(String[] args) {
          Editor editor = new Editor();
          EditorHistory history = new EditorHistory();
  
          // 编辑文档
          editor.setContent("Hello World!");
          history.push(editor.saveStateToMemento());
  
          // 继续编辑文档
          editor.setContent("Hello Memento Pattern!");
          history.push(editor.saveStateToMemento());
  
          // 执行撤销操作
          editor.getStateFromMemento(history.pop());
          System.out.println(editor.getContent()); // 输出: Hello World!
  
          // 再次执行撤销操作
          editor.getStateFromMemento(history.pop());
          System.out.println(editor.getContent()); // 输出: (无输出，因为栈已空)
      }
  }
  ```

在这个示例中，`Editor` 是发起人，它负责创建备忘录 `Memento` 并可以恢复到备忘录中的状态。`Memento` 类存储了 `Editor` 的状态。`EditorHistory` 是管理者，它负责维护备忘录的历史记录。

客户端代码通过编辑文档、保存状态到备忘录、以及从备忘录恢复状态来模拟撤销操作。

#### 注意事项：

- **封装性**：备忘录模式保证了Originator的内部状态的封装性，外部对象无法直接访问Originator的内部状态。
- **状态恢复**：通过备忘录，Originator可以恢复到之前的状态，这在需要撤销和重做功能的场合非常有用。
- **资源消耗**：备忘录模式可能会消耗更多的资源来存储多个状态，特别是当状态数据较大时。

### 4.7.观察者模式（Observer）：

主要用于实现事件的发布/订阅机制。在这种模式中，对象（称为主题或可观察对象）维护其订阅者（称为观察者），并在其状态发生变化时通知它们。这种模式常用于解耦发送者和接收者，确保当对象间存在一对多关系时，当一个对象改变状态，它的所有依赖者都会收到通知并自动更新。

#### 观察者模式的主要组成元素：

1. **Subject（主题）**：是被观察的对象，它包含一些订阅者，并定义了添加、删除和通知订阅者的方法。
2. **Observer（观察者）**：定义了观察者更新的接口。
3. **ConcreteSubject（具体主题）**：实现 `Subject` 接口，存储状态，并在状态改变时通知所有订阅的观察者。
4. **ConcreteObserver（具体观察者）**：实现 `Observer` 接口，完成在接收到主题通知时的具体更新逻辑。

#### 示例：

假设我们有一个天气预报系统，需要在天气变化时通知所有订阅的用户。

- 主题接口：

```java
interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

- 具体主题：

```java
class WeatherStation implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String weather;

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(weather);
        }
    }

    public void setWeather(String weather) {
        this.weather = weather;
        notifyObservers();
    }

    public String getWeather() {
        return weather;
    }
}
```

- 观察者接口：

```java
interface Observer {
    void update(String weather);
}
```

- 具体观察者：

```java
class User implements Observer {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public void update(String weather) {
        System.out.println(name + " is notified: " + weather);
    }
}
```

- 客户端代码：

```java
public class ObserverPatternDemo {
    public static void main(String[] args) {
        Subject weatherStation = new WeatherStation();

        User user1 = new User("User1");
        User user2 = new User("User2");

        weatherStation.registerObserver(user1);
        weatherStation.registerObserver(user2);

        weatherStation.setWeather("Sunny and Warm");
        // User1 is notified: Sunny and Warm
        // User2 is notified: Sunny and Warm
    }
}
```

在这个示例中，`WeatherStation` 是具体主题，它管理了一组订阅者（观察者），并在天气变化时通知它们。`User` 是具体观察者，实现了 `Observer` 接口，它定义了接收到天气变化通知时的行为。

客户端代码通过创建天气预报站和用户，将用户注册为观察者，并改变天气来触发通知。

#### 注意事项：

- **解耦**：观察者模式可以实现主题和观察者之间的解耦，它们之间不需要知道对方的存在。
- **广播通信**：观察者模式使用广播通信方式，所有注册的观察者都会收到通知。
- **顺序**：观察者接收通知的顺序不可预测，如果有依赖关系，可能需要额外的处理。

### 4.8.状态模式（State）：

允许一个对象在其内部状态改变时改变其行为。这个对象看起来似乎修改了它的类，而实际上它是依赖于内部表示来允许改变其行为的。状态模式主要用于解决当一个对象的行为取决于它的状态，且这个状态的变更需要引起行为的显著变化的问题。

#### 状态模式的主要组成元素：

1. **State（状态）**：定义了一个接口以封装与Context的一个特定状态相关的行为。
2. **ConcreteState（具体状态）**：实现了State接口，并定义了与一个状态相关的行为。
3. **Context（上下文）**：维护一个ConcreteState子类的实例，这个实例定义当前的状态。
4. **Client（客户端）**：与Context对象交互，而并不直接与State对象交互。

#### 示例：

假设我们有一个自动售货机，它可以处于几种不同的状态，如出售商品、等待付款、售卖完成等。

- 状态接口：

```java
interface State {
    void insertMoney();
    void ejectMoney();
    void selectProduct();
    void dispenseProduct();
}
```

- 具体状态：

```java
class HasMoneyState implements State {
    private static final double MONEY_AMOUNT = 1.0;

    public void insertMoney() {
        System.out.println("Money already inserted.");
    }

    public void ejectMoney() {
        System.out.println("Ejecting money...");
    }

    public void selectProduct() {
        // ... 选择商品的逻辑
    }

    public void dispenseProduct() {
        // ... 分发商品的逻辑
    }
}

class NoMoneyState implements State {
    // ... 缺少金钱时的状态逻辑
}

class SoldOutState implements State {
    // ... 商品售罄时的状态逻辑
}

// 其他具体状态实现...
```

- 上下文：

```java
class VendingMachine {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void insertMoney() {
        state.insertMoney();
    }

    public void ejectMoney() {
        state.ejectMoney();
    }

    public void selectProduct() {
        state.selectProduct();
    }

    public void dispenseProduct() {
        state.dispenseProduct();
    }
}
```

- 客户端代码：

```java
public class StatePatternDemo {
    public static void main(String[] args) {
        VendingMachine vendingMachine = new VendingMachine();
        vendingMachine.setState(new HasMoneyState());
        vendingMachine.insertMoney(); // 输出: Money already inserted.
        vendingMachine.dispenseProduct(); // 输出分发商品的逻辑
        // 更改状态，模拟用户投入了足够的钱
        vendingMachine.setState(new NoMoneyState());
        vendingMachine.insertMoney(); // 输出投入金钱的逻辑
        // 更多的交互...
    }
}
```

在这个示例中，`VendingMachine` 是上下文，它根据当前的状态来执行不同的行为。`HasMoneyState`、`NoMoneyState` 和 `SoldOutState` 是具体的状态类，它们实现了 `State` 接口，并定义了在特定状态下的行为。

客户端代码通过改变 `VendingMachine` 的状态来模拟不同的用户交互。

#### 注意事项：

- **封装性**：状态模式将与特定状态相关的行为局部化，并且将不同状态的行为分割开来。
- **可扩展性**：当需要添加新的状态时，只需添加一个新的具体状态类即可，符合开闭原则。
- **外部触发**：状态之间的转换通常是通过上下文对象的某个方法进行的，而不是由状态对象自身进行。

### 4.9.策略模式（Strategy）：

它定义了一系列算法，并将每个算法封装起来，使它们可以互换使用，算法的变化不会影响使用算法的用户。策略模式主要用于减少系统中的复杂性，特别是当存在多种算法或行为时，策略模式可以提供一种灵活的、可重用的方式。

#### 策略模式的主要组成元素：

1. **Strategy（策略）**：定义了所有支持的算法的公共接口。
2. **ConcreteStrategy（具体策略）**：实现了 `Strategy` 接口，提供了算法的具体实现。
3. **Context（上下文）**：使用 `Strategy` 对象来执行算法，维护一个 `Strategy` 类型的引用。

#### 示例：

假设我们有一个排序系统，需要支持不同的排序算法，如冒泡排序、选择排序等。

策略接口：

```java
interface SortingStrategy {
    void sort(int[] array);
}
```

具体策略：

```java
class BubbleSort implements SortingStrategy {
    public void sort(int[] array) {
        // 冒泡排序算法的实现
    }
}

class SelectionSort implements SortingStrategy {
    public void sort(int[] array) {
        // 选择排序算法的实现
    }
}

// 其他排序算法的实现...
```

上下文：

```java
class Sorter {
    private SortingStrategy strategy;

    public Sorter(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public void sort(int[] array) {
        strategy.sort(array);
    }
}
```

客户端代码：

```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        int[] numbers = {5, 3, 8, 4, 2};

        Sorter sorter = new Sorter(new BubbleSort());
        System.out.println("Sorted using Bubble Sort:");
        sorter.sort(numbers);
        // 打印排序后的数组

        sorter.setStrategy(new SelectionSort());
        System.out.println("Sorted using Selection Sort:");
        sorter.sort(numbers);
        // 打印排序后的数组
    }
}
```

在这个示例中，`SortingStrategy` 是策略接口，定义了排序算法的接口。`BubbleSort` 和 `SelectionSort` 是具体策略，它们实现了 `SortingStrategy` 接口，并提供了具体的排序算法实现。`Sorter` 是上下文，它使用 `SortingStrategy` 对象来执行排序算法。

客户端代码通过创建 `Sorter` 对象，设置不同的排序策略，并调用 `sort` 方法来对数组进行排序。

#### 注意事项：

- **解耦**：策略模式将算法封装起来，使算法的变化不会影响到上下文。
- **可扩展性**：当需要添加新的算法时，只需添加一个新的具体策略类即可，符合开闭原则。
- **策略的选择**：客户端可以根据需要选择不同的策略，而不需要了解策略的具体实现。

### 4.10.模板方法模式（Template Method）：

用于定义一个算法的骨架，将算法的一些步骤延迟到子类中实现。模板方法模式可以让子类在不改变算法结构的情况下，重新定义算法的某些特定步骤。

#### 模板方法模式的主要组成元素：

1. **AbstractClass（抽象类）**：定义了模板方法和一些基本的方法。模板方法一般是一个抽象方法，它在抽象类中被声明，同时提供一个默认的实现。
2. **PrimitiveOperation（基本操作）**：在抽象类中定义的，作为算法的组成部分的方法。这些方法可以是具体的，也可以是抽象的，它们代表了算法的固定步骤或可变步骤。
3. **ConcreteClass（具体类）**：继承自抽象类，实现抽象类中定义的未实现的基本操作。

#### 示例：

假设我们有一个制作饮料的系统，饮料的制作包括准备杯子、添加原料、调制饮料等步骤，但每种饮料的具体调制过程可能不同。

- 抽象类：

```java
abstract class Beverage {
    // 模板方法，定义饮料制作的流程
    public final void prepare() {
        boilWater();
        brew();
        pourInCup();
        templateMethod();
    }

    // 钩子方法，可以被子类重写
    public void addCondiments() {
        System.out.println("No condiments added");
    }

    // 基本操作，具体实现
    public void boilWater() {
        System.out.println("Boiling water");
    }

    public void brew() {
        System.out.println("Brewing the tea");
    }

    public void pourInCup() {
        System.out.println("Pouring into cup");
    }

    // 基本操作，延迟到子类实现
    public abstract void templateMethod();
}
```

- 具体类：

```java
class Tea extends Beverage {
    public void templateMethod() {
        addCondiments();
        System.out.println("Tea is ready");
    }
}

class Coffee extends Beverage {
    public void templateMethod() {
        System.out.println("Coffee is ready");
    }

    // 重写钩子方法
    public void addCondiments() {
        System.out.println("Adding milk and sugar");
    }
}
```

- 客户端代码：

```java
public class TemplateMethodPatternDemo {
    public static void main(String[] args) {
        Beverage tea = new Tea();
        tea.prepare(); // 输出: 制作茶的步骤

        Beverage coffee = new Coffee();
        coffee.prepare(); // 输出: 制作咖啡的步骤
    }
}
```

在这个示例中，`Beverage` 是抽象类，它定义了制作饮料的模板方法 `prepare()`，以及几个基本操作。`Tea` 和 `Coffee` 是具体类，它们继承自 `Beverage` 并实现了 `templateMethod()` 方法，该方法是算法的可变部分。

客户端代码通过创建具体的饮料对象，并调用 `prepare()` 方法来制作饮料。

#### 注意事项：

- **算法骨架**：模板方法模式定义了算法的骨架，将一些步骤延迟到子类实现，从而保证了算法的结构不变，同时允许子类定制某些步骤。
- **钩子方法**：钩子方法提供了一个默认实现，但通常期望子类会重写它。钩子方法可以被视为模板方法中的“可选行为”。
- **代码复用**：模板方法模式通过提供一个通用的结构，允许子类复用代码，同时提供定制化的步骤。

模板方法模式是一种有用的设计模式，它适用于需要在一个固定算法骨架中定制特定步骤的场景。这种模式在处理多个步骤的算法，且算法步骤基本不变但某些步骤需要定制化时非常有用。

### 4.11.访问者模式（Visitor）：

它使你可以在不修改对象结构的情况下，为对象结构中的每个元素添加新的功能。访问者模式通过将处理从对象结构中分离出来，将处理封装到访问者对象中，从而提高了对象结构的灵活性和可扩展性。

#### 访问者模式的主要组成元素：

1. **Visitor（访问者）**：定义了对每一个元素类进行访问的行为，对应于每一个元素类，都有一个对应的访问操作。
2. **ConcreteVisitor（具体访问者）**：实现 `Visitor` 接口，为各个元素类提供相应的访问实现。
3. **Element（元素）**：定义了一个 `accept` 方法，用于接受访问者。
4. **ConcreteElement（具体元素）**：实现了 `Element` 接口，具体实现了元素对象，每一个具体元素类都对应有一个接受访问者的方法。
5. **ObjectStructure（对象结构）**：是一个包含元素集合的容器，提供了一个方法用来迭代元素集合，使每个元素对象接受访问者。

#### 示例：

假设我们有一个文档编辑器，文档由不同类型的内容组成，如段落、图片等。我们需要对文档执行不同的操作，如计算字数、显示内容等。

- 访问者接口：

```java
interface DocumentVisitor {
    void visit(Paragraph paragraph);
    void visit(Image image);
}
```

- 具体访问者：

```java
class WordCountVisitor implements DocumentVisitor {
    public void visit(Paragraph paragraph) {
        System.out.println("Word count for paragraph: " + paragraph.wordCount());
    }

    public void visit(Image image) {
        System.out.println("Image has no word count");
    }
}

class DisplayVisitor implements DocumentVisitor {
    public void visit(Paragraph paragraph) {
        System.out.println("Displaying paragraph: " + paragraph.getContent());
    }

    public void visit(Image image) {
        System.out.println("Displaying image: " + image.getFileName());
    }
}
```

- 元素接口：

```java
interface DocumentElement {
    void accept(DocumentVisitor visitor);
}
```

- 具体元素：

```java
class Paragraph implements DocumentElement {
    private String content;

    public Paragraph(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }

    public int wordCount() {
        // 简单的计数逻辑，实际应用可能更复杂
        return content.split("\\s+").length;
    }

    public void accept(DocumentVisitor visitor) {
        visitor.visit(this);
    }
}

class Image implements DocumentElement {
    private String fileName;

    public Image(String fileName) {
        this.fileName = fileName;
    }

    public String getFileName() {
        return fileName;
    }

    public void accept(DocumentVisitor visitor) {
        visitor.visit(this);
    }
}
```

- 对象结构：

```java
import java.util.ArrayList;
import java.util.List;

class Document {
    private List<DocumentElement> elements = new ArrayList<>();

    public void addElement(DocumentElement element) {
        elements.add(element);
    }

    public void displayAll() {
        for (DocumentElement element : elements) {
            element.accept(new DisplayVisitor());
        }
    }

    public void wordCountAll() {
        for (DocumentElement element : elements) {
            element.accept(new WordCountVisitor());
        }
    }
}
```

- 客户端代码：

```java
public class VisitorPatternDemo {
    public static void main(String[] args) {
        Document document = new Document();
        document.addElement(new Paragraph("Hello World"));
        document.addElement(new Image("image.png"));

        document.displayAll(); // 显示文档内容
        document.wordCountAll(); // 计算文档字数
    }
}
```

在这个示例中，`DocumentVisitor` 是访问者接口，定义了对文档元素进行操作的方法。`Paragraph` 和 `Image` 是具体元素，实现了 `DocumentElement` 接口，并提供了 `accept` 方法来接受访问者。`Document` 是对象结构，它维护了文档元素的集合，并提供了方法来执行所有元素的特定操作。

客户端代码通过创建文档和元素，添加元素到文档，并调用 `displayAll` 和 `wordCountAll` 方法来显示文档内容和计算字数。

#### 注意事项：

- **解耦**：访问者模式将算法从对象结构中分离出来，使得对象结构和访问者可以独立变化。
- **扩展性**：可以在不修改对象结构的情况下，通过添加新的访问者来扩展新的功能。
- **灵活性**：访问者模式提供了一种灵活的、可复用的方式来操作对象结构中的元素。
