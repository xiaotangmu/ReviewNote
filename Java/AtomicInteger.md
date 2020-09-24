原文：https://www.jianshu.com/p/bc7b6f14984e

java.util.concurrent.atomic 的包里有AtomicBoolean, AtomicInteger,AtomicLong,AtomicLongArray,
AtomicReference等原子类的类，主要用于在高并发环境下的高效程序处理,来帮助我们简化同步处理.

在Java语言中，++i和i++操作并不是线程安全的，在使用的时候，不可避免的会用到synchronized关键字。而AtomicInteger则通过一种线程安全的加减操作接口。

原文：https://baijiahao.baidu.com/s?id=1647621616629561468&wfr=spider&for=pc

一、从a++说起为什么使用AtomicInteger

我们知道java并发机制中主要有三个特性需要我们去考虑，原子性、可见性和有序性。synchronized关键字可以保证可见性和有序性却无法保证原子性。而这个AtomicInteger的作用就是为了保证原子性。我们先看一个例子。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2262002840,4184061721&fm=173&app=25&f=JPEG?w=422&h=400&s=BA81E14CD2B6866E4244FD1B0000F0C3)

在上面的这个例子中，我们定义了一个变量a。并且使用了5个线程分别去增加。为了保证可见性和有序性我们使用了synchronized关键字对a进行修饰。在这里我们只测试原子性。如果我们第一次接触的话肯定会觉得5个线程，每个线程加10，最后结果一定是50呀。我们可以运行一边测试一波

![img](https://pics2.baidu.com/feed/b64543a98226cffce560e1104605d195f703ead1.jpeg?token=f983e43a096f9832417d01f74c012a06&s=1594ED32494E5E4D52F001DA000050B0)

很明显，可能跟你想象的不一样。为什么会出现这个问题呢？这是因为变量a虽然保证了可见性和有序性，但是缺没有保证原子性。其原因我们可以来分析一下。

对于a++的操作，其实可以分解为3个步骤。

（1）从主存中读取a的值

（2）对a进行加1操作

（3）把a重新刷新到主存

这三个步骤在单线程中一点问题都没有，但是到了多线程就出现了问题了。比如说有的线程已经把a进行了加1操作，但是还没来得及重新刷入到主存，其他的线程就重新读取了旧值。因为才造成了错误。如何去解决呢？方法当然很多，但是为了和我们今天的主题对应上，很自然的联想到使用AtomicInteger。下面我们使用AtomicInteger重新来测试一遍：

![img](https://pics0.baidu.com/feed/5fdf8db1cb1349545da1700a524b095dd1094a2f.jpeg?token=e9ece332f1b5fa751c5705147dfad8e1&s=3A81A14CD2BE966F4659FD0B0000A0C1)

在上面的代码中我们使用了AtomicInteger来定义a，而且使用了AtomicInteger的函数incrementAndGet来对a进行自增操作。现在我们再来测试一遍。

![img](https://pics3.baidu.com/feed/3812b31bb051f8193362b978f6b1d1e82e73e74c.jpeg?token=98c0fddfa5c56fd59900cfe6bb60014e&s=CC10ED12191FCDCE40D4F9DE000080B2)

现在使用了AtomicInteger，不管你测试多少次，最后结果一定是50。为什么会出现这样的结果呢？AtomicInteger又是如何保证了这样的特性呢？下面我们就正式的开始揭开其面纱。

二、原理分析

上面的例子中我们只是调用了incrementAndGet函数来进行自增操作。其实AtomicInteger类为我们提供了很多函数。可以先使用一下。

1、基本使用

![img](https://pics2.baidu.com/feed/42a98226cffc1e17d8afb4bd4d956d06738de930.jpeg?token=85e546fffc327ba37f79bc5ed324b6b2&s=B2D131CED2B6B57E54519C0B000030C1)

最常用的方法就是这么几个。当然了还有很多其他的方法。对于上面几个函数，每一个函数的意思都已经列了出来。意思都很简单。下面我们就通过源码的角度分析一下AtomicInteger的真正原理。

2、源码分析

既然AtomicInteger使用了incrementAndGet函数，那我们就直接来看这个方法，对于其他的方法也是同样的道理。我们直接看源码，这里使用的是jdk1.8的版本，不同的版本会有出入。

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=1200141411,3902210711&fm=173&app=49&f=JPEG?w=446&h=134&s=B8C1A14CDFE4BD7050418C0C0000B0C1)

在这里我们会看到，底层使用的是unsafe的getAndAddInt方法。这里你可能有一个疑问了，这个unsafe是个什么鬼，而且还有一个valueOffset参数又是什么，想要看明白，我们从源码的开头开始看起。

![img](https://pics6.baidu.com/feed/e61190ef76c6a7efefb70385cfff3454f2de668e.jpeg?token=ae405312dfd4fe0cae0efc2de47fd464&s=B29131CE17F0A86854F1940E0000B081)

开头在Unsafe的上面会发现，有一行注释叫做Unsafe.compareAndSwapInt。这又是什么？带着这些疑问我们开始一点一点揭开其面纱。

（1）compareAndSwapInt的含义

compareAndSwapInt又叫做CAS，如果你将来找工作，这个不清楚的话，基本上可以告别java这个方向了。

CAS 即比较并替换，实现并发算法时常用到的一种技术。CAS操作包含三个操作数——内存位置、预期原值及新值。执行CAS操作的时候，将内存位置的值与预期原值比较，如果相匹配，那么处理器会自动将该位置值更新为新值，否则，处理器不做任何操作。

我看过无数篇文章，对这个概念都是这样解释的，但是一开始看会一脸懵逼。我们使用一个例子来解释相信你会更加的清楚。

比如说给你儿子订婚。你儿子就是内存位置，你原本以为你儿子是和杨贵妃在一起了，结果在订婚的时候发现儿子身边是西施。这时候该怎么办呢？你一气之下不做任何操作。如果儿子身边是你预想的杨贵妃，你一看很开心就给他们订婚了，也叫作执行操作。现在你应该明白了吧。

对于CAS的解释我不准备长篇大论讲解。因为里面涉及到的知识点还是挺多的。在这里你理解了其含义就好。

（2）Unsafe的含义

在上面我们主要是讲解了CAS的含义，CAS修饰在Unsafe上面。那这个Unsafe是什么意思呢？

Unsafe是位于sun.misc包下的一个类，Unsafe类使Java语言拥有了类似C语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用Unsafe类会使得程序出错的概率变大，使得Java这种安全的语言变得不再“安全”，因此对Unsafe的使用一定要慎重。

这里说一句题外话，在jdk1.9中，对Usafe进行了删除，所以因为这，那些基于Usafe开发的框架慢慢的都死掉了。

在这里也就是说，Usafe再进行getAndAddInt的时候，首先是先加1，然后对底层对象的地址做出了更改。这个地址是什么呢？这就是涉及到我们的第三个疑问参数了。

（3）valueOffset的含义

这个valueOffset是long类型的，代表的含义就是对象的地址的偏移量。下面我们重新解释一下这行代码。

unsafe.getAndAddInt(this, valueOffset, 1) + 1。这行代码的含义是，usafe通过getAndAddInt方法，对原先对象的地址进行了加1操作。现在应该明白了。我们return的时候，也是直接返回的最新的值。这一点我们对比另外一个方法incrementAndGet就能看出。

![img](https://pics2.baidu.com/feed/80cb39dbb6fd5266af9aa73ebf1d0c2ed4073675.jpeg?token=9cc8153747da873983d52fde2b9580b5&s=BAC1E14CCFE0BD70546D8C0C0000F0C3)

在这个方法的源代码中我们可以看到最后的+1操作没有了，也就是说，直接返回的是旧地址的值，然后再进行自增操作。如何去拿的地址的偏移量呢？是通过下面这个代码。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=3857883826,556911697&fm=173&app=49&f=JPEG?w=467&h=117&s=B8D1A14CCD84BF725E4D641B010010C1)

OK，到了这一步相信你已经知道了，usafe对a的值使用getAndAddInt方法进行了加1操作。然后返回最新的值。那么这个getAndAddInt方法是如何实现的呢？我们可以在进入看看：

![img](https://pics2.baidu.com/feed/32fa828ba61ea8d3ebabbcb1930fab4b241f5880.jpeg?token=bcf4c486c0118b457fc3b00159da9417&s=B281B14CCDC48F7004DD9D0A0000F0C1)

这段代码的含义也很清晰。底层还是通过compareAndSwapInt这个CAS机制来完成的增加操作，

第一个参数var1表示的是当前对象，也就是a。

第二个参数var2表示的是地址偏移量

第三个参数var3表示的是我们要增加的值，这里表示为1

对于AtomicInteger的原理就是这，主要是通过Usafe的方式来完成的。Usafe又是通过CAS机制来实现的，因此想要弄清整个原子系列的真正实现，就是要搞清楚CAS机制。不过我会在下一章节进行讲解。

3、其他方法

对于其他方法其实也是同样的道理，我们可以给出几个看看。

![img](https://pics5.baidu.com/feed/b8389b504fc2d562702343e2b7140bea76c66ca5.jpeg?token=549935772915427e2a7dec1316b50dfd&s=3281F14C53B4A4690E45A40B000070C1)

我们可以看到底层基本上还是Usafe来实现的。Usafe又是经过CAS实现。

三、总结

对于jdk1.8的并发包来说，底层基本上就是通过Usafe和CAS机制来实现的。有好处也肯定有一个坏处。从好的方面来讲，就是上面AtomicInteger类可以保持其原子性。但是从坏的方面来看，Usafe因为直接操作的底层地址，肯定不是那么安全，而且CAS机制也伴随着大量的问题，比如说有名的ABA问题等等。关于CAS机制，我也会在后续的文章中专门讲解。大家可以先根据那个给儿子订婚的例子有一个基本的认识。

