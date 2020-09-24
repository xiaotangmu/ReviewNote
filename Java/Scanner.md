大部分公司的笔试题跟 LeetCode 的审核机制不一样，测试数据的给定并不是由函数参数提供，而是需要自己使用 Scanner 去扫描，有的公司还好，给你提供了 Scanner 的使用示例，有的公司直接就给你白板，要是不熟悉 Scanner 的话，在这上要浪费好多不必要的时间，所以写篇文章特意梳理一下 Scanner 的用法。


Scanner 介绍


直接来看 Java API 文档。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619091329134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)
简单的翻译一下，它就是一个简单的文本扫描器，可以使用正则表达式解析原始类型和字符串。 可以自己指定分割的方式，将输入切分，然后可以调用不同的 next 方法，获取你想得到的类型的值。


Scanner 构造函数


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619115835663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)
大家关注我圈红线的这个构造函数就行，它是笔试题中用到的，一般将键盘输入作为输入流传入即可，如下所示

Scanner sc = new Scanner(System.in);


之后便可以读取控制台的输入了


常用的 Scanner 函数


笔试中常用的 Scanner 函数无非就以下这几种：

    next()
    nextLine()
    nextInt()
    hasNext()

依次来看一下

一、next()

首先是 next() 方法，它还有两个重载的方法，JDK 给我们提供了这两个自己定义分割方式的 next() 方法。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619144437761.png)
我们只关注第一个默认的 next() 方法即可，它从遇到的第一个有效字符（非空格、非换行）开始扫描，直到遇到空格或者换行符。将这段内容以 String 返回。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619145106128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)
另外，注意到我画红线的这段，如果以控制台输入作为输入流传入 Scanner 的构造函数，那么这个方法会在有输入之前阻塞。一会儿在介绍 hasNext() 会着重提及阻塞特性。

此处的代码示例在介绍下个 nextLine() 方法时一并给出，以便将这两个方法做一个区分。

二、nextLine()

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619145412617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)

没有重载版本，跟 next() 方法不同的地方在于，它是以换行符作为分割点，这点要着重记下来。

下面是一个非常简单的代码示例，输入都是 A B C D

首先看 next()

public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("请输入：");
		//之后我们输入  A B C D
		int matchNum = 1;
		while(scanner.hasNext()) {
			System.out.print("扫描到第" + matchNum + "段匹配内容：");
			System.out.println(scanner.next());
			matchNum++;
		}
	}

控制台输出结果

请输入：A B C D
扫描到第1段匹配内容：A
扫描到第2段匹配内容：B
扫描到第3段匹配内容：C
扫描到第4段匹配内容：D



注意到，next() 以空格符（换行符也行，这里不多写了）作为分割。

接下来我们看一看 nextLine()

public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("请输入：");
		//之后我们输入  A B C D
		int matchNum = 1;
		while(scanner.hasNextLine()) {
			System.out.print("扫描到第" + matchNum + "段匹配内容：");
			System.out.println(scanner.nextLine());
			matchNum++;
		}
	}

控制台输出结果

请输入：A B C D
扫描到第1段匹配内容：A B C D



注意到，nextLine() 以换行符作为分割。

三、nextInt()![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619151149751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)
跟 next() 方法很像，都是以空格或者换行符作为扫描终点。代码如下

诸如 nextLong()、nextFloat() 等这一系列的相信不用我说你也应该知道跟 nextInt() 同理。

public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("请输入：");
		//之后我们输入  1 2 3 4
		int matchNum = 1;
		while(scanner.hasNextInt()) {
			System.out.print("扫描到第" + matchNum + "段匹配内容：");
			System.out.println(scanner.nextInt());
			matchNum++;
		}
	}

控制台

请输入： 1 2 3 4
扫描到第1段匹配内容：1
扫描到第2段匹配内容：2
扫描到第3段匹配内容：3
扫描到第4段匹配内容：4



四、hasNext()![在这里插入图片描述](https://img-blog.csdnimg.cn/20190619152516149.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NjgzNzM=,size_16,color_FFFFFF,t_70)
hasNext() 方法判断输入流里是否还有内容，有的话就返回 true。

但是要注意我这里两个圈红线的地方。

第一处，我在介绍第一个 next() 方法时候提及过，阻塞这个特性，主要就是为了 hasNext() 这个方法进行铺垫。

第二处，骚的不行，就告诉你啥时候会返回 true, 但不告诉你何时返回 false！

针对此，我们来看下面的代码。

public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("请输入：");
		int matchNum = 1;
		while(scanner.hasNext()) {
			System.out.print("扫描到第" + matchNum +"段匹配的内容：");
			System.out.println(scanner.next());
			matchNum++;
		}
		System.out.println("能到这儿吗？");
		//do something
	}

控制台结果

请输入：hasNext() will block your code!
扫描到第1段匹配的内容：hasNext()
扫描到第2段匹配的内容：will
扫描到第3段匹配的内容：block
扫描到第4段匹配的内容：your
扫描到第5段匹配的内容：code!



睁大眼睛瞧一瞧，循环体外的打印没有执行，这就是阻塞的特性，如果你在实例化 Scanner 的时候构造函数使用的是 System.in 作为输入流传入，那么就算读完了 **hasNext() 也会一直阻塞你的代码**。

阻塞特性这点我为什么要着重提及呢，因为你在你的笔试答题的过程当中（前提是这场笔试你能使用本地的 IDE debug你的代码），你的代码可能会使用 hasNext() 作为循环条件，而你还有其他部分的代码需要在 While 循环体外执行（当然如果你的代码逻辑全都在你的循环体内处理了，那就当我没说），这时你在笔试站点执行调试（比如牛客，一般提供两个简单的测试用例）的时候是正常的，但是你没有通过所有的测试用例，这时你把代码挪到自己的 IDE 上慢慢调试的时候发现代码跑的不正常了，别慌，看看你的构造函数你就明白了，因为你使用的是 System.in 作为 InputStream 构造的 Scanner 对象。
解决办法：

1. 循环条件改一下，比如加点别的判断条件，保证能跳出循环。

2. 在本地调试的时候，构造 Scanner 对象别使用 System.in，自己将测试数据写在一个文件里，然后利用 Scanner(File source) 这个构造函数实例化你的 Scanner 对象。这个比较麻烦，当然还是推荐第一种。

   ```
   @Test
   	public void testScanner2() {
   		Scanner sc = new Scanner(System.in);
   		System.out.println("请输入要读取的信息：");
   		int size = 3;
   		while(size > 0) {
   			if(sc.hasNext()) {
   				System.out.println("打印：" + sc.next());
   			}
   			size--;
   		}
   		System.out.println("hello");//输出成功
   	}
   ```

   ​

使用 File 对象不会阻塞，示例如下。

我先创建了一个测试文件在我的桌面上。
在这里插入图片描述

内容如下

在这里插入图片描述
测试一下，会不会阻塞

public static void main(String[] args) throws FileNotFoundException {
		System.out.println("使用文件 test.txt 作为输入流实例化 Scanner");
		System.out.println();
		Scanner scanner = new Scanner(new File("C:\\Users\\xudaxia0610\\Desktop\\test.txt"));
		int matchNum = 1;
		while(scanner.hasNext()) {
			System.out.print("扫描到第" + matchNum +"段匹配的内容：");
			System.out.println(scanner.next());
			matchNum++;
		}
		System.out.println("能到这儿吗？");
		//do something
	}

控制台打印结果

使用文件 test.txt 作为输入流实例化 Scanner

扫描到第1段匹配的内容：hasNext()
扫描到第2段匹配的内容：will
扫描到第3段匹配的内容：block
扫描到第4段匹配的内容：your
扫描到第5段匹配的内容：code？
能到这儿吗？



没有阻塞，成功跳出了 while 循环。
————————————————
版权声明：本文为CSDN博主「许大侠0610」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u013568373/article/details/92803182