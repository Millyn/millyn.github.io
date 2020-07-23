---
title: 牛客Java基础刷题解析
date: 2017-07-08 00:29:16
categories: Java
tags:
- JAVA
- 牛客刷题
- 编程技术
- String
- Stringbuff
---

牛客是个不错的刷题地方，让我想起当时考驾照的时候那种刷题感觉，
可对于开发来说的题目和考驾照差别太多。
当然，这些题目里也有那种题目意义混淆, 和一些让你犯小错误的地方。
今天刷了点题目，做点总结。

<!-- more -->

# 关于下列程序段的输出结果，说法正确的是：（ ）

```
public class MyClass{
static int i;
public static void main(String argv[]){
System.out.println(i);
}
}
```

正确答案 D

A. 有错误，变量i没有初始化。
B. null
C. 1
D. 0

## 解析

首先我们可以看到调用的i是一个static变量, static修饰过的变量和方法都会在程序执行时优先被初始化.
所以i不可能没有被初始化, 那么null并不是int类型的”空值表达”.
i没有被赋予1这个值, 所以也不是1.
那么int类型在定义后没有被赋值的情况下, 默认是”0”这个空值去代表它.

# 关于抽象类和接口叙述正确的是? ()

正确答案 C

A. 抽象类和接口都能实例化的
B. 抽象类不能实现接口
C. 抽象类方法的访问权限默认都是public
D. 接口方法的访问权限默认都是public

## 解析

抽象类不可以被实例化
抽象类可以实现接口
抽象类的方法默认权限是protected
接口方法的访问权限默认都是public

# 关于static说法不正确的是()

正确答案 D

A. 可以直接用类名来访问类中静态方法(public权限)
B. 静态块仅在类加载时执行一次
C. static方法就是没有this的方法
D. 不可以用对象名来访问类中的静态方法(public权限)

## 解析

因为static是静态的, 所以他不需要用对象去初始化, 也就没有所谓用”对象名”来访问”类”

# String str1 = “abc”，“abc”分配在内存哪个区域？

正确答案 C

A. 堆
B. 栈
C. 字符常量区
D. 寄存器

## 解析
用new创建的对象都在堆里.
栈里存放的是基本数据类型和临时变量
在Java里, 说到String是不可变的, 原因就在于String的值会是”常量”, 常量都知道是不可变.
所以在String的值里使用”+”这种操作符会有效率底下一说的原因就在于,
每一次”+”出来的新值, 实际上是产生了一个新的常量, 而不是对旧的值进行修改.

# 线程安全的map在JDK 1.5及其更高版本环境 有哪几种方法可以实现?

正确答案: C D

A. Map map = new HashMap()
B. Map map = new TreeMap()
C. Map map = new ConcurrentHashMap();
D. Map map = Collections.synchronizedMap(new HashMap());

## 解析
HashMap和TreeMap的线程都不是安全的.
ConcurrentHashMap是安全线程.
Collections.synchronizedMap可以使得HashMap变为一个安全的线程.
因为synchronized在英语里表达的就是”同步的”

# Java语言支持程序并行执行的多线程编程，实现了一般传统语言难以实现的某些功能；Java的线程是通过java.lang. 1 类来实现的，在该类中封装了虚拟的 2

正确答案:
1= Thread 即为 java.lang.Thread
2= CPU

# 实现或继承了Collection接口的是（）

正确答案: B C E

A. Map
B. List
C. Vector
D. Iterator
E. Set

## 解析

Map是接口, 他的定义是public interface Map<K,V>
list也是接口, 定义是public interface List<E> extends Collection<E>
Vector继承与List接口
Iterator接口, 他的定义是public interface Iterator<E>
Set接口, 他的定义是public interface Set<E> extends Collection<E>
现在就很明显的能表达, B C E 是正确答案

# 以下集合对象中哪几个是线程安全的？

正确答案: B C D

A. ArrayList
B. Vector
C. Hashtable
D. Stack

## 解析
ArrayList并不是安全线程, Vector和HashTable是安全线程, Stack继承与Vector所以它也是安全线程.

# 类 ABC 定义如下： 将以下哪个方法插入行 3 是不合法的。（ ）。

```
public  class  ABC{
    public  int  max( int  a, int  b) {   }
}
```

正确答案: B

A. public float max(float a, float b, float c){ }
B. public int max (int c, int d){ }
C. public float max(float a, float b){ }
D. private int max(int a, int b, int c){ }

## 解析
在定义方法时的形参, 编译器是不认形参名字只认形参的类型的.
public int max (int c, int d) 和 public int max (int a, int b)
对于编译器来说都是接受2个int类型的参数, 所以如果同时有这2个方法出现.
编译器接受2个int参数的调用时, 就不知道该去找哪一个方法才是正确的.

# 检查程序，是否存在问题，如果存在指出问题所在，如果不存在，说明输出结果。

```
public class HelloB extends HelloA 
{
 public HelloB() {
     System.out.println("I’m B class");
 }
 static {
     System.out.println("static B");
 }
 public static void main(String[] args) {
     new HelloB();
 }
}
class HelloA
{
 public HelloA(){
     System.out.println("I’m A class");
 }
 static{
     System.out.println("static A");
 }
}
```

正确答案: C

|答案|结果|
|:--|:--|
|A|static A / I’m A class / static B /I’m B class|
|B|I’m A class / I’m B class / static A / static B|
|C|static A / static B / I’m A class / I’m B class|
|D|I’m A class / static A / I’m B class / static B|

## 解析
这一题主要是讲static和初始化对象时的流程.
我们知道的执行顺序是
HelloB里的main方法new HelloB();那么HelloB继承与HelloA, 所以
HelloA(构造方法)执行顺序要比HelloB(构造方法)前.
但HelloA和B中都有static方法, 那么static的优先级又会比构造方法高
所以这段代码的执行顺序是
HelloA_Static->HelloB_Static->HelloA_Constructor->HelloB_Constructor

# String与StringBuffer的区别。

正确答案: A

A. String是不可变的对象，StringBuffer是可以再编辑的
B. String是常量，StringBuffer是变量
C. String是可变的对象，StringBuffer是不可以再编辑的
D. 以上说法都不正确

## 解析
String是不可变的对象原因是, String的值是常量, 常量就是不变值.
关于StringBuffer, 可以来解析一下StringBuffer的源码

```
public StringBuffer(String str) {
    super(str.length() + 16);
    append(str);
}
```

StringBuffer的构造器传入一个str时, 会把传入这个str的length(长度)+16在父类进行构造方法的调用.

```
AbstractStringBuilder(int capacity) {
    value = new char[capacity];
}
```

这个是super()父类的构造方法.
可以看到这个str.length() +16的值传进来后是根据这个值创建了一个char[]对象
在看append(str);
在StringBuffer里是复写了append这个方法.

```
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
```

`toStringCache`是定义的一个全局变量`private transient char[] toStringCache;`
然后在调用父类的append

```
public AbstractStringBuilder append(String str) {
    if (str == null)
        return appendNull();
    int len = str.length();
    ensureCapacityInternal(count + len);
    str.getChars(0, len, value, count);
    count += len;
    return this;
}
```

`ensureCapacityInternal()` 这里我们传入了count + len , count目前是只有声明, 没有内容也就是0.
0+len, 如果string是”ABC” 那么len是3. 即等于传递了3这个参数过去.

```
private void ensureCapacityInternal(int minimumCapacity) {
        // overflow-conscious code
        if (minimumCapacity - value.length > 0) {
            value = Arrays.copyOf(value,
                    newCapacity(minimumCapacity));
        }
    }
```

到这里ensureCapacityInternal这里, 进行第一句if判断
`if (minimumCapacity - value.length > 0) {`
我们还是作刚才那个例子, 3 - value.length > 0 这个value.length是什么?
value就是在最早`StringBuffer`的构造方法, 会创建一个char[str.length+16]的char对象
3 - 19 > 0 是否定的, 所以不执行.
接下来执行`str.getChars(0, len, value, count);`
已知len=3, value=char[str.length+16],count=0;

```
public void getChars(int srcBegin, int srcEnd, char dst[], int dstBegin) {
    if (srcBegin < 0) {
        throw new StringIndexOutOfBoundsException(srcBegin);
    }
    if (srcEnd > value.length) {
        throw new StringIndexOutOfBoundsException(srcEnd);
    }
    if (srcBegin > srcEnd) {
        throw new StringIndexOutOfBoundsException(srcEnd - srcBegin);
    }
    System.arraycopy(value, srcBegin, dst, dstBegin, srcEnd - srcBegin);
}
```

有了数据, 我们把形参可以认为是实参
srcBegin = 0
srcEnd = 3
dst[] = char[19]
dstBegin = 0
一步一步执行判断, 可以看到判断如果是true那么肯定会出错, throw 一个错误.
那么如果错误都过了一边之后, 进入到最后一个环节, arraycopy.
`System.arraycopy(value, srcBegin, dst, dstBegin, srcEnd - srcBegin);`
执行拷贝

```
public static native void arraycopy(Object src,  int  srcPos,Object dest, int destPos,int length);
```

这5个参数是
原obj，原位置，目标obj ， 目标位置，长度

到这里, 我们Stringbuffer就已经赋值完成了.
通过这些我们就知道Stringbuffer为什么是可变的, 首先他并不是传统的”字符串”
而是通过char类型的数组拼接起来的也就是为什么我们输出Stringbuffer类型字符串时需要使用toString()方法.

```
@Override
public synchronized String toString() {
    if (toStringCache == null) {
        toStringCache = Arrays.copyOfRange(value, 0, count);
    }
    return new String(toStringCache, true);
}
```