https://www.cnblogs.com/fysola/p/6021134.html

# [Collections工具类    ](https://www.cnblogs.com/fysola/p/6021134.html)            

Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。

（下面操作不单单针对List）

## 排序操作

Collections提供以下方法对List进行排序操作

void reverse(List list)：反转

void shuffle(List list),随机排序

void sort(List list),按自然排序的升序排序

void sort(List list, Comparator c);定制排序，由Comparator控制排序逻辑

void swap(List list, int i , int j),交换两个索引位置的元素

void rotate(List list, int distance),旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面。

下面简单演示Collections操作List

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package collection;
 2 
 3 import java.util.ArrayList;
 4 import java.util.Collections;
 5 import java.util.Comparator;
 6 
 7 public class CollectionsTest {
 8     public static void main(String[] args) {
 9         ArrayList nums =  new ArrayList();
10         nums.add(8);
11         nums.add(-3);
12         nums.add(2);
13         nums.add(9);
14         nums.add(-2);
15         System.out.println(nums);
16         Collections.reverse(nums);
17         System.out.println(nums);
18         Collections.sort(nums);
19         System.out.println(nums);
20         Collections.shuffle(nums);
21         System.out.println(nums);
22         //下面只是为了演示定制排序的用法，将int类型转成string进行比较
23         Collections.sort(nums, new Comparator() {
24             
25             @Override
26             public int compare(Object o1, Object o2) {
27                 // TODO Auto-generated method stub
28                 String s1 = String.valueOf(o1);
29                 String s2 = String.valueOf(o2);
30                 return s1.compareTo(s2);
31             }
32             
33         });
34         System.out.println(nums);
35     }
36 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

执行结果，

```
1 [8, -3, 2, 9, -2]
2 [-2, 9, 2, -3, 8]
3 [-3, -2, 2, 8, 9]
4 [9, -2, 8, 2, -3]
5 [-2, -3, 2, 8, 9]
```

## 查找，替换操作

int binarySearch(List list, Object key), 对List进行二分查找，返回索引，注意List必须是有序的

int max(Collection coll),根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)

int max(Collection coll, Comparator c)，根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)

void fill(List list, Object obj),用元素obj填充list中所有元素

int frequency(Collection c, Object o)，统计元素出现次数

int indexOfSubList(List list, List target), 统计targe在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target).

boolean replaceAll(List list, Object oldVal, Object newVal), 用新元素替换旧元素。

下面示范简单用法，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package collection.collections;
 2 
 3 import java.util.ArrayList;
 4 import java.util.Collections;
 5 
 6 public class CollectionsTest {
 7     public static void main(String[] args) {
 8         ArrayList num =  new ArrayList();
 9         num.add(3);
10         num.add(-1);
11         num.add(-5);
12         num.add(10);
13         System.out.println(num);
14         System.out.println(Collections.max(num));
15         System.out.println(Collections.min(num));
16         Collections.replaceAll(num, -1, -7);
17         System.out.println(Collections.frequency(num, 3));
18         Collections.sort(num);
19         System.out.println(Collections.binarySearch(num, -5));
20     }
21 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

执行结果，

```
1 [3, -1, -5, 10]
2 10
3 -5
4 1
5 1
```

 

## 同步控制

Collections中几乎对每个集合都定义了同步控制方法，例如 SynchronizedList(), SynchronizedSet()等方法，来将集合包装成线程安全的集合。下面是Collections将普通集合包装成线程安全集合的用法，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package collection.collections;
 2 
 3 import java.util.ArrayList;
 4 import java.util.Collection;
 5 import java.util.Collections;
 6 import java.util.HashMap;
 7 import java.util.HashSet;
 8 import java.util.List;
 9 import java.util.Map;
10 import java.util.Set;
11 
12 public class SynchronizedTest {
13     public static void main(String[] args) {
14         Collection c = Collections.synchronizedCollection(new ArrayList());
15         List list = Collections.synchronizedList(new ArrayList());
16         Set s = Collections.synchronizedSet(new HashSet());
17         Map m = Collections.synchronizedMap(new HashMap());
18     }
19 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

设置不可变（只读）集合

Collections提供了三类方法返回一个不可变集合，

emptyXXX(),返回一个空的只读集合（这不知用意何在？）

singleXXX()，返回一个只包含指定对象，只有一个元素，只读的集合。

unmodifiablleXXX()，返回指定集合对象的只读视图。

用法如下，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package collection.collections;
 2 
 3 import java.util.Collection;
 4 import java.util.Collections;
 5 import java.util.HashMap;
 6 import java.util.List;
 7 import java.util.Map;
 8 import java.util.Set;
 9 
10 public class UnmodifiableCollection {
11     public static void main(String[] args) {
12         List lt = Collections.emptyList();
13         Set st = Collections.singleton("avs");
14         
15         Map mp = new HashMap();
16         mp.put("a",100);
17         mp.put("b", 200);
18         mp.put("c",150);
19         Map readOnlyMap = Collections.unmodifiableMap(mp);
20         
21         //下面会报错
22         lt.add(100);
23         st.add("sdf");
24         mp.put("d", 300);
25     }
26 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

 执行结果，

```
1 Exception in thread "main" java.lang.UnsupportedOperationException
2     at java.util.AbstractList.add(Unknown Source)
3     at java.util.AbstractList.add(Unknown Source)
4     at collection.collections.UnmodifiableCollection.main(UnmodifiableCollection.java:22)
```



https://www.cnblogs.com/wei-jing/p/10540192.html

# Arrays类

Arrays类位于 java.util 包中，主要包含了操纵数组的各种方法

使用时导包:import java.util.Arrays

二、Arrays常用函数（都是静态的)

1.void Arrays.sort()

void Array.sort(Object[] array)

功能：对数组按照升序排序

示例

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        int[] nums = {2,5,0,4,6,-10};
        Arrays.sort(nums);
        for(int i :nums)
            System.out.print(i+" ");
        /* 之前:2 5 0 4 6 -10 
         * 结果:-10 0 2 4 5 6 
         */
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

Arrays.sort(Object[] array, int from, int to)

功能：对数组元素指定范围进行排序（排序范围是从元素下标为from,到下标为to-1的元素进行排序）

示例

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
int[] nums = {2,5,0,4,1,-10};
        //对前四位元素进行排序
        Arrays.sort(nums, 0, 4);
        for(int i :nums)
            System.out.print(i+" ");
        /* 之前:2 5 0 4 1 -10
         * 结果:0 2 4 5 1 -10 
         */
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 2.Arrays.fill(Object[] array,Object object)

功能：可以为数组元素填充相同的值

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
int[] nums = {2,5,0,4,1,-10};
        Arrays.fill(nums, 1);
        for(int i :nums)
            System.out.print(i+" ");
        /* 之前:2 5 0 4 1 -10
         * 结果:1 1 1 1 1 1 
         */
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

Arrays.fill(Object[] array,int from,int to,Object object)

功能：对数组的部分元素填充一个值,从起始位置到结束位置，取头不取尾

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
int[] nums = {2,5,0,4,1,-10};
        //对数组元素下标2到4的元素赋值为3
        Arrays.fill(nums,2,5,3);
        for(int i :nums)
            System.out.print(i+" ");
        /* 之前:2 5 0 4 1 -10
         * 结果:2 5 3 3 3 -10 
         */
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

3.Arrays.toString(Object[] array)

功能：返回数组的字符串形式

示例

```
        int[] nums = {2,5,0,4,1,-10};
        System.out.println(Arrays.toString(nums));
        /*
         * 结果:[2, 5, 0, 4, 1, -10]
         */
```

4.Arrays.deepToString(Object[][] arrays)

功能：返回多维数组的字符串形式

示例

```
int[][] nums = {{1,2},{3,4}};
        System.out.println(Arrays.deepToString(nums));
        /*
         * 结果:[[1, 2], [3, 4]]
         */
```



来源：http://liuguoquan727.github.io/2015/12/18/Java%E9%9B%86%E5%90%88%E4%B9%8BArrays%E5%92%8CCollections/

#  Java集合之Arrays和Collections                                                                                                       

## Arrays

java.util.Arrays

[Arrays详细介绍](http://www.apihome.cn/api/java/Arrays.html)

Array是Java特有的数组。在你知道所要处理数据元素个数的情况下非常好用。java.util.Arrays 包含了许多处理数据的实用方法：

Arrays.asList：可以从 Array 转换成 List。可以作为其他集合类型构造器的参数。

Arrays.binarySearch：在一个已排序的或者其中一段中快速查找。

Arrays.copyOf：如果你想扩大数组容量又不想改变它的内容的时候可以使用这个方法。

Arrays.copyOfRange：可以复制整个数组或其中的一部分。

Arrays.deepEquals、Arrays.deepHashCode：Arrays.equals/hashCode的高级版本，支持子数组的操作。

Arrays.equals：如果你想要比较两个数组是否相等，应该调用这个方法而不是数组对象中的 equals方法（数组对象中没有重写equals()方法，所以这个方法之比较引用而不比较内容）。这个方法集合了Java 5的自动装箱和无参变量的特性，来实现将一个变量快速地传给 equals() 方法——所以这个方法在比较了对象的类型之后是直接传值进去比较的。

Arrays.fill：用一个给定的值填充整个数组或其中的一部分。

Arrays.hashCode：用来根据数组的内容计算其哈希值（数组对象的hashCode()不可用）。这个方法集合了Java 5的自动装箱和无参变量的特性，来实现将一个变量快速地传给 Arrays.hashcode方法——只是传值进去，不是对象。

Arrays.sort：对整个数组或者数组的一部分进行排序。也可以使用此方法用给定的比较器对对象数组进行排序。

Arrays.toString：打印数组的内容。

如果想要复制整个数组或其中一部分到另一个数组，可以调用 System.arraycopy方法。此方法从源数组中指定的位置复制指定个数的元素到目标数组里。这无疑是一个简便的方法。（有时候用 ByteBuffer bulk复制会更快。可以参考这篇文章）.

最后，所有的集合都可以用T[] Collection.toArray( T[] a ) 这个方法复制到数组中。通常会用这样的方式调用：

```

```

```
return coll.toArray( new T[ coll.size() ] );
```

这个方法会分配足够大的数组来储存所有的集合，这样 toArray 在返回值时就不必再分配空间了。

## Collections

java.util.Collections

[Collections详细介绍](http://www.apihome.cn/api/java/Collections.html)

就像有专门的java.util.Arrays来处理数组，Java中对集合也有java.util.Collections来处理。

第一组方法主要返回集合的各种数据：

Collections.checkedCollection / checkedList / checkedMap / checkedSet / checkedSortedMap / checkedSortedSet：检查要添加的元素的类型并返回结果。任何尝试添加非法类型的变量都会抛出一个ClassCastException异常。这个功能可以防止在运行的时候出错。//fixme

Collections.emptyList / emptyMap / emptySet ：返回一个固定的空集合，不能添加任何元素。

Collections.singleton / singletonList / singletonMap：返回一个只有一个入口的 set/list/map 集合。

Collections.synchronizedCollection / synchronizedList / synchronizedMap / synchronizedSet / synchronizedSortedMap / synchronizedSortedSet：获得集合的线程安全版本（多线程操作时开销低但不高效，而且不支持类似put或update这样的复合操作）

Collections.unmodifiableCollection / unmodifiableList / unmodifiableMap / unmodifiableSet / unmodifiableSortedMap / unmodifiableSortedSet：返回一个不可变的集合。当一个不可变对象中包含集合的时候，可以使用此方法。

第二组方法中，其中有一些方法因为某些原因没有加入到集合中：

Collections.addAll：添加一些元素或者一个数组的内容到集合中。

Collections.binarySearch：和数组的Arrays.binarySearch功能相同。

Collections.disjoint：检查两个集合是不是没有相同的元素。

Collections.fill：用一个指定的值代替集合中的所有元素。

Collections.frequency：集合中有多少元素是和给定元素相同的。

Collections.indexOfSubList / lastIndexOfSubList：和String.indexOf(String) / lastIndexOf(String)方法类似——找出给定的List中第一个出现或者最后一个出现的子表。

Collections.max / min：找出基于自然顺序或者比较器排序的集合中，最大的或者最小的元素。

Collections.replaceAll：将集合中的某一元素替换成另一个元素。

Collections.reverse：颠倒排列元素在集合中的顺序。如果你要在排序之后使用这个方法的话，在列表排序时，最好使用Collections.reverseOrder比较器。

Collections.rotate：根据给定的距离旋转元素。

Collections.shuffle：随机排放List集合中的节点，可以给定你自己的生成器——例如java.util.Random / java.util.ThreadLocalRandom or java.security.SecureRandom。

Collections.sort：将集合按照自然顺序或者给定的顺序排序。

Collections.swap：交换集合中两个元素的位置（多数开发者都是自己实现这个操作的）。