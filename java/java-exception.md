# Java异常处理

标签（空格分隔）： Java

---

## Java异常处理

1. 在定义一个方法的时候可以使用throws关键字声明，使用throws声明的方式表示此方法不处理异常，而是将异常抛给方法的调用者处理
2. throw关键字抛出一个异常，抛出的时候直接抛出异常类的实例化对象即可
3. 如果在子类中覆盖了超类的一个方法，子类方法中声明的已检查异常范围不能超过超类方法中声明的异常范围；如果该子类没有抛出异常，那么它必须捕获方法代码中出现的每一处已检查异常

> **抛出还是捕获？**
应捕获那些知道如何处理的异常，而将那些不知道怎样处理的异常传递出去（在方法首部使用throws说明符）

<br />

> 应该抛出异常的情况：
    1. 调用一个抛出已检查异常的方法，如FileInputStream构造器
    2. 程序运行时发现错误，并利用throw语句抛出一个已检查异常
    3. 程序出现错误，如a[-1]=0将会抛出一个ArrayIndexOutOfBoundsException这样的未检查异常
    4. Java虚拟机和运行时库出现的内部异常

总结：一个方法应声明所有可能抛出的已检查异常，而未检查异常要么不可控制（error），要么就应该避免发生（RuntimeException）