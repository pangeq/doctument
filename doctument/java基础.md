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
>>![](C:\Users\pangeq\Documents\笔记\image\java\java-virtual-machine-program-language-os.png)
>>JVM 并不只有一种！只要满足JVM规范，每个组织、公司、个人都可以开发自己专属的JVM。我们平时接触的HotSpot VM仅仅是JVM规范的一种实现。
>>其它JVM还有：J9 VM、Zing VM、JRockit VM 等JVM，扩展 各个版本JDK对应JVM规范 [Java SE Specifications](https://docs.oracle.com/javase/specs/index.html)
>- JDK和JRE
>>JDK (Java Development Kit) ,他是功能齐全的Java SDK ，是提供给开发者使用的，能够创建和编译java程序。它包含了JRE,同时还包含了编译Java源码的编译器Javac、Javadoc（文档注释工具）、jdb（调试器）、jcosole（基于JMX的可视化监控工具）、Javap（反编译工具）等等。
>>JRE （Java Runtime Environment） 是Java 运行时环境。他是运行已编译Java程序所需所有内容的集合，主要包括Java虚拟机（JVM）、Java基础类库（Class Library）。

### 3、什么是字节码？采用字节码的好处？
