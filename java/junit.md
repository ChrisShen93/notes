# JUnit

标签（空格分隔）： Java JUnit 单元测试

---

Junit注解提供了资源初始化和回收方法：@Before、@BeforeClass、@After、@AfterClass。

### 参数化测试

1. 必须使用@RunWith指定运行器为Parameterized.class
2. 声明测试中使用到的实例变量
3. 提供一个@Parameters注解的方法，且其签名必须为@Parameters public static java.util.Collection，无任何参数

```java
package learnJUnit.chapter02;
import static org.junit.Assert.assertEquals;
import java.util.Arrays;
import java.util.Collection;
import learnJUnit.chapter01.Calculator;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;
@RunWith(value = Parameterized.class)
public class ParameterizedTest {

    private double expected;
    private double valueOne;
    private double valueTwo;

    @Parameters
    public static Collection<Integer[]> getTestParameters() {
        return Arrays.asList(new Integer[][] {
            {2, 1, 1},
            {3, 2, 1},
            {4, 3, 1}
        });
    }
    /**
     * @param expected
     * @param valueOne
     * @param valueTwo
     */
    public ParameterizedTest(double expected, double valueOne, double valueTwo) {
        super();
        this.expected = expected;
        this.valueOne = valueOne;
        this.valueTwo = valueTwo;
    }

    @Test
    public void sum() {
        Calculator cal = new Calculator();
        assertEquals(expected, cal.add(valueOne, valueTwo), 0);
    }
}
```

### 组合测试Suite

```java
@RunWith(value = Suite.Class)
@SuiteClasses(value = {xxx.class, yyy.class})
```

在没有显示创建Suite的情况下，测试运行器会自动创建一个Suite扫描测试类，找出所有以@Test注释的方法。默认的Suite会在内部为每个@Test方法创建一个测试类的实例，以便在JUnit独立运行每个@Test并避免潜在的负面影响。
