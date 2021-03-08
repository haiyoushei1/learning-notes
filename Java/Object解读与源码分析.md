Object类作为所有类的超类，有三个很重要的函数 
# 1. equals()方法  

Object类中equals方法用于检测一个对象是否等于另一个对象。
底层代码是使用的==判定。这个对于大部分情况都是没有作用的，要根据具体情况做修改。
```java
	public boolean equals(Object obj) {
        return (this == obj);
    }
```
equals方法的特性：
1. 自反性：除空引用外，对象equals自己应返回true
2. 对称性：任意引用，x.equals(y)的返回结果，应于y.equals(x)结果相同
3. 传递性：任意引用，x.equals(y)返回true,y.equals(z)返回true，z.equals(x)应返回true
4. 一致性：若两对象没有变化，那么无论两对象equals多少次，结果都相同
5. 非空的对象，equals(null)应返回false

所有的equals都要同时满足这些特性。

如何编写一个完美的equals：
1. 显示参数命名otherObject，等下转换为other变量
2. 检测this与otherObject是否引用同一个对象。（这个如果直接相同可以后面就省下了）
3. 检测otherObject是否为null。检测对象是否为空很重要
4. 比较this与otherObject是否是同一个类。如果两个对象，类都不同，那没得相同
* 以上的这几步可以放在父类中，去super调(这不就是提高复用)。
* 这里可以选择getClass与instanceof
* getClass应用在equals比较子类特有的属性时用，因为getClass不考虑继承的情况
* instanceof应用在equals在后面子类比较的都是父类的属性。instanceof考虑继承的情况
5. 将otherObject转换为相应的类 类型变量
```java
ClassName other = (ClassName)otherObject
```
6. 对需要的域进行比较，使用==比较基本类型，equals比较类对象

---
# 2. hashcode()方法  
Object类的hashcode默认返回对象的储存地址  
equals有修改hashcode必须修改，原因：  
  
这个修改是为了在使用哈希表的时候不会出错，比如： 
```java
class Student{
	String name;
    int ID;
}
```
# 重写equals必需重写hashcode
如果不重写equals，那么在调用相同是就是Object的equals，比较对象地址的。  
假设equals ID  
如果只重写equals，不重写hashcode，那么修改ID，这两个还是相同  
这个是设计问题，但是在使用哈希表时，比如HashSet有去重功能，先检查hashcode是否相同，再比较equals，如果没改，那就有出现去重失败的情况。  
如果不用哈希，那根本就用不到hashcode，随便equals比较属性。但是用哈希表的目的就是通过哈希来简化查找，那么只有hashcode与equals比较的是同一种属性时，才能够充分发挥哈希的作用
hashcode的主要目的就是为了用集合能好用，并且设计要求equals true，hashcode true;hashcode false，equals false。
   
 注：hashcode相同，可能equals不相同，因为存在哈希冲突

---
# 3. toString()方法  
对于每个类必有的一个函数  
```java
    public String toString() {
        return getClass().getName() + "@" + 		Integer.toHexString(hashCode());
    }
```
在Object类中toString则是获取类的名字+类的hashcode  
(即自建类若只重写hashcode，而不重写toString，在toString时调用自建类的hashcode函数)

重写toString一般是：
```java
	return getClass().getName() + "[属性1=" + 属性1 + "属性2=" + 属性2 + "]";
```
所有的转化字符串的时候都会调用toString。




























