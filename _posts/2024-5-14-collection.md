---
layout: post
title: Java集合
description: Java集合
author: MuZhi
date: 2024-5-14 21:29:46 +0800
categories: [Java,集合]
tags: [集合,笔记]
pin: false #这是一个布尔值，表示这篇文章是否置顶
math: true #同样是一个布尔值，表示是否启用数学公式渲染
mermaid: true # 表示是否启用墨水图（Mermaid）渲染
image:
  path: public-images/Java%20%E9%9B%86%E5%90%88-logo.png
  alt: 集合.
---

## 常见集合

Java 集合类主要由两个接口`Collection`和`Map`派生出来，`Collection`有三个子接口：`List`、`Set`、`Queue`。

![Collection](https://raw.githubusercontent.com/MuzhiCoder/MuZhiCoderImages/main/public-images/Collection.png)
### List 集合

`List` 是一个接口，属于 Java 集合框架（Java Collections Framework）的一部分，用于存储一系列元素。`List` 接口继承自 `Collection` 接口，提供了一些额外的功能，比如元素的有序性（元素按照插入顺序保存）和通过索引访问元素。

以下是 `List` 接口的一些关键特性：

1. **有序性**：`List` 中的元素按照添加顺序排列，可以通过索引来访问。
2. **可重复**：`List` 允许存储重复的元素。
3. **实现类**：`List` 接口的一些常见实现包括 `ArrayList`、`LinkedList` 和 `Vector`。
4. **方法**：`List` 提供了多种方法，如 `add(E e)`, `remove(int index)`, `get(int index)`, `size()` 等。
5. **性能**：不同的实现类在性能上有不同的特点。例如，`ArrayList` 提供快速的随机访问，而 `LinkedList` 提供快速的插入和删除操作。
6. **同步**：`Vector` 是 `List` 的同步版本，适合多线程环境。
7. **不可变列表**：Java 还提供了不可变列表，如 `Collections.unmodifiableList(List<? extends E> list)`，可以创建原始列表的只读视图。

### Set 集合

`Set` 是一个接口，它继承自 `Collection` 接口，用于存储一组元素。与 `List` 不同，`Set` 集合具有以下特性：

1. **不包含重复元素**：`Set` 集合不允许存储重复的元素。如果尝试将一个已存在的元素添加到 `Set` 中，那么添加操作不会改变集合，该元素也不会被再次添加。
2. **无序性**：`Set` 集合中的元素不保持任何顺序。当遍历一个 `Set` 集合时，不能保证哪个元素先被访问。
3. **实现类**：Java 提供了几个 `Set` 接口的实现，包括 `HashSet`、`LinkedHashSet` 和 `TreeSet`。
4. **方法**：`Set` 提供了添加（`add()`）、移除（`remove()`）、检查是否存在（`contains()`）等操作。
5. **性能**：不同的 `Set` 实现在性能上有不同的特点。例如，`HashSet` 通常提供最快的查找速度，而 `TreeSet` 保持了元素的排序。
6. **线程安全**：与 `List` 中的 `Vector` 相对应，`Set` 也有线程安全的实现，如 `Collections.synchronizedSet(Set<T> s)`。

### Queue 集合

`Queue` 是一个接口，属于 Java 集合框架的一部分，用于表示一个队列。队列是一种特殊类型的集合，遵循先进先出（FIFO）的原则，即队列的第一个元素是第一个被取出的元素，而最后一个元素是最后一个被添加的。

以下是 `Queue` 接口的一些关键特性：

1. **先进先出（FIFO）**：队列中的元素按照它们被添加的顺序进行处理。
2. **阻塞行为**：某些 `Queue` 实现可能是阻塞的，意味着如果队列为空，取出元素的操作会等待，直到有元素被添加。同样，如果队列满了，添加元素的操作也会等待，直到有足够的空间。
3. **实现类**：`Queue` 接口的一些常见实现包括 `LinkedList`（作为双端队列 `Deque` 使用）、`PriorityQueue` 和 `ArrayDeque`。
4. **方法**：`Queue` 提供了 `add(E e)`, `offer(E e)`, `poll()`, `remove()`, `peek()`, `element()` 等方法。其中，`add` 方法在无法添加元素时会抛出 `IllegalStateException`，而 `offer` 方法会返回 `false`。`poll` 和 `remove` 方法在队列为空时会返回 `null`，而 `peek` 和 `element` 方法则返回队列头部的元素而不移除它，如果队列为空，则 `peek` 返回 `null`，`element` 会抛出 `NoSuchElementException`。
5. **优先级队列**：`PriorityQueue` 是一个无界的优先级队列，元素根据自然排序或者根据提供的 `Comparator` 进行排序。

### Map 集合

`Map` 是一个接口，属于 Java 集合框架的一部分，用于存储键值对（key-value pairs）。每个键（key）映射到一个值（value），并且每个键在 `Map` 中都是唯一的。

以下是 `Map` 接口的一些关键特性：

1. **键值对存储**：`Map` 存储数据的方式是键值对，其中每个键映射到一个特定的值。
2. **键的唯一性**：`Map` 接口不允许有重复的键，但可以有多个值相同。
3. **无序性**：`Map` 集合中的键值对不保持任何顺序。
4. **实现类**：`Map` 接口的一些常见实现包括 `HashMap`、`TreeMap`、`LinkedHashMap` 和 `Hashtable`。
5. **线程安全**：`Hashtable` 是 `Map` 的线程安全实现，但性能较低。`Collections.synchronizedMap` 方法也可以提供对其他 `Map` 实现的线程安全包装。
6. **键和值的null性**：大多数 `Map` 实现允许键和值为 `null`，但 `HashMap` 在 Java 9 及以后的版本中不允许键为 `null`。
7. **遍历**：可以通过键集合（`keySet()`）或值集合（`values()`）来遍历 `Map`。

## List、Set和Map区别

`List`、`Set` 和 `Map` 都是 Java 集合框架中的重要组成部分，它们各自有不同的特点和用途。

以下是它们之间的主要区别：

1. **存储的结构**：
  - `List`（如 `ArrayList`, `LinkedList`）: 存储一系列元素（有序，可以有重复元素）。
  - `Set`（如 `HashSet`, `TreeSet`）: 存储一组元素（无序，不含有重复元素）。
  - `Map`（如 `HashMap`, `TreeMap`）: 存储键值对（键唯一，每个键映射到一个值）。
2. **性能特点**：
  - `List` 提供了通过索引访问元素的能力，因此可以快速地随机访问。
  - `Set` 通常用于快速查找，因为它们不允许重复，并且很多实现（如 `HashSet`）提供了快速查找的能力。
  - `Map` 通常用于快速查找特定的键值对，特别是当需要根据键快速检索或存储数据时。
3. **元素的有序性**：
  - `List` 是有序的，可以按照元素的插入顺序访问元素。
  - `Set` 是无序的，元素的顺序不固定。
  - `Map` 本身不是有序的，但一些实现（如 `LinkedHashMap` 或 `TreeMap`）可以保持键的插入顺序或基于键的自然排序顺序。
4. **元素的唯一性**：
  - `List` 可以包含重复的元素。
  - `Set` 不能包含重复的元素，即每个元素都是唯一的。
  - `Map` 中的键必须是唯一的，但值可以重复。
5. **访问方式**：
  - `List` 通过索引访问元素，例如 `get(int index)`。
  - `Set` 通常通过迭代器进行访问，因为它们不提供索引。
  - `Map` 通过键访问值，例如 `get(Object key)`。
6. **典型用例**：
  - `List` 适合用于索引或顺序很重要的场景，如数组的动态版本。
  - `Set` 适合用于需要去重或需要测试元素是否存在的场景。
  - `Map` 适合用于需要根据键快速查找值的场景，如数据库索引。
7. **线程安全**：
  - 集合框架中，`List` 和 `Set` 的线程安全版本可以通过 `Collections.synchronizedList(List<T> list)` 和 `Collections.synchronizedSet(Set<T> set)` 获得。
  - `Map` 的线程安全版本可以通过 `Collections.synchronizedMap(Map<K, V> m)` 获得，或者使用 `Hashtable`（虽然它已经过时，被 `ConcurrentHashMap` 所取代）。
8. **空元素**：
  - `List` 和 `Set` 通常不允许空（null）元素，尽管 `Set` 的某些实现可能允许一个空元素。
  - `Map` 的键不能为空（null），但值可以是空（null）。

## ArrayList 类

`ArrayList` 是 Java 中的一个非常常用的类，它实现了 `List` 接口，提供了一个可调整大小的数组实现。

以下是 `ArrayList` 的一些关键特性：

1. **动态数组**：`ArrayList` 内部使用一个动态数组（`Object` 数组）来存储元素，这意味着它的大小可以根据需要自动增长。
2. **随机访问**：由于 `ArrayList` 基于数组实现，它可以提供快速的随机访问，即通过索引访问元素的时间复杂度为 O(1)。
3. **非同步**：`ArrayList` 不是线程安全的。如果需要线程安全的 `List`，可以使用 `Vector`（虽然它已经过时），或者使用 `Collections.synchronizedList` 方法将 `ArrayList` 包装为线程安全。
4. **允许空元素**：`ArrayList` 允许存储空（null）元素。
5. **非固定大小**：与数组不同，`ArrayList` 的大小可以根据需要增长或缩小。
6. **性能**：对于频繁的插入和删除操作，`ArrayList` 可能不是最佳选择，因为这些操作可能需要数组的复制和重新分配。但对于随机访问，`ArrayList` 是非常高效的。

### ArrayList的扩容机制

`ArrayList` 在 Java 中是一个基于动态数组实现的集合类，它可以根据需要自动增长。当 `ArrayList` 中的元素数量达到其当前容量时，就会触发扩容机制。

以下是 `ArrayList` 扩容机制的详细说明：

1. **初始容量**：当你创建一个 `ArrayList` 实例时，如果没有指定初始容量，那么默认的初始容量是 10。也可以在创建时指定一个初始容量。
2. **扩容时机**：当尝试向 `ArrayList` 添加元素，使其大小超过当前容量时，就会触发扩容操作。
3. **扩容操作**：在扩容时，`ArrayList` 会创建一个新的数组，其大小是原有数组的 1.5 倍（或者说，新容量是原容量加上原容量的一半）。这样可以确保 `ArrayList` 在添加元素时不需要频繁地进行扩容操作，从而在大多数情况下保持了较好的性能。
4. **复制元素**：创建新数组后，`ArrayList` 会将原有数组中的所有元素复制到新数组中。这个复制过程是扩容过程中最耗时的部分，因为它需要 O(n) 的时间复杂度。
5. **更新引用**：复制完成后，`ArrayList` 会丢弃旧数组，并将新数组设置为 `ArrayList` 的内部数组。
6. **内存开销**：由于 `ArrayList` 会在容量不足时进行扩容，并且每次扩容都会创建一个更大的数组，所以它可能会比使用固定大小数组的实现占用更多的内存。
7. **自定义扩容**：在添加大量元素时，可以通过调用 `ensureCapacity()` 方法预先设置一个较大的容量，以避免多次扩容操作。

**默认情况下，新的容量会是原容量的1.5倍**。以JDK1.8为例说明:

```java
public boolean add(E e) {
    //判断是否可以容纳e，若能，则直接添加在末尾；若不能，则进行扩容，然后再把e添加在末尾
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //将e添加到数组末尾
    elementData[size++] = e;
    return true;
    }

// 每次在add()一个元素时，arraylist都需要对这个list的容量进行一个判断。通过ensureCapacityInternal()方法确保当前ArrayList维护的数组具有存储新元素的能力，经过处理之后将元素存储在数组elementData的尾部
private void ensureCapacityInternal(int minCapacity) {
      ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {
        //如果传入的是个空数组则最小容量取默认容量与minCapacity之间的最大值
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
    
  private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // 若ArrayList已有的存储能力满足最低存储要求，则返回add直接添加元素；如果最低要求的存储能力>ArrayList已有的存储能力，这就表示ArrayList的存储能力不足，因此需要调用 grow();方法进行扩容
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }

private void grow(int minCapacity) {
	// 获取elementData数组的内存空间长度
	int oldCapacity = elementData.length;
	// 扩容至原来的1.5倍
	int newCapacity = oldCapacity + (oldCapacity >> 1);
	//校验容量是否够
	if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
		//若预设值大于默认的最大值，检查是否溢出
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // 调用Arrays.copyOf方法将elementData数组指向新的内存空间
         //并将elementData的数据复制到新的内存空间
        elementData = Arrays.copyOf(elementData, newCapacity);
}
```

### Arraylist 和 Vector 的区别

`ArrayList` 和 `Vector` 都是 Java 中用于存储可变大小的元素列表的类，它们都是 `List` 接口的实现。尽管它们有相似之处，但也有一些关键的区别：

1. **同步性**：
  - `ArrayList` 是非线程安全的，这意味着它不保证在多线程环境下的数据一致性。在单线程环境下，`ArrayList` 通常性能更好，因为它没有同步的开销。
  - `Vector` 是线程安全的，因为它的所有方法都是同步的。这使得 `Vector` 可以在多线程环境中安全使用，但这也导致了它的性能比 `ArrayList` 低。
2. **性能**：
  - 由于 `Vector` 的同步特性，它在单线程环境下的性能通常不如 `ArrayList`。
  - `ArrayList` 提供了快速的随机访问，因为它是基于数组实现的。
3. **方法**：
  - `Vector` 提供了一些额外的方法，如 `addElement()` 和 `removeElement()`，这些方法在 `ArrayList` 中没有直接对应的版本。
4. **遗留代码**：
  - `Vector` 是 Java 早期版本中的一部分，现在已经不推荐使用，因为 Java 引入了 `Collections` 类和同步包装器，提供了更好的线程安全解决方案。
5. **扩容机制**：
  - `ArrayList` 和 `Vector` 的扩容机制相似，但 `Vector` 通常使用更保守的扩容策略，它增加的容量可能比 `ArrayList` 更小。
6. **泛型**：
  - 在 Java 5 引入泛型之前，`Vector` 没有泛型支持。Java 5 之后，`Vector` 有了泛型的支持，但 `ArrayList` 仍然是首选，因为它更高效。
7. **使用场景**：
  - 在单线程环境下，推荐使用 `ArrayList`。
  - 在多线程环境下，可以使用 `Collections.synchronizedList` 包装一个 `ArrayList` 来提供线程安全，或者使用 `CopyOnWriteArrayList`，这是一种线程安全的变体，适用于读多写少的场景。
8. 总结
  1. ArrayList在内存不够时扩容为原来的1.5倍，Vector是扩容为原来的2倍。
  2. Vector属于线程安全级别的，但是大多数情况下不使用Vector，因为操作Vector效率比较低。


### Arraylist 与 LinkedList的区别

`ArrayList` 和 `LinkedList` 都是 Java 中 `List` 接口的实现，但它们在内部实现和性能特性上有所不同。

以下是 `ArrayList` 和 `LinkedList` 的主要区别：

1. **内部实现**：
  - `ArrayList`：基于动态数组（即数组）实现，允许随机访问。数组不是连续的内存空间，但提供了快速的索引定位。
  - `LinkedList`：基于双向链表实现，不支持随机访问。链表由一系列节点组成，每个节点包含数据部分和指向前一个节点和后一个节点的链接。
2. **性能**：
  - `ArrayList` 提供快速的随机访问操作（O(1)时间复杂度），但插入和删除操作可能较慢（O(n)时间复杂度），特别是当涉及到大量元素时，因为它们可能需要数组的复制和重新分配。
  - `LinkedList` 提供快速的插入和删除操作（O(1)时间复杂度，如果已经有了迭代器或者访问的是头尾部），但随机访问较慢（O(n)时间复杂度），因为需要顺序遍历链表。
3. **内存使用**：
  - `ArrayList` 通常使用较少的内存，因为它是一个连续的数组。
  - `LinkedList` 使用更多的内存，因为每个元素都需要额外的存储空间来维护指向前一个和后一个元素的引用。
4. **线程安全**：
  - 两者都不是线程安全的。如果需要线程安全的 `List`，可以使用 `Collections.synchronizedList` 方法包装它们。
5. **空元素**：
  - `ArrayList` 和 `LinkedList` 都允许存储空（null）元素。
6. **迭代器**：
  - `LinkedList` 提供了双向迭代器，可以方便地从任一方向遍历列表。
  - `ArrayList` 提供的是标准的 Java 迭代器。
7. **使用场景**：
  - 当你需要频繁进行搜索和随机访问时，`ArrayList` 是更好的选择。
  - 当你需要频繁地在列表中插入或删除元素时，尤其是在列表的中间或开始位置，`LinkedList` 是更好的选择。
8. **序列化**：
  - `ArrayList` 和 `LinkedList` 都可以被序列化，但 `LinkedList` 在序列化时可能会更慢，因为它需要序列化整个链表结构。
9. **扩容**：
  - `ArrayList` 有一个固定的扩容机制，通常当达到容量时会增长到原来的 1.5 倍。
  - `LinkedList` 不需要扩容，因为它的存储不是连续的，所以不会遇到空间不足的问题。
10. **总结**：
1. ArrayList基于动态数组实现；LinkedList基于链表实现。
2. 对于随机index访问的get和set方法，ArrayList的速度要优于LinkedList。因为ArrayList直接通过数组下标直接找到元素；LinkedList要移动指针遍历每个元素直到找到为止。
3. 新增和删除元素，LinkedList的速度要优于ArrayList。因为ArrayList在新增和删除元素时，可能扩容和复制数组；LinkedList实例化对象需要时间外，只需要修改指针即可。


## HashMap 类

`HashMap` 是 Java 中的一个类，它实现了 `Map` 接口，用于存储键值对（key-value pairs）。`HashMap `使用数组+链表+红黑树（JDK1.8增加了红黑树部分）实现的，链表长度大于8（`TREEIFY_THRESHOLD`）时，会把链表转换为红黑树，红黑树节点个数小于6（`UNTREEIFY_THRESHOLD`）时才转化为链表，防止频繁的转化。

以下是 `HashMap` 的一些关键特性：

1. **基于哈希表**：`HashMap` 使用哈希表数据结构来存储元素，通过键的哈希值来确定值的存储位置，这使得它能够提供快速的查找和插入操作。
2. **键值对映射**：每个键映射到一个特定的值，键在 `HashMap` 中是唯一的，但值可以有重复。
3. **允许空键和空值**：从 Java 8 开始，`HashMap` 允许一个空（null）键和多个空（null）值。
4. **不保证有序性**：`HashMap` 不保证其元素的有序性，即元素的顺序可能会随着时间的推移而改变。
5. **线程安全性**：`HashMap` 不是线程安全的。如果需要线程安全的 `Map`，可以使用 `ConcurrentHashMap` 或将 `HashMap` 包装为线程安全，例如使用 `Collections.synchronizedMap(new HashMap<K, V>())`。
6. **初始容量和加载因子**：在创建 `HashMap` 时，可以指定初始容量和加载因子。初始容量是哈希表的大小，加载因子是一个影响哈希表扩容的参数。
7. **键的哈希码**：`HashMap` 使用键对象的 `hashCode()` 方法来计算哈希值，该哈希值随后用于确定键值对的存储位置。
8. **碰撞处理**：当两个或多个键的哈希值相同时（即发生碰撞），`HashMap` 会使用链表或红黑树（在 Java 8 及以后版本中，当链表超过一定长度时会自动转为红黑树）来解决冲突。
9. **遍历**：可以通过键集合（`keySet()`）、值集合（`values()`）或条目集合（`entrySet()`）来遍历 `HashMap`。

### Hash冲突

解决哈希冲突（hash collision）的方法主要有以下几种：

1. **链地址法（Chaining）**：
  - 在每个哈希桶（或称为哈希槽）中，使用链表来存储具有相同哈希值的元素。当发生冲突时，新的元素不是替换掉原有元素，而是在同一个哈希桶中形成一个链表的下一个节点。
2. **开放寻址法（Open Addressing）**：
  - 当冲突发生时，算法会寻找表中的下一个空闲位置来存储元素。这个过程可能会使用线性探测（linear probing）、二次探测（quadratic probing）或双重散列（double hashing）等技术。
3. **线性探测**：
  - 线性探测是开放寻址法的一种，当冲突发生时，算法会从当前位置开始，线性地探测下一个空闲位置。
4. **二次探测**：
  - 这种方法也是开放寻址法的一种，它在发生冲突时，按照二次函数（如 `i^2`）的步长来寻找空闲位置。
5. **双重散列**：
  - 使用两个哈希函数来确定元素的存储位置。当第一个哈希函数发生冲突时，使用第二个哈希函数计算步长，从而找到新的空闲位置。
6. **表格扩容**：
  - 当哈希表中的元素太多，导致冲突增加时，可以通过增加哈希表的大小来减少冲突。这通常涉及到重新计算所有元素的哈希值并将它们放入新的更大的哈希表中。
7. **红黑树**：
  - 这是 Java 8 中 `HashMap` 的改进之一。当链表的长度超过一定阈值（默认为 8）时，链表会被转换成红黑树，以提高搜索效率。
8. **哈希函数优化**：
  - 设计一个好的哈希函数可以减少冲突的发生。理想情况下，哈希函数应该能够将输入均匀地分布在哈希表的所有槽中。
9. **哈希表的负载因子控制**：
  - 负载因子是哈希表已使用的槽的数量与总槽数量的比率。通过控制负载因子的大小，可以在保持哈希表较小的同时减少冲突。

### HashMap为什么线程不安全

1. **并发修改导致的数据不一致**：当多个线程尝试同时修改 `HashMap` 结构时，可能会导致内部数据结构的不一致。例如，两个线程可能同时尝试对 `HashMap` 进行扩容，这可能导致其中一些元素丢失。
2. **导致死循环的转链**：在 `HashMap` 中，如果使用了不恰当的哈希函数，且同时有多个线程进行 put 操作，可能会导致链表形成循环，这将使得 `HashMap` 的某些操作（如 `get` 或 `remove`）陷入无限循环。
3. **快速失败的迭代器**：`HashMap` 的迭代器是快速失败的，这意味着在迭代过程中如果检测到 `HashMap` 结构发生变化（除了迭代器自身的 `remove` 方法），迭代器会立即抛出 `ConcurrentModificationException`。在多线程环境下，结构变化是很可能发生的。
4. **计数器的不准确性**：在 `HashMap` 中，有一个用来统计结构修改次数的计数器，这个计数器用于迭代器的快速失败检测。在多线程环境下，这个计数器可能会变得不准确。

### HashMap和HashTable的区别

`HashMap` 和 `Hashtable` 都是 Java 中的 `Map` 接口的实现，它们用于存储键值对集合。

尽管它们有相似之处，但也存在一些关键的区别：

1. **线程安全性**：
  - `Hashtable` 是线程安全的，它的所有方法都是同步的，这意味着可以在多线程环境中直接使用 `Hashtable` 而不会发生并发访问问题。
  - `HashMap` 是非线程安全的，它没有提供任何同步控制。在多线程环境中使用 `HashMap` 时，需要采取其他措施来保证线程安全，如使用 `Collections.synchronizedMap` 包装 `HashMap` 或者使用 `ConcurrentHashMap`。
2. **性能**：
  - 由于 `Hashtable` 的所有方法都是同步的，因此在单线程环境下，它比 `HashMap` 要慢。
  - `HashMap` 在单线程环境下性能更优，因为它避免了同步带来的开销。
3. **空键和空值**：
  - `Hashtable` 不允许空键和空值，尝试插入空键或空值将抛出 `NullPointerException`。
  - `HashMap` 允许一个空键和多个空值，但从 Java 9 开始，`HashMap` 也不允许空键。
4. **遗留特性**：
  - `Hashtable` 是 Java 早期版本的一部分，它继承自 `Dictionary` 类，是一个遗留类。
  - `HashMap` 是 Java Collections Framework 的一部分，是推荐使用的现代 `Map` 实现。
5. **遍历方式**：
  - `Hashtable` 继承了 `Dictionary` 类，因此可以使用 `keys()` 和 `elements()` 方法来获取 `Enumeration` 对象，进而遍历键和值。
  - `HashMap` 通过 `keySet()`、`values()` 和 `entrySet()` 提供了遍历键、值和键值对的方式。
6. **遗留方法**：
  - `Hashtable` 提供了一些遗留方法，如 `elements()` 和 `keys()`，这些方法返回 `Enumeration` 类型的集合视图，它们是 Java 1.1 的特性。
  - `HashMap` 没有这些遗留方法，它遵循 Java Collections Framework 的标准方法。
7. **使用场景**：
  - 由于性能和线程安全的原因，`HashMap` 更适用于单线程环境或需要明确管理线程安全的场合。
  - `Hashtable` 由于其线程安全的实现，可以不经过额外同步处理就用于多线程环境。
8. **替代实现**：
  - 在需要线程安全的 `Map` 实现时，推荐使用 `ConcurrentHashMap`，它是专为高并发环境设计的。

### LinkedHashMap 类

`LinkedHashMap` 是 Java 中的一个类，它是 `HashMap` 的子类，因此它继承了 `HashMap` 的所有特性，并且提供了一些额外的功能。

以下是 `LinkedHashMap` 的一些关键特性：

1. **有序性**：`LinkedHashMap` 保持了元素的插入顺序，或者如果指定了访问顺序，那么它将按照访问顺序来排列元素。这是通过维护一个双向链表来实现的，这个链表与 `HashMap` 的条目是相互关联的。
2. **散列和链接**：`LinkedHashMap` 同样使用哈希表来存储数据，同时通过链表解决冲突，这与 `HashMap` 类似。但是，`LinkedHashMap` 中的链表结构还负责维护元素的顺序。
3. **可选的访问顺序**：`LinkedHashMap` 可以按照两种顺序来遍历元素：
  - **插入顺序**：元素按照它们被插入到 `LinkedHashMap` 的顺序来排列。
  - **访问顺序**：每当通过 `get()` 或 `put()` 方法访问或更新一个元素时，该元素就会移动到链表的尾部，从而改变遍历顺序。
4. **性能**：与 `HashMap` 相比，`LinkedHashMap` 可能会有稍微的性能开销，因为它需要维护元素的顺序信息。
5. **线程安全性**：`LinkedHashMap` 同样不是线程安全的。如果需要线程安全的有序映射，可以考虑使用 `Collections.synchronizedMap` 方法包装 `LinkedHashMap`。
6. **构造函数**：
  - 除了继承自 `HashMap` 的构造函数外，`LinkedHashMap` 还提供了一个构造函数，允许你指定一个 `boolean` 值，该值决定是按照插入顺序还是访问顺序来遍历映射。

#### LinkedHashMap底层原理

HashMap是无序的，迭代HashMap所得到元素的顺序并不是它们最初放到HashMap的顺序，即不能保持它们的插入顺序。

LinkedHashMap继承于HashMap，是HashMap和LinkedList的融合体，具备两者的特性。每次put操作都会将entry插入到双向链表的尾部。

## TreeMap 类

`TreeMap` 是 Java 中的一个类，它是 `Map` 接口的实现之一，并且是 `SortedMap` 接口的实现。`TreeMap` 可以自动按照键的自然顺序或者根据提供的 `Comparator` 对键进行排序。以下是 `TreeMap` 的一些关键特性：

1. **红黑树实现**：`TreeMap` 内部使用一个红黑树数据结构来存储键值对，红黑树是一种自平衡的二叉搜索树。
2. **键的排序**：`TreeMap` 可以按照键的自然顺序对键值对进行排序，或者根据构造函数中提供的 `Comparator` 对键进行排序。
3. **有序性**：`TreeMap` 保证了按照键的顺序对元素进行有序遍历。
4. **线程安全性**：和 `HashMap` 一样，`TreeMap` 也是非线程安全的。如果需要线程安全的有序映射，可以考虑使用 `Collections.synchronizedSortedMap` 方法包装 `TreeMap`。
5. **性能**：`TreeMap` 通常在插入、删除和查找操作上比 `HashMap` 慢，因为它需要维护树的平衡。但是，对于需要按序遍历键值对的场景，`TreeMap` 是一个合适的选择。
6. **遍历方式**：由于 `TreeMap` 是有序的，它支持从键值对中按照键的顺序进行高效的遍历。
7. **导航方法**：`TreeMap` 提供了一些导航方法，如 `firstKey()`, `lastKey()`, `ceilingKey()`, `floorKey()`, `higherKey()`, `lowerKey()`，这些方法可以方便地找到与给定键最接近的键。
8. **克隆**：`TreeMap` 实现了 `cloneable` 接口，这意味着它可以被克隆。

```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable{
}
```

### TreeMap的特点

1. TreeMap是有序的key-value集合，通过红黑树实现。根据键的自然顺序进行排序或根据提供的Comparator进行排序。

2. TreeMap继承了AbstractMap，实现了NavigableMap接口，支持一系列的导航方法，给定具体搜索目标，可以返回最接近的匹配项。如floorEntry()、ceilingEntry()分别返回小于等于、大于等于给定键关联的Map.Entry()对象，不存在则返回null。lowerKey()、floorKey、ceilingKey、higherKey()只返回关联的key。

## HashSet 类

`HashSet` 是 Java 中的一个类，它是 `Set` 接口的实现之一。`HashSet` 存储一组不包含重复元素的集合，并且提供了快速查找的功能。以下是 `HashSet` 的一些关键特性：

1. **基于哈希表**：`HashSet` 实际上是基于 `HashMap` 实现的。在 `HashSet` 中，对象作为 `HashMap` 的键，而与之关联的值是一个固定的对象。
2. **不包含重复元素**：`HashSet` 不允许存储重复的元素。如果尝试将一个已存在的元素添加到 `HashSet` 中，该添加操作不会改变集合。
3. **无序性**：`HashSet` 不保证元素的顺序，即元素的顺序可能会随着时间的推移而改变。
4. **快速查找**：由于 `HashSet` 基于哈希表，它提供了快速的查找操作，查找操作的时间复杂度为 O(1)。
5. **线程安全性**：`HashSet` 不是线程安全的。如果需要线程安全的集合，可以使用 `Collections.synchronizedSet` 方法包装 `HashSet` 或使用 `ConcurrentHashMap` 新增的 `newKeySet()` 方法。
6. **迭代器**：`HashSet` 提供了迭代器，可以方便地遍历集合中的所有元素。
7. **散列和碰撞**：`HashSet` 使用对象的 `hashCode()` 方法来计算哈希值，该哈希值随后用于确定对象在哈希表中的存储位置。如果两个对象的哈希值相同，会发生碰撞，`HashSet` 通过链表或红黑树（在 Java 8 及以后版本中）解决冲突。
8. **空元素**：`HashSet` 不允许空（null）元素，尝试添加空元素将抛出 `NullPointerException`。

### HashSet、LinkedHashSet 和 TreeSet 的区别

`HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 Java 中 `Set` 接口的实现，它们各自有不同的特性和用途。以下是它们之间的主要区别：

1. **基于的数据结构**：
  - `HashSet`：基于 `HashMap` 实现，使用哈希表存储元素。
  - `LinkedHashSet`：也是基于 `HashMap` 实现，但它维护了一个双向链表来保存插入顺序，因此可以按照元素的插入顺序遍历集合。
  - `TreeSet`：基于红黑树（一种自平衡二叉搜索树）实现，可以保持元素的排序。
2. **元素排序**：
  - `HashSet` 和 `LinkedHashSet` 不保证元素的有序性。
  - `TreeSet` 保证了元素处于排序状态，可以是自然排序，也可以是通过 `Comparator` 提供的顺序。
3. **元素遍历顺序**：
  - `HashSet`：无序。
  - `LinkedHashSet`：按照元素的插入顺序遍历。
  - `TreeSet`：按照自然排序或定制排序顺序遍历。
4. **性能**：
  - `HashSet` 和 `LinkedHashSet` 提供了接近 O(1) 的时间复杂度来添加、删除和查找元素。
  - `TreeSet` 的添加、删除和查找操作的时间复杂度是 O(log n)，但提供了有序的元素访问。
5. **内存占用**：
  - `HashSet` 通常比 `LinkedHashSet` 使用更少的内存，因为后者需要额外的链表来维护元素的顺序。
  - `TreeSet` 可能比其他两者使用更多的内存，因为它需要存储红黑树的节点。
6. **线程安全性**：
  - 这三个类都不是线程安全的。如果需要线程安全的集合，可以使用 `Collections.synchronizedSet` 方法包装它们，或者使用并发集合类，如 `ConcurrentSkipListSet` 作为线程安全的 `TreeSet` 替代品。
7. **空元素**：
  - `HashSet` 和 `LinkedHashSet` 都不允许空（null）元素。
  - `TreeSet` 也不允许空元素。
8. **使用场景**：
  - `HashSet`：适用于不需要元素有序性的场景，并且需要快速查找和插入操作。
  - `LinkedHashSet`：适用于需要保持元素插入顺序的场景。
  - `TreeSet`：适用于需要元素有序的场景，如需要排序的集合或需要执行范围查询的集合。

## 那些集合类是线程安全的？哪些不安全？

### 线程安全的集合类：

1. **`Collections.synchronizedList(List<T> list)`**：返回一个线程安全的 `List` 包装器。
2. **`Collections.synchronizedSet(Set<T> s)`**：返回一个线程安全的 `Set` 包装器。
3. **`Collections.synchronizedMap(Map<K, V> m)`**：返回一个线程安全的 `Map` 包装器。
4. **`CopyOnWriteArrayList`**：一种线程安全的变长数组。
5. **`CopyOnWriteArraySet`**：一种线程安全的 `Set` 实现，基于 `CopyOnWriteArrayList`。
6. **`ConcurrentHashMap`**：一种线程安全的 `HashMap` 实现，适用于高并发环境。
7. **`ConcurrentSkipListMap`**：一种线程安全的 `NavigableMap` 实现，类似于 `TreeMap`，但更适合并发环境。
8. **`ConcurrentSkipListSet`**：一种线程安全的 `NavigableSet` 实现，类似于 `TreeSet`，但更适合并发环境。

### 非线程安全的集合类：

1. **`ArrayList`**：非线程安全的动态数组实现。
2. **`LinkedList`**：非线程安全的双向链表实现。
3. **`HashSet`**：非线程安全的基于哈希表的 `Set` 实现。
4. **`LinkedHashSet`**：非线程安全的基于哈希表和链表的 `Set` 实现，保持插入顺序。
5. **`TreeSet`**：非线程安全的基于红黑树的 `Set` 实现，元素自动排序。
6. **`HashMap`**：非线程安全的基于哈希表的 `Map` 实现。
7. **`LinkedHashMap`**：非线程安全的基于哈希表和链表的 `Map` 实现，保持键的插入顺序。
8. **`TreeMap`**：非线程安全的基于红黑树的 `Map` 实现，键值对按键排序。

### 注意事项：

- 使用非线程安全的集合类时，如果需要线程安全，可以手动同步它们的部分操作，或者使用 `Collections` 类提供的同步包装器。
- `Vector` 和 `Hashtable` 是遗留类，它们的方法都是同步的，因此是线程安全的，但由于性能原因，通常不推荐使用。
- `CopyOnWriteArrayList` 和 `CopyOnWriteArraySet` 特别适合于读多写少的场景，因为它们在写操作时会复制整个底层数组，这可能会导致性能问题。
