# Java从入门到精通

标签（空格分隔）： Java

---

[TOC]

## 5. 数组

### 5.1 一维数组

数组是具有**<span style="color: red;">相同数据类型</span>**的一组数据的集合。

> 注：基本数据类型不是对象，但是由基本数据类型组成的数组是对象。

数组的数据类型取决于数组元素类型，可以是Java中任意的数据类型。

#### 5.1.1 声明一维数组

```java
// 声明一维数组
int[] arr;
// 或
String str2[];
```

Java中若要访问数组，应先为其分配内存空间，必须指明数组的长度。

```java
// 在[]中指明数组的长度，一般情况下，长度为最大下标+1。（下标从0开始计算）
arr = new int[5];
```

#### 5.1.2 初始化一维数组

```java
// 第一种方式
int[] arr1 = new int[]{1, 2, 3, 4, 5};
// 第二种方式
int[] arr2 = {1, 2, 3, 4, 5};
```

> 初始化数组的时候可以省略new操作符和数组的长度，编译器将根据初始值的数量来自动计算并分配内存；
若指定的数组长度大于初始值，则空余的位置将以“0”或null等填位。

### 5.2 多维数组

当数组元素依然为数组时，就产生了多维数组。
最常用的多维数组为二维数组，在此以二维数组举例。

#### 5.2.1 声明多维数组

```java
// 声明二维数组
int[][] arr;
// 或
String str2[][];
```

为二维数组分配内存
```java
arr = new int[1][2];
```

> 为二维数组分配内存时，可对其每一个一维数组单独分配内存，且分配的内存可以不相同。eg：

```java
int[][] a = new int[2][];
a[0] = new int[2];
a[1] = new int[3];
```

#### 5.2.2 初始化多维数组

```java
int[][] a = {{1, 2}, {11, 12}};
```

或者通过循环赋值。


### 5.3 数组的基本操作

#### 5.3.1 数组的遍历

数组的遍历一般通过for循环实现：

```java
int[] day = new int[] { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
for (int i = 0; i < day.length; i++) {
    System.out.println((i + 1) + "月有" + day[i] + "天" + "\t");
}
```

#### 5.3.2 数组的排序

可使用Arrays.sort(T[] a, Comparator<? super T> c)

```java
class Person {
    String name;
    int age;

    public Person(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }
}

class PersonAgeComparator implements Comparator<Person> {
    @Override
    public int compare(Person o1, Person o2) {
        return o1.age - o2.age;
    }
}

public class Array5_3 {
    public static void main(String[] args) {
        Person p1 = new Person("yhshen", 23);
        Person p2 = new Person("Chris", 24);
        Person p3 = new Person("mwei", 22);

        Person[] pArray = { p1, p2, p3 };
        printPerson(pArray);

        Arrays.sort(pArray, new PersonAgeComparator());
        printPerson(pArray);
    }

    private static void printPerson(Person[] pArray) {
        for (Person person : pArray) {
            System.out.println(person.name + " is at " + person.age);
        }
        System.out.println();
    }
}
```

#### 5.3.3 数组的复制

Arrays.copyOf(arr, int newLength)

Arrays.copyOfRange(arr, int fromIndex, int toIndex)



## 6. 字符串

### 6.1 字符串的声明

字符串的声明有两种方式：
1. 同基本类型一样，使用双引号来进行声明；
2. 通过String类的构造器进行声明。

> **两种声明方式的区别：**
使用方式一声明并初始化，JVM会先在字符串常量池中查找是否已存在相同的字符串，因此若有String a = "test"; String b = "test";，a==b返回true
而使用方式二声明并初始化，则无论是否存在都会new一个新的字符串常量。

### 6.2 字符串的常见操作

#### 6.2.1 字符串的拼接

字符串可以和字符串类型或其他任意类型的数据进行拼接操作，都使用“+”操作符。
在和其他类型的数据拼接时，编译器会将该类型自动转换为String类型。

#### 6.2.2 获取字符串信息

```java
// 计算字符串长度
String str1 = "qwertyuiopasdfghjklzxcvbnm";
System.out.println(str1.length());

// 获取指定字符的索引位置
// indexOf() 和 lastIndexOf()使用类似
System.out.println(str1.indexOf("q"));          // 0
System.out.println(str1.indexOf("qwe"));        // 0
System.out.println(str1.indexOf("qwes"));       // -1

// 获取指定索引位置的字符
System.out.println(str1.charAt(25));            // m
```

#### 6.2.3 字符串的替换

```java
// Java中字符串替换主要有replace()、replaceAll()、replaceFirst()三个方法
// 其中后两个的第一个参数是正则表达式的字符串形式
// 因此在使用时要注意元字符、转义符

String s = "my.test.txt";
System.out.println(s.replace(".", "#"));        // my#test#txt
System.out.println(s.replaceAll(".", "#"));     // ###########
System.out.println(s.replaceFirst(".", "#"));   // #y.test.txt
```

#### 6.2.4 字符串的比较

应首先使用equals()或equalsIgnoreCase()方法，而非“==”

### 6.3 正则表达式


### 6.4 字符串生成器

StringBuffer、StringBuilder



## 7. 类和对象

> Java类执行顺序
1. 父类的静态成员赋值和静态块
2. 子类的静态成员和静态块
3. 父类的构造方法
4. 父类的成员赋值和初始化块
5. 父类的构造方法中的其它语句
6. 子类的成员赋值和初始化块
7. 子类的构造方法中的其它语句


## 工程结构管理

### Build Path

Build Path一般包括：

* JRE运行时库
* 第三方功能扩展库（通产为jar包）
* 其他的工程
* 其他的源代码或Class文件

通过Build Path，可以更好地管理Java工程所包含的资源，让工程结构清晰合理。
反之，随着代码与功能的增加，工程机构会变得杂乱无章，难以管理。
