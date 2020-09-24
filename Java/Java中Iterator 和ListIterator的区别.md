原文链接：https://blog.csdn.net/a1439775520/article/details/95372292

# Java中Iterator 和ListIterator的区别

## 1.Iterator

Iterator的定义如下：

public interface Iterator {}
Iterator是一个接口，它是集合的迭代器。集合可以通过Iterator去遍历集合中的元素。Iterator提供的API接口如下：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195036358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)

forEachRemaining(Consumer<? super E> action)：为每个剩余元素执行给定的操作,直到所有的元素都已经被处理或行动将抛出一个异常

hasNext()：如果迭代器中还有元素，则返回true。

next()：返回迭代器中的下一个元素

remove()：删除迭代器新返回的元素。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195045795.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195054359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)

例子:

import java.util.*;
public class TestIterator {
    public static void main(String[] args) {
        ArrayList<String> a = new ArrayList<String>();
        a.add("aaa");
        a.add("bbb");
        a.add("ccc");
        System.out.println("Before iterate : " + a);
        Iterator<String> it = a.iterator();
        while (it.hasNext()) {
            String t = it.next();
            if ("bbb".equals(t)) {
                it.remove();
            }
        }
        System.out.println("After iterate : " + a);
    }
}

我们可以看到：首先往一个ArrayList里装了aaa，bbb，ccc，然后进行判断删除bbb，最后ArrayList里只剩 aaa，ccc。

我们来验证一下：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195130119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)

注意：

（1）Iterator只能单向移动。

（2）Iterator.remove()是唯一安全的方式来在迭代过程中修改集合；如果在迭代过程中以任何其它的方式修改了基本集合将会产生未知的行为。而且每调用一次next()方法，remove()方法只能被调用一次，如果违反这个规则将抛出一个异常。

## 2.ListIterator

ListIterator是一个功能更加强大的, 它继承于Iterator接口,只能用于各种List类型的访问。可以通过调用listIterator()方法产生一个指向List开始处的ListIterator, 还可以调用listIterator(n)方法创建一个一开始就指向列表索引为n的元素处的ListIterator。

我们先来看一段关于ListIterator的描述：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195140777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)

ListIterator接口定义如下：

Interface ListIterator<E>{}


包含的方法有：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195153547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)
由以上定义我们可以推出ListIterator可以:

(1)双向移动（向前/向后遍历）.

(2)产生相对于迭代器在列表中指向的当前位置的前一个和后一个元素的索引.

(3)可以使用set()方法替换它访问过的最后一个元素.

(4)可以使用add()方法在next()方法返回的元素之前或previous()方法返回的元素之后插入一个元素.

使用例子：

import java.util.*;
public class TestListIterator{

    public static void main(String[] args) {
        ArrayList<String> a = new ArrayList<String>();
        a.add("aaa");
        a.add("bbb");
        a.add("ccc");
       System.out.println("Before iterate : " + a);
       ListIterator<String> it = a.listIterator()
        while (it.hasNext()) {
            System.out.println(it.next() + ", " + it.previousIndex() + ", " + it.nextIndex());
        }
        while (it.hasPrevious()) {
           System.out.print(it.previous() + " ");
        }
        System.out.println();
        it = a.listIterator(1);//调用listIterator(n)方法创建一个一开始就指向列表索引为n的元素处的ListIterator。
        while (it.hasNext()) {
            String t = it.next();
            System.out.println(t);
            if ("ccc".equals(t)) {
                it.set("nnn");
            } else {
                it.add("kkk");
           }
        }
System.out.println("After iterate : " + a);
    }
}



解释：
第1行：新建一个ArrayList，命名为a；

第2行、第3行和第4行分别一次往ArrayList里添加了aaa,bbb,ccc；

第5行：输出ArrayList里的值：aaa,bbb,ccc

第6行：调用了a的listIterator方法，并使ListIterator类型的it指向，也就是说ListIterator类型的it指向了ArrayList容器， 通过调用ArrayList的listIterator方法来进行容器内的遍历。

第7行、8、9行，调用it的hasNext()方法进行判断容器中是否还有元素，如果有，则输出元素，当前元素前一个元素的索引，当前元素后一个元素的索引，

所以会输出：

aaa，0，1

bbb，1，2

ccc，2，3

第10行，此时，it已经指向了ArrayList的最后一个元素，在这里调用了ListIterator的hasPrevious()方法，就是，开始往前遍历(上面是往后遍历) 在这个while循环中，会以此输出:ccc bbb aaa。

第13行：输出换行。

第14行：现在it应该已经再一次指向ArrayList的开头。在这一行中，it又被用到了，同样的用到了ArrayList的listIteror方法，这一次不同，而是it指向了listIteror的第二个元素，因为是1，第一个元素的索引是0，也就是说it指向了ArrayList里的bbb。bbb是开头的元素。

第15行：再一次是调用了ListIterator的hasnext()方法，来判断ArrayList里是否还有元素。

第16行:调用了it的next()方法，所谓next方法，是指找到剩下元素的第一个元素，也就是bbb，并把它赋值了String 的 t；

第17行：输出bbb

第18行：19、20，21行，如果bbb与ccc相等则将bbb set成nnn，否则，add（）来添加kkk，那么在哪里添加呢，是在next方法返回的元素之前，next方法返回的元素是ccc，也就是在bbb，和ccc之间添加kkk。现在容器中有aaa、bbb、kkk以及ccc。返回到第15行，再次以此往下执行，会进行if判断，然后把ccc设置nnn。

（注意：add() 完后，it仍然是原来那个iterator，迭代器的序列依然没有改变，改变的是原list a ）

第24行，最后输出ArrayList里的元素：aaa、bbb、kkk、nnn。

我们来验证一下：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190710195215289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExNDM5Nzc1NTIw,size_16,color_FFFFFF,t_70)

## 3.Iterator和ListIterator区别

我们在使用List，Set的时候，为了实现对其数据的遍历，我们经常使用到了Iterator(迭代器)。使用迭代器，你不需要干涉其遍历的过程，只需要每次取出一个你想要的数据进行处理就可以了。但是在使用的时候也是有不同的。List和Set都有iterator()来取得其迭代器。对List来说，你也可以通过listIterator()取得其迭代器，两种迭代器在有些时候是不能通用的，Iterator和ListIterator主要区别在以下方面：

（1）ListIterator有add()方法，可以向List中添加对象，而Iterator不能
（2）ListIterator和Iterator都有hasNext()和next()方法，可以实现顺序向后遍历，但是ListIterator有hasPrevious()和previous()方法，可以实现逆向（顺序向前）遍历。Iterator就不可以。
（3）ListIterator可以定位当前的索引位置，nextIndex()和previousIndex()可以实现。Iterator没有此功能。
（4）都可实现删除对象，但是ListIterator可以实现对象的修改，set()方法可以实现。Iierator仅能遍历，不能修改。
因为ListIterator的这些功能，可以实现对LinkedList等List数据结构的操作。其实，数组对象也可以用迭代器来实现。