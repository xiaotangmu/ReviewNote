#                 [    Java常用类（一）之Object类详解    ](https://www.cnblogs.com/zhangyinhua/p/7715486.html)            

**阅读目录(Content)**

- [一、clone()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label0)

- - [1.1、clone与copy的区别](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_lab2_0_0)
  - [1.2、Shallow Clone与Deep Clone](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_lab2_0_1)
  - [1.3、clone方法的保护机制](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_lab2_0_2)
  - [1.4、clone方法的使用](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_lab2_0_3)

- [二、toString()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label1)

- [三、getClass()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label2)

- [四、finalize()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label3)

- [五、equals()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label4)

- [六、hashCode()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label5)

- [七、wait()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label6)

- [八、notify()方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label7)

- [九、notifyAll方法](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_label8)

大家都知道Object是所有类的父类，任何类都默认继承Object

理论上Object类是所有类的父类，即直接或间接的继承java.lang.Object类。由于所有的类都继承在Object类，因此省略了extends Object关键字。
该类中主要有以下方法: toString(),getClass(),equals(),clone(),finalize(), 其中toString(),getClass(),equals是其中最重要的方法。

注意：

​    Object类中的getClass(),notify(),notifyAll(),wait()等方法被定义为final类型，因此不能重写。

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 一、clone()方法

保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。

主要是JAVA里除了8种基本类型传参数是值传递，其他的类对象传参数都是引用传递，我们有时候不希望在方法里讲参数改变，这是就需要在类中复写clone方法（实现深复制）。

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023104810066-929720224.png)

 创建并返回此对象的一个副本。“副本”的准确含义可能依赖于对象的类。

## 1.1、clone与copy的区别

​    假设现在有一个Employee对象，Employee tobby =new Employee(“CMTobby”,5000)

​    通常我们会有这样的赋值Employee cindyelf=tobby，这个时候只是简单了copy了一下reference，cindyelf和tobby都指向内存中同一个object，

​    这样cindyelf或者tobby的一个操作都可能影响到对方。打个比方，如果我们通过cindyelf.raiseSalary()方法改变了salary域的值，那么tobby通过

​    getSalary()方法得到的就是修改之后的salary域的值，显然这不是我们愿意看到的。我们希望得到tobby的一个精确拷贝，同时两者互不影响，这时候

​    我们就可以使用Clone来满足我们的需求。Employee cindy=tobby.clone()，这时会生成一个新的Employee对象，并且和tobby具有相同的属性值和方法。

## 1.2、Shallow Clone与Deep Clone

​    Clone是如何完成的呢？Object在对某个对象实施Clone时对其是一无所知的，它仅仅是简单地执行域对域的copy，这就是Shallow Clone。这样，问题就来了咯。

​    以Employee为例，它里面有一个域hireDay不是基本数据类型的变量，而是一个reference变量，经过Clone之后就会产生一个新的Date型的reference，

​    它和原始对象中对应的域指向同一个Date对象，这样克隆类就和原始类共享了一部分信息，而这样显然是不利的，过程下图所示：

 　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023104906207-1097755810.png)

​    这个时候我们就需要进行deep Clone了，对那些非基本型别的域进行特殊的处理，例如本例中的hireDay。我们可以重新定义Clone方法，对hireDay做特殊处理，如下代码所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<span data-wiz-span="data-wiz-span" style="font-size: 1.167rem;">   class Employee implements Cloneable  {  
        public Object clone() throws CloneNotSupportedException  {  
         Employee cloned = (Employee) super.clone();  
      cloned.hireDay = (Date) hireDay.clone()  
      return cloned;  
        }  
   }</span>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 1.3、clone方法的保护机制

​    在Object中Clone()是被声明为protected的，这样做是有一定的道理的，以Employee。

​    类为例，通过声明为protected，就可以保证只有Employee类里面才能“克隆”Employee对象

## 1.4、clone方法的使用

​    Clone()方法的使用比较简单，注意如下几点即可：
​         什么时候使用shallow Clone，什么时候使用deep Clone，这个主要看具体对象的域是什么性质的，基本型别还是reference variable

​        调用Clone()方法的对象所属的类(Class)必须implements Clonable接口，否则在调用Clone方法的时候会抛出CloneNotSupportedException

更加详细的解释：[点击查看](http://blog.csdn.net/zhangjg_blog/article/details/18369201#0-qzone-1-28144-d020d2d2a4e8d1a374a433f596ad1440)

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 二、toString()方法

Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象[哈希码](https://www.baidu.com/s?wd=%E5%93%88%E5%B8%8C%E7%A0%81&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHcvrjTdrH00T1YLuyDvPWnsuHIbrjRsn1Rz0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHTdrjfYPW0dn1fLPHT4PW0Y)的无符号十六进制表示组成。

该方法用得比较多，一般子类都有覆盖。

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105034285-916665602.png)

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/982b5b8e-367c-4599-b9b3-4284083522d5.png)

​    例子：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<span data-wiz-span="data-wiz-span" style="font-size: 1.167rem;">package com.cal.toString;  
  
public class Test1 {  
    public static void main(String[] args){  
        Object o1 = new Object();  
        System.out.println(o1.toString());  
    }  
}  </span>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

​        结果：java.lang.Object@7852e922

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 三、getClass()方法

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/7a2dd94c-80d7-4bfd-8245-6185c0be4e47.png)

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105112176-1319850217.png)

 返回次Object的运行时类类型。

不可重写，要调用的话，一般和getName()联合使用，如getClass().getName();

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 四、finalize()方法

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/1f3fef7f-2ca3-4827-abac-6c5dc800b60f.png)

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105136254-754577107.png)

该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。

Java允许在类中定义一个名为finalize()的方法。它的工作原理是：一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其finalize()方法。并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。

关于垃圾回收，有三点需要记住：

　　1、对象可能不被垃圾回收。只要程序没有濒临存储空间用完的那一刻，对象占用的空间就总也得不到释放。

　　2、垃圾回收并不等于“析构”。

　　3、垃圾回收只与内存有关。使用垃圾回收的唯一原因是为了回收程序不再使用的内存。

finalize()的用途：

　　无论对象是如何创建的，垃圾回收器都会负责释放对象占据的所有内存。这就将对finalize()的需求限制到一种特殊情况，即通过某种创建对象方式以外的方式为对象分配了存储空间。

　　不过这种情况一般发生在使用“本地方法”的情况下，本地方法是一种在Java中调用非Java代码的方式。

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 五、equals()方法

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/ca4252f4-c1ec-45e0-a3c3-da5d87c3136b.png)

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105208410-192032116.png)

Object中的equals方法是直接判断this和obj本身的值是否相等，即用来判断调用equals的对象和形参obj所引用的对象是否是同一对象，

所谓同一对象就是指内存中同一块存储单元，如果this和obj指向的hi同一块内存对象，则返回true,如果this和obj指向的不是同一块内存，则返回false。

注意：即便是内容完全相等的两块不同的内存对象，也返回false。

​          如果是同一块内存，则object中的equals方法返回true,如果是不同的内存，则返回false

​          如果希望不同内存但相同内容的两个对象equals时返回true,则我们需要重写父类的equal方法

​          String类已经重写了object中的equals方法（这样就是比较内容是否相等了）

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 六、hashCode()方法

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105256816-24840383.png)

返回该对象的哈希码值

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/33bc6bd0-847f-427f-bccf-47387f9e1ce9.png)

该方法用于哈希查找，可以减少在查找中使用equals的次数，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

一般必须满足obj1.equals(obj2)==true。可以推出obj1.hash- Code()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

如果不重写hashcode(),在HashSet中添加两个equals的对象，会将两个对象都加入进去。

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 七、wait()方法

1）wait()

　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105520926-284756973.png)

2)wait(long timeout)

　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105602644-1407424916.png)

3)wait(long timeout,int naos)

　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023105703582-1118717096.png)

　　什么意思呢？

　　　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023110123051-53486543.png)

　　方法中的异常：

　　　　![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023110228816-448528101.png)　　　

wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，

如果在规定时间内没有获得锁就返回。

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/5eae5ced-2a62-4c5e-aef2-829f46d7f123.png)

 ![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/e9c3a286-e9ca-4402-adc8-a25dfbc66c9c.png)

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

（1）其他线程调用了该对象的notify方法。

（2）其他线程调用了该对象的notifyAll方法。

（3）其他线程调用了interrupt中断该线程。

（4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 八、notify()方法

![img](file:///C:/Users/Shinelon/Documents/My%20Knowledge/temp/9187e79f-af20-4896-b6c5-cc29bb9f2e23/128/index_files/b650ee6c-c161-4fdd-98a3-8c9c7d790f93.png)

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023110323441-835706372.png)

该方法唤醒在该对象上等待的某个线程。

[回到顶部(go to top)](https://www.cnblogs.com/zhangyinhua/p/7715486.html#_labelTop)

# 九、notifyAll方法

![img](https://images2017.cnblogs.com/blog/999804/201710/999804-20171023110347160-1577147468.png)

该方法唤醒在该对象上等待的所有线程。