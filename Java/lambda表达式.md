 lambda表达式就是一个可以传递的代码块。如果需要一个多次执行的代码块，正常使用思路就是我搞一个接口，完了类实现，创建对象，赋给接口，调用。太麻烦了。
 有lambda表达式后，直接将代码段赋给接口
 主要是在不确定要传递什么代码时，
 ```java
Comparator<String> comp =
    	(first, second) -> {first.length() - second.length()};  // 编译器会推导first与second是String类型，不需要修饰

//没有参数：
() -> {*********}

//一个参数,如果可以推导出可以不加小括号，目前阶段觉得还是都加上吧，按照标准形式写
```
注：lambda表达式在返回值上，如果用了控制语句，那么每个分支都要返回值，即：分支如果返回值，要保证代码块即使不执行该分支也应有返回值

---
# 函数式接口：如果一个接口只有一个方法，并且需要这种接口的对象时，用lambda表达式，那么这个接口被叫做函数式接口
```java
Arrays.sort(words, (first, second) -> first.length() - second.length());
public static <T> void sort(T[] a, Comparator<? super T> c)
```

---
#方法引用：当有现成的方法时，使用方法引用实现lambda表达式的效果
```java
System.out::println 等价于 x -> System.out.println(x)
Arrays.sort(str, Strings::compareToIgnoreCase)
```
用::操作符分隔方法名与对象或类名
```java
object::instanceMethod
Class::staticMethod
Class::instanceMethod
```
前两种与方法引用等价于提供方法参数的lambda表达式
第三种情况，第一个参数会成为方法的目标
String::compareToIgnoreCase ----->
(x, y) -> x.compareToIgnoreCase(y)
