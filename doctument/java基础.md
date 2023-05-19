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
>- 为什么说JAVA语言“编译与解释并存”？
>>我们可以将高级编程语言根据执行方式分成两种：
>>- 编译型：编译型语言会通过编译器将源代码一次性翻译成可供该平台执行的机器码。一般情况下，编译语言执行的速度比较快，开发效率比较低。常见的语言有C、C++、GO、Rust。
>>- 解释型：解释型语言会将源代码一句一句的解释为机器码后再执行。解释性语言开发效率比较快，执行效率比较低。常见的语言 Python、Javascript、PHP。
>>![编译型语言和解释型语言](https://github.com/pangeq/doctument/blob/06b38673ab0798e73cc9cd7123c07e0f87913efb/image/java/java-virtual-machine-program-language-os.png)
>>- 根据维基百科介绍：为了改善编译语言的效率而发展出的即时编译技术，已经缩小了这两种类型语言的差距。这种技术混合了编译语言与解释型语言的优点，他像编译语言一样，先把程序源代码编译成字节码。到执行时将字节码直译，之后执行。Java和LLVM是这种技术的代表产物。
>>- 为什么说JAVA语言“编译与解释并存 ：这是因为Java具有这两种解释性、编译型语言的特征，因为Java程序需要先编译，后解释两个步骤，有Java编写的程序需要先经过编译步骤，生成字节码（.class文件），这种字节码必须由Java解释器来进行解释执行。

### 3、Oracle JDK vs OpenJDK
>- 来源：2006年 ，SUN公司将Java开源，也就有了OpenJDK 。2009年年，Oracle将SUN公司收购，于是自己在OpenJDK的基础上搞了一个OracleJDK。Oracle JDK不是开源的，并且刚开始的几个版本（Java8~Java11）还会相比于OpenJDK 添加了一些特有的功能和工具。
>其次，对于Java7而言，OpenJDK 和 OracleJDK 十分接近。Oracle JDK 是基于 OpenJDK 开发的，只添加了一些小功能，由Oracle工程师参与维护。
>区别：
>- 是否开源：OpenJDK 是一个参考模型并且完全开源，OracleJDK 是基于OpenJDK实现，并不完全开源。openJdk开源项目：https://github.com/openjdk/jdk
>- 是否免费：OpenJDK 完全免费；OracleJDK 会提供免费版本，但会有期限限制。JDK17 之后的版本可以免费和分发商用，但仅有3年时间，三年后无法免费商用。不过，JDK8u221 之前的只要不升级，则可以无期限免费。
>- 功能性：OracleJDK 在OpenJDk的基础上添加了一些特有的功能和工具。比如 Java Flight Recorder （JFR,一种监控工具），Java Mission Control （JMC,一种监控工具）。不过在Jdk11 之后，OracleJDK 和 OpenJDK 的功能基本一致，之前oracle大多数的私有组件也都捐赠给了开源组织。
>- 稳定性：OpenJDK 不通过LTS服务（长期服务），而OracleJDK大概每三年会推出一个LTS版本进行长期维护。不过很多公司的都基于OpenJdk 提供了对应OraclJDK 周期相同的LTS 版本，因此两者的稳定性其实也差不多
>- 协议性：OracleJDK 使用BCL/OTN 协议获得许可，而OpenJDK 根据GPL v2获得许可。
>既然OracleJDK 这么好，为什么OpenJdK 还会存在？
>- OpenJDK 是开源的 开源的意味着你可以根据自己的需要进行修改、优化。例如Alibaba 基于OpenJDK 开发了 Dragonwell8 https://github.com/alibaba/dragonwell8
>- OpenJDK 是商业免费的，（这也是为什么yum包管理器上默认安装的是OpenJDK）。虽然OracleJDK 也是商业免费（JDK8）,但不是所有版本免费。
>- OpenJDK更新频率更快。Oracle JDK一般6个月发布一次新版本，而OpenJDK 一般三个月发布一次新版本。（这也是oracleJdk 稳定的原因，现在OpenJDK 上试试水，把问题解决后才会放在 Oracle JDK上。）
>>![oracle jdk release cadence](https://github.com/pangeq/doctument/blob/uat/image/java/oracle-jdk-release-cadence.jpg)
>- Oracle JDK 和 Open JDK 如何选择？
>>建议选择OpenJdk 或者基于OpenJDK 的发行版，比如AWS 的Amazon Corretto,阿里巴巴的Alibaba Dragonwell。
>- 扩展一下：
>> BCL协议（Oracle Binary Code License Agreement）： 可以使用JDK（支持商用），但不可以修改
>> OTN（Oracle Technology NetWork License Agreement）：11及之后新发布的JDK都用的是这个协议，可以自己私下用，商用需要付费。
>> ![](https://github.com/pangeq/doctument/blob/06b38673ab0798e73cc9cd7123c07e0f87913efb/image/java/up-5babce06ef8fad5c4df5d7a6cf53d4a7901.webp)

### Java 和 C++ 区别？
> 同：都是面向对象语言，都支持封装、继承、多态
> 异：
>> - java 不提供指针来直接访问内存，程序内存更加安全
>> - java 类是单继承，C++支持多重继承，虽然java类不支持多继承，但是接口支持。
>> - java有自动内存管理垃圾回收机制（GC）,不需要管理员手动释放。
>> - C++ 同时支持方法重载和操作符重载，java 只支持方法重载，操作符重载操作复杂，违背了java 最初设计思想。

## 二、基本语法
### 注释有哪几种形式？
>![Java 注释类型总结](https://github.com/pangeq/doctument/blob/uat/image/java/java-annotation-types.png)
>- 单行注释：通常用于解释某方法内单行代码作用
>- 多行注释：用于解释一段代码左右
>- 文档注释：通常用于生成java 开发文档
>用的比较多的是单行注释和文档注释，多行注释则相对使用较少
> ```java
> /**
> *文档注释
> */
> //单行注释 
> ```

> 注释是代码的解释说明书，利于人们方便解读，快速理清代码之间的关系，注释不是越详细越好。好的代码本身就是注释，我们要尽量规范和美化自己的代码，来减少不必要的注释。
> 若编程语言有足够表达力，就不需要注释，通过代码来阐述即可。
> ```java
> // check to see if the employee is eligible for full benefits
> if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)){}
> //同义可以用如下来表达：
> if (employee.isEligibleForFullBenefits())
> ```

### 标识符和关键字的区别是什么
>在我们编写代码时侯，需要大量的为程序、类、方法、变量等取名字，于是就有了标识符。简单来说，标识符就是一个名字。
>有一些标识符被java赋予了特殊的含义，只能用于特定地方，这些特殊的标识符就被称为关键字，关键字是被赋予特殊含义的标识符。就比如我们要开一家店，需要给这个店取名字，但是名字不可以是“警察局”，因为“警察局”已经被赋予了特殊的含义。

### Java 语言关键字有哪些?
| 分类 | 关键字 |  |  |  |  |  |  |
| :--- | :--: | :----: | :----: | :----: | :----: | :----: | :----: |
| 访问控制 | private | protectoed | public |||||
| 类，方法和变量修饰符 | abstract | class | extends | final | implements | interface | native |
| | new | static | stricfp | synchronized | transient | volatile | enumn |
| 程序控制 | break | continue | return | do | while | if | else | 
| | for | instanceof | switch | case | default | assert | |
| 错误处理 | try | catch | throw | throws | finally | | |
| 包相关 | import | package |  |  |  |  |  |
| 基本类型 | boolean | byte | char | double | float | int | long |
| | short | |  |  |  |  |  |  |
| 变量引用 | super | this | void |  |  |  |  |
| 保留字 | goto | const | |  |  |  |  |
>Tips：所以关键字都是小写，在IDEA 会以特殊颜色现实
>default 这个关键字很特殊，既属于程序控制，又属于类，方法和变量修饰符，还属于访问控制。
>- 在程序控制中，当在 **switch** 中匹配不到任何情况时，可以使用 **default** 来编写默认匹配情况。
>- 在类，方法和修饰符情况中，从JDK8中引入了默认方法，可以使用 **default** 来定义一个方法的默认实现。
>- 在访问控制中，如果一个方法前没有任何修饰符，则默认会有一个默认修饰符 **default** ,但这个修饰符手动加上则会报错。
>⚠️ 注意：虽然  **true** ，**false** , 和 **null** 看起来很像关键字，但实际上就是字面意思，同时你也不可以作为标识符来使用。
>官方文档： https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html

### 自增自减运算符
>在写代码过程中，常见的一种情况是整数类型变量需要增加1或者减少1，Java 提供了一种特殊的运算符，用于这种表达式，叫做自增运算符（++）和自减运算符（--）。
>++ 和 -- 运算符可以放在变量之前，也可以放在变量之后，当放在变量之前时，先自增/减，再赋值。当放在变量之后，先赋值，再自增/减；例如 b = ++a ，先自增（自己增加1），再赋值（赋值b）,当 b = a++ 时，先赋值（赋值给b），再自增（自己增加1），也就是 ++a 输出的是 a+1 的值，a++ 输出的是a值。用一句口诀就是：“符号在前，就先加/减，符号在后就后加/减”。

### 移位运算符
>移位运算符是最基本的运算符之一，几乎每种编程语言都会包含这一种运算符。移位操作中，被操作的数据视为二进制数，移位就是将其向左或向右移动若干位的运算。
>移位运算符在各种框架以及JDK源码中使用还是很广泛的，Hash Map（JDK1.8） 的hash方法源码中就用到的移位运算符：
>
>```java
>static final int hash(Object key){
>int h;
>//key.hashcode()：返回散列值也就是hashcode
>//^：按位异或
>// >>>：无符号右移，忽略符号位，空位都以0补齐
>return (key == null ) ? 0 : (h = key.hashcode()) ^ (h >>> 16);
>}
>```
>在java 代码里使用<<、>>和>>>转换成的指令码会更加高效。
>掌握最基本的移位运算符知识还是很有必要的，这不仅可以帮助我们在代码中使用，还可以帮我们在阅读源码时帮助我们理解涉及到移位运算符的代码。
>java 中有三种移位运算符
>![java移位运算符总结](https://github.com/pangeq/doctument/blob/uat/image/java/shift-operator.png)
>- <<：左移运算符，向左移若干位，高位丢弃，低位补零。 **X << 1** ,相当于x乘以2（不溢出的情况下）。
>- \>>：带符号右移，向右移若干位，高位补符号位，低位丢弃。正数高位补0，符数高位补1。 **X>>1** 相当于x除以2
>- \>>>：无符号右移，忽略符号位，空位都以0补齐。
>由于double、float 在二进制中表现特殊，因此不支持位移操作。
>移位操作符实际上支持的类型只有int和long，编译器对short 、byte、char 类型进行位移前，都会将其转换为int类型在操作。