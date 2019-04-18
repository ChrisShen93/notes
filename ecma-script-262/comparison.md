[本文参考](http://www.ecma-international.org/ecma-262/7.0/index.html#sec-abstract-equality-comparison)

## 非严格比较

The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is the same as Type(y), then
  Return the result of performing Strict Equality Comparison x === y.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String, return the result of the comparison x == ToNumber(y).
5. If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.
6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
8. If Type(x) is either String, Number, or Symbol and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
9. If Type(x) is Object and Type(y) is either String, Number, or Symbol, return the result of the comparison ToPrimitive(x) == y.
10. Return false.

## 严格比较

The comparison x === y, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is different from Type(y), return false.
2. If Type(x) is Number, then
  1. If x is NaN, return false.
  2. If y is NaN, return false.
  3. If x is the same Number value as y, return true.
  4. If x is +0 and y is -0, return true.
  5. If x is -0 and y is +0, return true.
  6. Return false.
3. Return SameValueNonNumber(x, y).

## SameValueNonNumber(x, y)

The internal comparison abstract operation SameValueNonNumber(x, y), where neither x nor y are Number values, produces true or false. Such a comparison is performed as follows:

1. Assert: Type(x) is not Number.
2. Assert: Type(x) is the same as Type(y).
3. If Type(x) is Undefined, return true.
4. If Type(x) is Null, return true.
5. If Type(x) is String, then
  1. If x and y are exactly the same sequence of code units (same length and same code units at corresponding indices), return true; otherwise, return false.
6. If Type(x) is Boolean, then
  1. If x and y are both true or both false, return true; otherwise, return false.
7. If Type(x) is Symbol, then
  1. If x and y are both the same Symbol value, return true; otherwise, return false.
8. Return true if x and y are the same Object value. Otherwise, return false.
