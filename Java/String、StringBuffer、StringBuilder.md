一、Java String 类——String字符串常量

字符串广泛应用 在Java 编程中，在 Java 中字符串属于对象，Java 提供了 String 类来创建和操作字符串。

需要注意的是，String的值是不可变的，这就导致每次对String的操作都会生成新的String对象，这样不仅效率低下，而且大量浪费有限的内存空间。我们来看一下这张对String操作时内存变化的图：

![img](https://img-blog.csdn.net/20180411091757991?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTEwMTE3Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们可以看到，初始String值为“hello”，然后在这个字符串后面加上新的字符串“world”，这个过程是需要重新在栈堆内存中开辟内存空间的，最终得到了“hello world”字符串也相应的需要开辟内存空间，这样短短的两个字符串，却需要开辟三次内存空间，不得不说这是对内存空间的极大浪费。为了应对经常性的字符串相关的操作，就需要使用Java提供的其他两个操作字符串的类——StringBuffer类和StringBuild类来对此种变化字符串进行处理。
二、StringBuffer 和 StringBuilder 类——StringBuffer、StringBuilder字符串变量

当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。

和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。

由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。

三者的继承结构

![img](https://img-blog.csdn.net/20180411092328691?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTEwMTE3Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

三者的区别：

![img](https://img-blog.csdn.net/20180411092400746?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTEwMTE3Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（1）字符修改上的区别（主要）

String：不可变字符串；

StringBuffer：可变字符串、效率低、线程安全；

StringBuilder：可变字符序列、效率高、线程不安全；

（2）初始化上的区别，String可以空赋值，后者不行，报错

    ①String

    StringBuffer s = null;   

    StringBuffer s = “abc”;   

    ②StringBuffer

    StringBuffer s = null; //结果警告：Null pointer access: The variable result can only be null at this location

    StringBuffer s = new StringBuffer();//StringBuffer对象是一个空的对象

    StringBuffer s = new StringBuffer(“abc”);//创建带有内容的StringBuffer对象,对象的内容就是字符串”

小结：（1）如果要操作少量的数据用 String；

（2）多线程操作字符串缓冲区下操作大量数据 StringBuffer；

（3）单线程操作字符串缓冲区下操作大量数据 StringBuilder（推荐使用）。



总结：末尾总是有福利，三者区别可参照下表：
String 	
String的值是不可变的，这就导致每次对String的操作都会生成新的

String对象，不仅效率低下，而且浪费大量优先的内存空间 	

不可变 



StringBuffer 	

StringBuffer是可变类，和线程安全的字符串操作类，任何对它指向的字符串的操作都不会产生新的对象。每个StringBuffer对象都有一定的缓冲区容量，当字符串大小没有超过容量时，不会分配新的容量，当字符串大小超过容量时，会自动增加容量 	

可变 

线程安全 	

多线程操作字符串 	



StringBuilder

可变类，速度更快

可变

线程不安全

单线程操作字符串



原文链接：https://blog.csdn.net/weixin_41101173/article/details/79677982