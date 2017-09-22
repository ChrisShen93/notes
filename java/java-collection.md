# Java集合

标签（空格分隔）： Java

---

## 集合接口

### Java类库中的集合接口和迭代器接口

#### 1. 迭代器

```java
Collection<String> c = ...;
Iterator<String> iter = c.iterator();
while(iter.hasNext()) {
    String element = iter.next();
    // do something with element;
}
```

或者

```java
Collection<String> c = ...;
for(String element : c) {
    // do something with element;
}
```
Collection接口扩展了Iterable接口，因此标准类库中的所有集合都可使用for each遍历集合。

#### 2. 删除元素

```java
Collection<String> c = ...;
Iterator<String> iter = c.iterator();
iter.next();
iter.remove();  // 每一次调用remove()前都必须先调用next()
```

## 具体的集合

| 集合类型 | 描述 |
| --- | --- |
| ArrayList | 可动态增长或缩减的索引序列 |
| LinkedList | 可在任何位置进行高效插入或删除操作的有序序列 |
| ArrayDeque | 用循环数组实现的双端序列 |
| HashSet | 没有重复元素的无序集合 |
| TreeSet | 有序集 |
| EnumSet | 包含枚举类型值的集合 |
| LinkedHashSet | 可记住元素插入位置的集合 |
| PriorityQueue | 允许高效删除最小元素的集合 |
| HashMap | 存储键值对的数据结构 |
| TreeMap | 键值有序排列的映射表 |
| EnumMap | 键值属于枚举类型的映射表 |
| LinkedHashMap | 可记住键值对添加次序的映射表 |
| WeakHashMap | 当值无用时可被ＧＣ回收的映射表 |
| IdentityHashMap | 使用 == 而非 equals 比较键值对的映射表 |

### 链表

Java中所有的链表都是双向链表。

```java
// 在一个链表中添加三个元素，再删除第二个元素
List<String> staff = new LinkedList();
staff.add("Mac OSX");
staff.add("Linux");
staff.add("Windows");
Iterator iter = staff.iterator();
String first = iter.next(); // 访问第一个元素
String second = iter.next(); // 访问第二个元素
iter.remove(); // 删除最后访问的元素
```

在有序表中的指定位置插入/删除元素，可以使用ListIterator()迭代器。

在编写程序时，可以给集合添加许多迭代器，但只应给一个迭代器赋予可写权限，其余为只读。

### 数组列表

当需要随机访问时以此代替链表。

可使用Vector代替ArrayList。Vector类的所有方法都是同步的。

### 散列集

hash code

### 树集

树集是一个有序集合。可以以任意顺序将元素插入集合中。遍历时，每个值将自动地按照排序后的顺序呈现。


### 小结

* 优先使用链表。使用链表的唯一理由是尽可能减少在链表中间插入或删除元素所付出的代价。若链表中只有少许几个元素，完全可以使用ArrayList。
* 应避免使用整数索引表示链表中的所有方法（即不使用get(n)）。若需对集合进行随机访问，应使用数组或ArrayList。
