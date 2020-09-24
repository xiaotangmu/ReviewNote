https://www.cnblogs.com/lsw9/p/8685623.html

### JRE

　　　JRE是Java Runtime Environment的缩写，顾名思义是java运行时环境，包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的，还有所有的Java类库的class文件，都在lib目录下，并且都打包成了jar。

至于在Windows上的虚拟机是哪个文件呢？就是<JRE安装目录>/bin/client中的jvm.dll。

![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401084338081-741195701.png)

　　　　　　　　　　　　　　　　　　　　（注：jre的版本不同存放 jvm.dll 的包名也有所不同）

### JDK

 

　　Jdk是Java Development Kit的缩写，顾名思义是java开发工具包，是程序员使用java语言编写java程序所需的开发工具包，是提供给程序员使用的。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。

如果你需要运行java程序，只需安装JRE就可以了。如果你需要编写java程序，需要安装JDK。

- 点击“我的电脑->属性->高级系统设置->环境变量”，

　　　　　　![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401094133652-1926848593.png)

- JAVA_HOME变量设置，在系统变量中点击【新建】，变量名处输入“JAVA_HOME”，变值处输入“的:\java\jdk”，如下图，点击【OK】。这里的变量值就是我们JDK的安装目录。

　　　　![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401095431523-1758414443.png)

- 系统变量→寻找 Path 变量→编辑在变量值最后输入 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;（注意原来Path的变量值末尾有没有;号，如果没有，先输入；号再输入上面的代码）

　　　　　　![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401101236313-1601608537.png)

- 系统变量→新建 CLASSPATH 变量

　　　　变量值填写   .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar（注意最前面有一点）

　　　　　　　　![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401101725509-1160306927.png)

- 检验是否配置成功 运行cmd 输入 java -version （java 和 -version 之间有空格）若如图所示 显示版本信息 则说明安装和配置成功。

　　　　 ![img](https://images2018.cnblogs.com/blog/1364838/201804/1364838-20180401101947925-1724240906.png)

　

 　　总结：简单来说，JRE可以支撑Java程序的运行，包括JVM虚拟机（java.exe等）和基本的类库（rt.jar等），JDK可以支持Java程序的开发，包括编译器（javac.exe）、开发工具（javadoc.exe、jar.exe、keytool.exe、jconsole.exe）和更多的类库（如tools.jar）等。

 

​       附上jdk 1.8的安装压缩文件地址： https://pan.baidu.com/s/1UCNq8oEMoptg8i7VEcecIA

　　 附上jre 1.8的安装压缩文件地址： https://pan.baidu.com/s/126wujlJTdhknC1fHSe1LrQ