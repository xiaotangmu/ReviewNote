Java关键字是电脑语言里事先定义的，有特别意义的标识符，有时又叫保留字，还有特别意义的变量。Java的关键字对Java的编译器有特殊的意义，他们用来表示一种数据类型，或者表示程序的结构等，关键字不能用作变量名、方法名、类名、包名和参数。

（一）总表：java关键字共53个（其中包含两个保留字const，goto）

abstract
​	

assert
​	

boolean
​	

break
​	

byte

case
​	

catch
​	

char
​	

class
	const
continue 	default 	do 	double 	else
enum 	extends 	final 	finally 	float
for 	goto 	if 	implements 	import
instanceof 	int 	interface 	long 	native
new 	package 	private 	protected 	public
return 	strictfp 	short 	static 	super
switch 	synchronized 	this 	throw 	throws
transient 	try 	void 	volatile 	while
true 	false 	null 	  	 

　另外，Java还有3个保留字:true、false、null。它们不是关键字，而是文字。包含Java定义的值。和关键字一样,它们也不可以作为标识符使用。参考https://baike.baidu.com/item/java%E5%85%B3%E9%94%AE%E5%AD%97/5808816?fr=aladdin#3_43

（二）大致含义
关键字 	含义
abstract 	表明类或者成员方法具有抽象属性
assert 	断言，用来进行程序调试
boolean 	基本数据类型之一，布尔类型
break 	提前跳出一个块
byte 	基本数据类型之一，字节类型
case 	用在switch语句之中，表示其中的一个分支
catch 	用在异常处理中，用来捕捉异常
char 	基本数据类型之一，字符类型
class 	声明一个类
const 	保留关键字，没有具体含义
continue 	回到一个块的开始处
default 	默认，例如，用在switch语句中，表明一个默认的分支
do 	用在do-while循环结构中
double 	基本数据类型之一，双精度浮点数类型
else 	用在条件语句中，表明当条件不成立时的分支
enum 	枚举
extends 	表明一个类型是另一个类型的子类型，这里常见的类型有类和接口
final 	用来说明最终属性，表明一个类不能派生出子类，或者成员方法不能被覆盖，或者成员域的值不能被改变，用来定义常量
finally 	用于处理异常情况，用来声明一个基本肯定会被执行到的语句块
float 	基本数据类型之一，单精度浮点数类型
for 	一种循环结构的引导词
goto 	保留关键字，没有具体含义
if 	条件语句的引导词
implements 	表明一个类实现了给定的接口
import 	表明要访问指定的类或包
instanceof 	用来测试一个对象是否是指定类型的实例对象
int 	基本数据类型之一，整数类型
interface 	接口
long 	基本数据类型之一，长整数类型
native 	用来声明一个方法是由与计算机相关的语言（如C/C++/FORTRAN语言）实现的
new 	用来创建新实例对象
package 	包
private 	一种访问控制方式：私用模式
protected 	一种访问控制方式：保护模式
public 	一种访问控制方式：共用模式
return 	从成员方法中返回数据
short 	基本数据类型之一,短整数类型
static 	表明具有静态属性
strictfp 	用来声明FP_strict（单精度或双精度浮点数）表达式遵循IEEE 754算术规范 [1] 
super 	表明当前对象的父类型的引用或者父类型的构造方法
switch 	分支语句结构的引导词
synchronized 	表明一段代码需要同步执行
this 	指向当前实例对象的引用
throw 	抛出一个异常
throws 	声明在当前定义的成员方法中所有需要抛出的异常
transient 	声明不用序列化的成员域
try 	尝试一个可能抛出异常的程序块
void 	声明当前成员方法没有返回值
volatile 	表明两个或者多个变量必须同步地发生变化
while 	用在循环结构中

（三）详细介绍

1.关键字abstract

abstract关键字可以修饰类或方法。

abstract类可以扩展（增加子类），但不能直接实例化。

abstract方法不在声明它的类中实现，但必须在某个子类中重写。

public abstract class MyClass{}

public abstract String myMethod();

采用abstract方法的类本来就是抽象类，并且必须声明为abstract。

abstract类不能实例化。

仅当abstract类的子类实现其超类的所有abstract方法时，才能实例化abstract类的子类。这种类称为具体类，以区别于abstract类。

如果abstract类的子类没有实现其超类的所有abstract方法，该子类也是abstract类。

abstract关键字不能应用于static、private或final方法，因为这些方法不能被重写，因此，不能在子类中实现。

final类的方法都不能是abstract，因为final类不能有子类。

2.关键字assert（断言）

断言在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。一般来说， 断言用于保证程序最基本、关键的正确性。断言检查通常在开发和测试时开启。为了保证程序 的执行效率，在软件发布后断言检查通常是关闭的。

断言是一个包含布尔表达式的语句，在执 行这个语句时假定该表达式为 true;如果表达式的值为 false，那么系统会报告一个 AssertionError。 断言的使用如下面的代码所示:

assert(a > 0); // throws an AssertionError if a <= 0

断言可以有两种形式:

assert Expression1;
assert Expression1 : Expression2 ;

3.关键字boolean

boolean类型适用于逻辑运算，一般用于程序流程控制。boolean类型数据只允许取值true或false，不可以0或非0的整数代替true和false，这点与C语言不同。

4.关键字break&Continue&case&switch

break&Continue语句

break语句用于终止某个语句块的执行。用在循环语句体中，可以强行推出循环。

    public class BreakTest {
        public static void main(String[] args) {
            int stop = 5;
            for (int i = 0; i < 10; i++) {
                if (i == stop) {
                    break;
                }
                System.out.println("i="+i);
            }
        }
    }
    结果：
    i=0
    i=1
    i=2
    i=3
    i=4


    /**
     * 说明：输出1～100内前5个可以被3整除的数
     *
     * @author huayu
     * @date 2018/8/1 下午4:14
     */
    public class BreakTest {
        public static void main(String[] args) {
           int num=0,i=1;
           while (i<=100){
               if(i%3==0){
                   System.out.println(i+" ");
                   num++;
               }
               if(num==5){
                   break;
               }
               i++;
           }
        }
    }

continue语句用在循环语句体中，用于终止某次循环过程，跳过循环体中continue语句下面未执行的循环，开始下一次循环。

    public class ContinueTest {
        public static void main(String[] args) {
            int skip = 5;
            for (int i = 0; i < 10; i++) {
                if (i == skip) {
                    continue;
                }
                System.out.println("i="+i);
            }
        }
    }
    结果：
    i=0
    i=1
    i=2
    i=3
    i=4
    i=6
    i=7
    i=8
    i=9


    /**
     * 说明：输出101～200内的质数
     *质数又称素数。一个大于1的自然数，除了1和它自身外，不能整除其他自然数的数叫做质数；
     * @author huayu
     * @date 2018/8/1 下午4:21
     */
    public class ContinueTest {
        public static void main(String[] args) {
            //只算奇数就可以，偶数肯定不是质数
            for (int i = 101; i < 200; i+=2) {
                //置个标记，首先默认是质数
                boolean flag=true;
                //如果进入这层for循环flag被置为false，则表示不是质数，那么就跳出内层循环不执行了
                for (int j = 2; j < i; j++) {
                    if(i%j==0){
                        flag=false;
                        break;
                    }
                }
                //筛选，如果不是质数，也就是flag是false就跳过外层这一次循环，继续下一次循环
                if(!flag){
                    continue;
                }
                //如果是质数则能走到这一步，输出出来
                System.out.println(" "+i);
            }
        }
    }

5）switch语句

语句形式：switch(choice){

     case xx:

            ....

        break;

    case xx:

            ....

         break;

    default:

            ....

        break;

}

5.关键词byte，short，int，long

整数类型(java里面所有的数都是带符号的)

java各整数类型有固定的表数范围和字段长度，其不受具体操作系统（windows，linux，mac...）的影响，以保证java程序的可移植性。

java语言整型常量的三种表示形式：

1）十进制整数，如：12，-31，0

2）八进制整数，要求以0开头，如:012(因0开头，010就不确定是什么进制表示的了，为了避免混淆不建议用八进制)

3）十六进制数，要求Ox或OX开头，如：OX12

注意：不管是什么进制在计算机中都是以二进制表示，所以不管是十进制，八进制，十六进制来表示，只要是表示的同一个数它在计算机中的存储都是一样的。

java语言的整型常量默认为int型，声明long型常量可以加l或L（我建议加L好区分）
类型 	占用存储空间 	表数范围
byte 	1字节 	-128～127（-2^7~2^7-1）
short 	2字节 	-2^15~2^15-1
int 	4字节 	-2^31~2^31-1
long 	8字节 	-2^63~2^63-1

6.关键字char

char型数据用来表示通常意义上“字符”。

字符常量为用单引号括起来的单个字符，例如：

char e='a'; 

char e='中'；

java字符采用Unicode编码，每个字符占两个字节（1个字节占8位,数据在计算机底层存储，用0101表示，每一个0，1都叫一个位（bit）），因而可用十六进制编码形式表示。例如：

char c1='\u0061';（在内存中应是01100001，\u指的是后面4位数是十六进制的Unicode编码）

注：Unicode编码是全球语言统一编码。

java语言中还允许使用转义字符‘\’来将其后的字符转变为其它的含义，例如：

char c2='\n';

7.可控制访问权限的四个关键字private，default，protected，public

java权限修饰符public protected private置于类的成员定义前，用来限定其他对象对该类对象成员的访问权限。
修饰符 	类内部 	同一个包 	子类 	任何地方
private 	yes（可以访问） 	  	  	 
default 	yes（可以访问） 	yes（可以访问） 	  	 
protected 	yes（可以访问） 	yes（可以访问） 	yes（可以访问） 	 
public 	yes（可以访问） 	yes（可以访问） 	yes（可以访问） 	yes（可以访问）

注意：1）对于class的权限修饰只可以用public和default。

          2）类的成员(包括内部类)的修饰符可以是以上四种。

          3）类的成员不写访问修饰时默认为 default，默认对于同一个包中的其他类相当于公开 (public)，对于不是同一个包中的其                 他类相当于私有(private)。

          4）受保护(protected)对子类 相当于公开，对不是同一包中的没有父子关系的类相当于私有。

8.关键字float，double

浮点类型：java浮点类型有固定的表数范围和字段长度，不受平台影响。

java浮点型常量有两种表示形式

1.十进制数形式，例如：3.14    41.0   .314

2.科学计数法形式，如3.14e2   3.14E2   100E-2

java浮点型常量默认为double，如果声明一个常量为float，则需在数字后面加f或F，如：

double d=123.4; float f=12.3f;//不加f则出错，f必须加
类型 	占用存储空间 	表数范围
float 	4字节 	-3.403E38～3.403E38（小数点后有效位数7位）
double 	8字节 	-1.798E308～1.798E308（小数点后有效位数15位）

注意：浮点数在计算机中表示是离散的，有误差，在有效位数（精度）之后就不精确了，浮点数值不适用于禁止出现舍入误差的金融计算中。如果需要在数值计算中不含有任何舍入误差，就应该用BigDecimal类。

9.关键字extends

类的继承与权限控制(继承关系：能说通XX是XX就可以看作继承关系，比如，狗是动物)

1）java中使用extends关键字实现类的继承机制，其语法规则为：<modifier>class<name>[extends<superclass>]{... ...}

2）通过继承，子类自动拥有了父类（基类superclass）的所有成员（成员变量和方法）。

3）java只支持单继承，不允许多继承（一个子类只能有一个基类，一个基类可以派生出多个子类）。

释例：public class Student extends Person{}

10.关键字import（引入）：用于引入非同包类，同胞类不需要引入，直接用就可以。

     例：import oop.Birthday;  //引入oop包下的Birthday类

11.关键字package

package (包)例如：package  com.jd;

1）为了便于管理大型软件系统中数目众多的类，解决类的命名冲突问题，java引入包（package）机制，提供类的多重类命名空  间。

2）约定俗称的包起名方式：把公司的域名倒过来。如：package  com.jd;这个包包了两层（建包可不是只能建两层哈，别误会，只是这个例子中有两层），公司域名不是一样的，这样起名避免类名产生重复。

3）package语句作为java源文件的第一条语句，指明该文件中定义的类所在的包。（若缺省该语句，则指定为无名包）。

它的格式为package pkg1[.pkg2[.pkg3....]];

4）java编译器把包对应于文件系统的目录管理，package语句中，用“.“来指明包（目录）的层次，例如package  com.jd;则该文件中所有的类位于.\com\jd 目录下

5）class文件的最上层包的父目录应位于classpath下（在公司不同的项目一般设置不同的classpath）。

6）要想执行一个类必须写全包名。如：java com.jd.TestPackage

12.关键字static

1）在类中，用static声明的成员变量为静态成员变量，它为该类的公用变量。在第一次使用时候被初始化，对于该类的所有对象来说，static成员变量只有一份。

2）用static声明的方法为静态方法，在调用该方法时，不会将对象的引用传递给它，所以在static方法中不可访问非static的成员。（静态方法只能访问静态成员，因为非静态方法的调用要先创建对象，在调用静态方法时可能对象并没有被初始化）。

3）可以通过对象引用或类名（不需要实例化）访问静态成员。

4）static静态变量存在在data seg数据区，就算是不new它，它也会在data seg部分保留一份。静态变量是属于整个类的，它不属于某一个对象。  

知识链接：字符串常量在内存中分配也是在data segment部分。

    /**
     * 说明：静态变量内存分析举例
     * 
     * @author huayu
     * @date 2018/8/3 
     */
    public class Cat {
        //静态成员变量，就算不new对象它也会在data seg里面保存一份，它属于整个类
        //不属于某个对象。int静态变量可以用来计数。
        //对静态值访问：1.任何一个对象通过对象的引用都可以访问这个静态对象，访问的时候都是同一块内存
        //2.即便是没有对象，也可以通过 类名. 来访问 如：System.out  out是个静态变量
    1.    private static  int sid=0;
        //非静态成员变量 new对象的时候在堆内存对象中保存，每new一个对象产生一块
    2.    private  String name;
        //非静态成员变量 new对象的时候在堆内存对象中保存，每new一个对象产生一块
    3.    private int id;
     
        public Cat(String name) {
    4.        this.name = name;
    5.        id=sid++;
        }
     
        public void info(){
    6.        System.out.println("My name is "+name+" No."+id);
        }
     
        public static void main(String[] args) {
            //静态变量sid属于整个Cat类，不属于某个对象，可以用类名.来访问，所以这儿没有new任何对 
            //象，直接用类名.（Cat.sid）来访问的。
    7.        Cat.sid=100;
            //字符串常量分配在data seg
    8.        Cat mimi=new Cat("mimi");
    9.        Cat pipi=new Cat("pipi");
    10.       mimi.info();
    11.       pipi.info();
     
        }
    }
     
    打印结果：
    My name is mimi No.id=100 sid= 102
    My name is pipi No.id=101 sid= 102



执行完7Cat.sid时，静态变量sid值为100。内存分布状态如下：

（1）执行第7句构造方法

第一步：执行第7句Cat mimi=new Cat("mimi");字符串常量“mimi”分布在data segment部分，内存分布如下（这儿看不懂的再回去看以前我的java程序内存分析的博客）：

第二步：调到上面后就该到Cat的构造方法了，执行第4句this.name = name;这时根据传入构造方法的name形参，栈中就会为其开辟一块名为name的空间，实参“mimi”传了进来，这时候栈中name的引用指向了data segment中的字符串常量“mimi”，因为this.name = name，所以自身成员变量的this.name也指向了data segment中的字符串常量“mimi”。

第三步：执行id=sid++；mimi对象的成员变量i值为原来sid的值100，接下来sid++，将sid的值改为101，内存状态如下图：

第四步：构造方法执行完成后，为执行这个方法在栈中分配的形参变量的内存空间收回，name在栈中的空间消失（当然，为执行方法而在栈中分配的局部变量空间，方法执行完毕后都应该被收回了）

（2）执行Cat pipi=new Cat("pipi"); 方法跟执行上面那个构造方法原理是一样的（当然，为执行方法而在栈中分配的局部变量空间，方法执行完毕后都应该被收回了），大家自己画一下，我这边把最后的内存分布状态给一下大家：

从以上sid（static id）的变化不难看出，int的静态变量可以用作计数用。

（3）将以上程序的static静态变量static int sid；再改为非静态变量 int sid；后做内存分析对比

代码：

    /**
     * 说明：将上面代码静态变量改为非静态的再做内存分析
     *
     * @author huayu
     * @date 2018/8/3 下午6:32
     */
    public class Cat1 {
        //成员变量在new的时候才会在堆内分配
        private  int sid=0;
        private  String name;
        private int id;
     
        public Cat1(String name) {
            this.name = name;
            id=sid++;
        }
     
        public void info(){
            System.out.println("My name is "+name+" No.id="+id+" sid= "+sid);
        }
     
        public static void main(String[] args) {
    //        Cat1.sid=100;
            Cat1 mimi=new Cat1("mimi");
            Cat1 pipi=new Cat1("pipi");
            mimi.info();
            pipi.info();
     
        }
    }
    输出结果：
    My name is mimi No.id=0 sid= 1
    My name is pipi No.id=0 sid= 1

对以上程序进行内存分析：这儿上面以前详细的讲过，大家亲自动手去画一画，感受了解一下静态变量跟非静态变量的区别，下面我给贴个最终状态的图（记得，为执行方法而在栈中分配的局部变量空间，最终方法执行完毕后都应该被收回了）：

从以上过程不难看出当静态变量static int sid；改为非静态变量int sid；后，每new一个对象，sid不再发生变化，故用它来计数是不可能了。

13.this关键字

1）在类的方法定义中使用this关键字代表代表使用该方法的对象的引用

2）当必须指出当前使用方法的对象是谁时要用this。

3）有时使用this可以处理方法中成员变量和参数重名的情况。

4）this可以看作是一个变量，它的值是当前对象的引用。

强调：this一般出现在方法里面，当这个方法还没调用的时候，this指的是谁并不知道，但是实际当中你如果new一个对象出来的话，this指的就是当前这个对象。你对那个方法调这个对象，那么this指的就是谁。

    /**
     * 说明：this用法举例
     *
     * @author huayu
     * @date 2018/8/3 下午5:39
     */
    public class Leaf {
        int i=0;
     
        public Leaf(int i) {
    1.        this.i = i;
        }
        public Leaf increament(){
    2.        i++;
    3.        return  this;
        }
        void print(){
    4.        System.out.println("i="+i);
        }
     
        public static void main(String[] args) {
    5.        Leaf leaf=new Leaf(100);
    6.        leaf.increament().increament().print();
        }
    }

上面代码内存分析：

第一步：通过执行main方法第5句，调到了Leaf类的构造方法Leaf(int i),这时栈空间为形参i开辟了一块内存，实参传入的值为100，又将栈内存中形参i的值赋值给了堆内存成员变量i，所以this.i=100。

第二步：构造方法完成，为它分配的局部变量形参i消失。

第三步：执行第6句leaf.increament().increament().print();  leaf.increament()执行leaf对象的increament()方法，执行i++；

第四步：执行第3句return this；return会在栈空间分配一块临时的内存，这块空间是this的内容，而this指向它自身，所以这块临时内存也指向了对象leaf。leaf.increament()执行完成之后内存分布如下图：

第五步：对leaf.increament()结果再调用increament()方法，即还是对原来的对象调用，执行完i++;后，成员变量i值变为102。

第六步：再执行return this;return又会在栈空间分配一块临时的内存，这块空间是this的内容，而this指向它自身，所以这块临时内存也指向了对象leaf。

第7步，调用完print()方法，整个方法执行完成后，栈中局部变量的内存空间回收。

14.关键字super

在java类中使用super来引用基类的成分；

    /**
     * 说明：super关键字引用父类成员演示，基类
     *
     * @author huayu
     * @date 2018/8/5 2:57 PM
     */
    public class Foo {
        public int value;
        public void f(){
            value=100;
            System.out.println("Foo.value="+value);
        }
    }
     
    /**
     * 说明：super关键字引用父类成员演示，子类
     *
     * @author huayu
     * @date 2018/8/5 2:58 PM
     */
    public class Coo extends Foo{
        public int value;
        public void fcoo(){
            //引用父类的f(0)方法
            super.f();
            value=200;
            System.out.println("Coo.value="+value);
            System.out.println(value);
            System.out.println(super.value);
        }
    }
     
    import oop.Coo;
     
    /**
     * 说明：测试方法继承
     *
     * @author huayu
     * @date 2018/8/5 3:05 PM
     */
    public class TestInherit {
        public static void main(String[] args) {
            Coo coo = new Coo();
            coo.fcoo();
        }
    }


    测试结果：
    Foo.value=100
    Coo.value=200
    200
    100

内存分析图：

把这个子类继承父类程序并子类调用父类成员的内存分配过程，这儿常出面试题：子类是怎么对父类的成员进行调用的。因为以前教过内存分析的具体过程，这儿我就只给了个最后的内存分布图，大家自己好好分析一遍。（当然，方法调用完，栈内为其分配的空间应被收回）。

15.关键字：while

while语句形式：while(逻辑表达式){语句;....;}

执行过程：先判断逻辑表达式的值，若为true则执行其后面的语句，然后再次判断条件并反复执行，直到条件不成立为止


原文链接：https://blog.csdn.net/Myuhua/article/details/81411707





作业一：Java基础知识复习
一、关键字总结
1.abstract

    修饰类：

abstract修饰类，这个类就是抽象类，抽象类中可以有非抽象变量和成员变量，也可以有普通方法、构造方法。但是不能实例化，只能被子类继承。
如果子类不是抽象类，则必须重写父类的抽象方法。

public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {
    ...
}

    修饰方法:

abstract修饰方法，这个方法就是抽象方法。抽象方法必须存在于抽象类中。抽象方法不能有具体实现。

abstract public E get(int index);




2.assert

assert表示“断言”，有两种使用方法：

assert 表达式;




若表达式为真，程序继续执行；若表达式为假，则抛出一个AssertionError异常。

assert 表达式:错误信息;




与第一种方法相同，只是异常中带有错误信息。
使用assert时不能在表达式中完成任何程序实际所需的行为（只能做判断）。因为正常发布的代码都是断言无效的，即正常发布的代码中断言语句都不不执行的。
3.boolean

boolean是Java的基本类型之一（默认值false）。只有两个值：true和false。区别C的判断句，Java不能直接使用1和0来表示真假，且boolean类型也不能强转到其他基本类型。

boolean a = true;
boolean b = false;

4.break

    break在switch中用于跳出switch块，停止switch向下穿透的现象。

case value:expression;
    break;


break在循环中用于跳出循环。

while(...){
    ...
    break;
}



break也可以在后面接标签，用来跳出一些嵌套比较复杂的循环中。

flag:
for(...){
    for(...){
        break flag;
    }
}

5.byte

byte是Java的基本类型之一（默认值0）。表示8位有符号整数。

范围：-128~127

byte a = 100;




6.case

case用于switch中，用于判断和执行语句。用法：

case 变量值:语句;




若变量值和switch(变量值)
中的变量值相等，就执行后面的语句。执行完后继续执行下一个case语句。
7.catch

catch用于捕获异常。
用法：

catch(异常类型 异常){...}




在try/catch语句块中，catch捕获发生的异常，并应对错误做一些处理。
当catch捕获到异常后，try中执行的语句终止，并跳到catch后的语句中。
8.char

char是Java的基本类型之一（默认值\u000）。表示16位、在Unicode编码表中的字符。使用单引号来表示字符常量，例如’A’。

范围：0-65535

char a = 'A';


9.class

class表示类。用于声明一个类。

[访问控制] (abstract) class 类名 (implements){...}




10.const

const是Java的一个保留关键字，没有实际意义，但是不能用于做变量名（因为被保留作为关键字了）。在C语言中表示常量，类似Java的final。
11.continue

    continue用于在循环中跳过本次循环。

while(...){
    ...
    continue;
}



continue也可以在后面接标签，在一些嵌套比较复杂的循环中跳过一次循环。

flag:
for(...){
    for(...){
        continue flag;
    }
}



12.default

default关键字：

    用于switch做默认分支：

default:语句；




    用于接口,让接口实现具体的方法：

public interface a{
    default void b(){
        具体方法;
    }
}



default用于接口时，必须要有具体实现。
(API>=24)
13. do

do用于和while组成循环，do/while循环不同于while循环，属于先执行循环体再判断。

do{
	循环体;
}while(...)

14.double

double是Java的基本类型之一（默认值0.0d），表示双精度、64位的浮点数。

double a = 0.1d;




15.else

else用于分支结构中的判断。例如：

if(判断1){
    语句1;
}else if(判断2){
    语句2;
}else{
    语句3;
}



16.enum

enum表示枚举，用于限制变量值的类型，例如：

public enum Alpha (implements 接口){
    (public static final)a,b,c,d
}



规定Color的实例只能为a,b,c,d其中之一。

枚举类中可以有成员变量和方法。
17.extends

extends表示继承。例如：

class 子类 extends父类{}




Java中的继承是单继承，即子类只能有一个直接父类。
除了private，子类可以访问父类的方法和成员变量。
18.final

    修饰变量：
    将变量变为常量，在初始化变量后不能再改变值。
    修饰方法：
    被final修饰的方法不能被子类重写。
    修饰类：
    被final修饰的类不能被继承。

19.finally

finally在try/catch语句块中处理一些后续的工作。例如关闭网络连接和输入输出流等。

    如果在try/catch中使用return，则finally会撤销这个return，无论如何都会执行finally中的语句。

20.float

float是Java的基本类型之一（默认值0.0f）。表示单精度、32位的浮点数。

float a = 0.1f;




21.for

for用于循环:

for(初始化循环变量; 判断执行条件;更新循环变量){
    语句
}
for(变量:数组){
    语句
}



22.goto

Java中的保留关键字，没有实际意义，但是不能用做变量名。在C中表示无条件跳转语句。
23.if

if用于分支结构中的判断。常与else和else if使用。

if(表达式){语句}




若表达式为真，则执行后面的语句。
24.implements

implements用于接入接口。接上接口的类必须实现接口的抽象方法（可以不实现默认方法和静态方法）。

class A implements B{
    @Override
    do(){
        ...
    }
}



25.import

用于导入包。

import android.content.Intent;




26.instanceof

instanceof用于判断类与对象的关系。例如：

a instanceof b




若a是b的一个实例(或子类对象)，则整个表达式的结果是true，否则结果为false。
27.int

int是Java的基本类型之一（默认值为0）。表示32位、有符号的整数。

范围：[-231,231-1)

int a = 1;




28.interface

interface用于声明一个接口，例如：

public interface A{
    void b();
}



声明a为一个接口，若接上该接口，则必须实现其中的抽象方法b。
接口中的成员变量是static、final、public的。接口中的方法为静态方法或默认方法和静态方法(API>=24)。
29.long

long是Java的基本类型之一（默认值为0L），表示64位、有符号的整数。

范围：[-263,263)

long a = 3216846849646L;




30.native

native可以让Java运行非Java实现的方法。例如c语言，要编译后用javah产生一个.h文件。导入该.h文件并且实现native方法，编译成动态链接库文件。在Java加载动态链接库文件，这个native方法就可以在Java中使用了。

public native void aVoid();




31.new

new用于生成类的实例。

Object a = new Object()；




32.package

package用于规定当前文件的包。

package com.example.zhangyijun.testdefactivity;




33.private

访问控制的一种。
私有的方法和变量只能在本类中访问。类和接口不能为私有。

private int a = 1;
private void b(){
    ...
}



34.protected

访问控制的一种。
受保护的方法和变量只能给子类和基类访问。

protected int a = 1;
protected void b(){
    ...
}



35.public

访问控制的一种。
公有的方法、类、变量、接口能够被任何其他类访问。
36.return

方法中返回数据，并结束方法。
37.strictfp

使用strictfp关键字来声明一个类、接口或者方法时，那么该类、接口或者方法会遵循IEEE-754标准来执行，提高浮点运算的精度，并且减少不同硬件平台之间由于浮点运算带来的差异。

public strictfp double aDouble(){
    return 0d;
}



38.short

short是Java的基本类型之一（默认值0）,表示16位、有符号的整数。

范围：[-215,215)

short a = 0;




39.static

static修饰的语句块存放在堆的方法区中。

    静态变量：依附在类中的变量，可以被类的所有的实例共用。

static int a = 0;




    静态方法：依附在类中的方法。静态方法只能访问类中的静态变量和静态方法。

publlic static void b(){
    ...
}



静态块：在类加载的时候执行块中的语句，块中不能访问非静态变量。

static{
    ...
}

    静态内部类：用static修饰内部类。

40.super

super即超类

    引用父类的的成员：

super.xxx




    变量或方法重名时用super调用父类的成员或方法。
    调用父类的构造方法:

super(xxx);




41.switch

switch用于分支结构，判断某个变量与一系列值是否相等。switch 语句中的变量类型可以是： byte、short、int 、char、String、enum。

switch(变量){
	case value1:语句1;
		break;
	case value2:语句2;
		break;
	...
	default:语句;
}



若变量和case后的值相等则执行语句。
当语句执行到break时跳到switch块后，如果没有break会产生穿透现象。
default分支必须为最后一个分支，在没有值和case变量相等时执行该语句。

42.synchronized

synchronized关键字用于保证线程安全。由这个关键字修饰的方法或者代码块保证了同一时刻只有一个线程执行该代码。

synchronized(obj){...}




当一个线程访问同步代码块时，检查obj是否有锁，如果有就挂起。如果没有就获得这个obj的锁，也就是把其他线程锁在了外面。当代码执行完毕时释放该锁，其他线程获得锁继续执行代码。
43.this

    指向当前对象：this.xxx
    形参和成员名字重名时时用this区分。
    引用构造函数。

44.throw

用于抛出一个异常。

throw (Exception);




45.throws

在方法中将发生的异常抛出。

[控制访问](返回类型)(方法名)([参数列表])[throws(异常类)]{...}




46.transient

类接上序列化接口后，可以通过transient关键字将某些变量变得无法序列化。

transient int a = 1;


47.try

在try/catch中，将可能出现异常的语句放在try{}块中，出现异常之后代码将会终止并跳到catch中继续执行。

try{
    ...
}catch(Exception e){
    ...
}finally{
    ...
}



48.void

修饰方法，表示方法没有返回值。
49.volatile

volatile关键字修饰的变量在多线程中保持同步。相比synchronized效率要高，不会阻塞线程。但只能保证数据的可见性，不能保证数据的原子性。例如在处理i++的时候另外一个线程修改i的值，那么i的值将会发生错误，这是原子性导致的。

volatile int a;




50.while

while用于两种循环结构：

while(判读语句){
    循环体...
}

do{
	循环体...
}while(判读语句)



二、查漏补缺
1.多态

多态就是同一个行为，使用不同的实例而发生不同的作用。在使用多态调用方法的时候，编译器检查父类中是否有该方法，如果有才能编译通过，例如：

public class Animals{
    void voice(){动物叫}
}

class Cat extends Animals{
    void voice(){猫叫}
}

public static void testVoice(Animals a){
    a.voice();
}

public static void main(String[] args) {
    testVoice(new Cat())；

    Animals a = new Cat();
    a.voice();
}



猫继承自动物这个类，Animals a = new Cat()是向上转型（父类引用指向子类对象），实际的运行时类型还是Cat，也就是说a instanceof Cat 表达式为真，因此调用a的voice()方法是猫叫。结合C的指针和内存分析来理解多态。
2.泛型

    类型通配符

    <? extends T>表示该通配符所代表的类型是T类型的子类。
    <? super T>表示该通配符所代表的类型是T类型的父类

public static <T extends Closable> void close(T... a){
    for(T temp:a){
        try{
            if(temp!=null){
                temp.close();
            }
        }catch(IOException e){
            e.printStackTrace();
        }
    }
}

    泛型不能用在静态属性上
    指定的类型不能为基本类型

3.反射

    获取目标类型的Class对象

Class<?> a = Object.getClass();
Class<?> b = T.class;
Class<?> c = Class.forName(...);



通过 Class 对象分别获取Constructor类对象、Method类对象 & Field 类对象
​    不带 "Declared"的方法支持取出包括继承、公有（Public） & 不包括有（Private）的构造函数
​    带 "Declared"的方法是支持取出包括公共（Public）、保护（Protected）、默认（包）访问和私有（Private）的构造方法，但不包括继承的构造函数

Constructor

//a.获取指定的构造函数（公共/继承）
Constructor<T> getConstructor(Class<?>... parameterTypes);
//b.获取所有的构造函数（公共/继承） 
Constructor<?>[] getConstructors(); 
//c.获取指定的构造函数（不包括继承）
Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes);
//d.获取所有的构造函（不包括继承）
Constructor<?>[] getDeclaredConstructors(); 



Field

//a.获取指定的属性（公共/继承）
Field getField(String name);
//b.获取所有的属性（公共/继承）
Field[] getFields();
//c.获取指定的所有属性（不包括继承）
Field getDeclaredField(String name);
//d.获取所有的所有属性（不包括继承）
Field[] getDeclaredFields();


Method

//a.获取指定的方法（公共/继承）
Method getMethod(String name, Class<?>... parameterTypes);
//b.获取所有的方法（公共/继承）
Method[] getMethods();
//c.获取指定的方法 （不包括继承）
Method getDeclaredMethod(String name, Class<?>... parameterTypes);
//d.获取所有的方法（不包括继承）
Method[] getDeclaredMethods();



4. 集合

    迭代器遍历ArrayList

Iterator<String> iterator = list.iterator();
while(ite.hasNext()){
    Log.d("TAG",ite.next());
}



遍历Map

//第一种：map.keySet()
for (String key : map.keySet()) {
    System.out.println("key= "+ key + " value= " + map.get(key));
}

//第二种：map.entrySet().iterator()
Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<String, String> entry = it.next();
    System.out.println("key= " + entry.getKey() + " value= " + entry.getValue());
}
​      
//第三种：map.entrySet()
for (Map.Entry<String, String> entry : map.entrySet()){
    System.out.println("key= " + entry.getKey() + " value= " + entry.getValue());
}
​    
//第四种：map.values()
for (String v : map.values()) {
    System.out.println("value= " + v);
}

    
    遍历 hashMap() 时 entrySet() 方法是将 key 和 value 全部取出来,所以性能开销是可以预计的, 而 keySet() 方法进行遍历的时候是根据取出的 key 值去查询对应的 value 值, 所以如果 key 值是比较简单的结构(如 1,2,3…)的话性能消耗上是比 entrySet() 方法低, 但随着 key 值得复杂度提高 entrySet() 的优势就会显露出来。
    在只遍历 key 的时候使用 keySet(), 在只遍历 value 的时候使用 values(), 在遍历 key-value 的时候使用 entrySet()。
    

5.正则

RegexBuddy

    转义字符

\n  \t  \\  \^  \$  \(  \)  \{  
\}  \?  \+  \*  \|  \[  \]



标准字符集合（大写取反）

\d 	0~9的任意一个数字
\w 	A~Z, a~z, 0~9, _中任意一个
\s 	空格、制表符、换行符等空白符的任意一个
. 	匹配任意一个字符
[] 	匹配方括号中任意一个字符
^ 	方括号取反
-   方括号中表示范围
  {} 花括号前正则表达式的重复次数，{m,n}至少m次，最多n次
  ? 花括号后加，非贪婪模式。非花括号后加，相当于{0,1}
  + 前面的正则表达式至少出现一次，相当于{1,}
  * 表达式不出现或出现多次，相当于{0,}
    ^ 与字符串开始的地方匹配
    $ 与字符串结束的地方匹配
    \b 匹配一个字符边界
    | 匹配左边或者右边
    (?=exp) 断言自身出现的位置的后面能匹配表达式exp
    (?<=exp) 断言自身出现的位置的前面能匹配表达式exp
    (?!exp) 断言此位置的后面不能匹配表达式exp
    (?<!exp) 断言此位置的前面不能匹配表达式exp
    6.引用分类

    强引用:StrongReference:引用指向对象，gc运行时不回收
    软引用:SoftReference:gc运行时回收，（jvm内存不够）
    弱引用:WeakReference:gc运行时立即回收
    虚引用:PhantomReference:跟踪对象被回收的状态，必须与ReferenceQueue一起使用
    ————————————————
    版权声明：本文为CSDN博主「Lazyjam」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
    原文链接：https://blog.csdn.net/u013998373/article/details/82714551