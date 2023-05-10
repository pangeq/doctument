# java基础
## 一、基础概念与尝试
### 1、java语言特点
>- 简单易学
>- 面向对象 （封装、继承、多态）
>- 平台无关性 （java虚拟机实现平台无关性）
>- 支持多线程 （c++ 11 之前 没有内置多线程机制，因此必须调用操作系统内的多线程功能来进行多线程程序设计，而java语言却提供了多线程支持，c++ 11 2011年 c++ 引入了多线程库）
>- 可靠性
>- 安全性
>- 支持网络编程并且很方便 （Java 语言诞生本身就是为了简化网络编程而设计的）
>- 编译和解释并存
>  拓展：“Write Once,Run AnyWhere” （一次编写，随处运行），java语言最初优势就是跨平台，然而经过多年发展，跨平台已经不再是java最大的卖点，各种JDK特性也不是。目前市面上虚拟化的技术的技术已经非常成熟，比如你可以通过docker 就很容易实现跨平台。在我看来，Java 强大的是他的生态！

### 2、JVM vs JDK vs JRE
>- JVM
>>Java 虚拟机（JVM） ，是运行Java字节码的虚拟机。JVM有针对不同系统特定实现（Windows，Linux，MacOs）,目的是使用相同的字节码，他们都会给出相同的结果。字节码和不同系统的JVM是实现Java语言“一次编译，随处运行”的关键所在
>>![运行在Java虚拟机之上的编程语言](https://github.com/pangeq/doctument/tree/uat/image/java/java-virtual-machine-program-language-os.png)
>>JVM 并不只有一种！只要满足JVM规范，每个组织、公司、个人都可以开发自己专属的JVM。我们平时接触的HotSpot VM仅仅是JVM规范的一种实现。
>>其它JVM还有：J9 VM、Zing VM、JRockit VM 等JVM，扩展 各个版本JDK对应JVM规范 [Java SE Specifications](https://docs.oracle.com/javase/specs/index.html)
>- JDK和JRE
>>JDK (Java Development Kit) ,他是功能齐全的Java SDK ，是提供给开发者使用的，能够创建和编译java程序。它包含了JRE,同时还包含了编译Java源码的编译器Javac、Javadoc（文档注释工具）、jdb（调试器）、jcosole（基于JMX的可视化监控工具）、Javap（反编译工具）等等。
>>JRE （Java Runtime Environment） 是Java 运行时环境。他是运行已编译Java程序所需所有内容的集合，主要包括Java虚拟机（JVM）、Java基础类库（Class Library）。
>>![JDK包含JRE](https://github.com/pangeq/doctument/blob/uat/image/java/jdk-include-jre.png)

### 3、什么是字节码？采用字节码的好处？
>- 在java中，JVM 可以理解的代码就叫做字节码（即扩展名为.class的文件），他不面向任何特定的处理器，只面向虚拟机。Java语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以，Java程序运行时相对来说还是高效的（不过，和C++，Rust，Go等语言还是有一定的差距），而且，由于字节码并不针对一种特定的机器，因此，Java程序无需重新编译便可以在多种不同操作系统的计算机上运行。
>>![Java程序转变成机器代码的过程](https://github.com/pangeq/doctument/blob/uat/image/java/java-code-to-machine-code.png)
>- 我们需要格外注意点 .class ->机器码 这一步JVM加载器首先加载字节码文件，然后再通过解释器逐步解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码快是经常需要被调用的（也就是所谓的热点代码），所以后面引进了 JIT(just-in-time compilation) 编译器，而JIT属于运行时编译。当JIT编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道机器码的运行效率肯定是高于Java解释器的。这也就解释了我们为什么经常说Java是编译与解释共存的语言。
>>![Java程序转变成机器代码的过程](https://github.com/pangeq/doctument/blob/uat/image/java/java-code-to-machine-code-with-jit.png)
>- HotSpot 采用了惰性评估（Lazy Evaluation）的做法，根据二八定律，消耗大部分系统资源的只有那一小部分代码（热点代码），而这也就是JIT所需要编译的部分。JVM会根据代码每次被执行的情况收集信息并相应的做出一些优化，因此执行的次数越多，它的速度就越快。JDK9 引入了一种新的编译模式AOT(Ahead of Time Compilation)，他是直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销。JDK支持分成编译和AOT协作使用。
>- JDK、JRE、JVM、JIT四者的关系
>>![JDK、JRE、JVM、JIT四者的关系](https://github.com/pangeq/doctument/blob/uat/image/java/jdk-jre-jvm-jit.png)
>>![JVM大致结构模型](https://github.com/pangeq/doctument/blob/uat/image/java/jvm-rough-structure-model.png)
>- 为什么不全部使用AOT呢？
>>AOT可以提前编译节省启动时间，那为什么不全部使用这种编译方式？
>>这个和JAVA语言的动态特性有千丝万缕的联系。举个例子，CGLIB动态代理使用的是ASM技术，而这种技术大致原理是运行时直接在内存中生成并加载修改后的字节码文件也就是.class 文件，如果全部使用AOT提前编译，也就不能使用ASM技术了。为了支持类似的动态特性，所以选择使用JIT即时编译器。