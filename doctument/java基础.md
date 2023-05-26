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
>>![运行在Java虚拟机之上的编程语言](https://github.com/pangeq/doctument/blob/uat/image/java/java-virtual-machine-program-language-os.png)
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
>- 来源：2006年 ，SUN公司将Java开源，也就有了OpenJDK 。2009年，Oracle将SUN公司收购，于是自己在OpenJDK的基础上搞了一个OracleJDK。Oracle JDK不是开源的，并且刚开始的几个版本（Java8~Java11）还会相比于OpenJDK 添加了一些特有的功能和工具。
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
>- 如果移位的位数超过数值所占有的位数会怎样？
>> 当int 类型左移/右移位数大于等于32位操作时，会先求余（%）后再进行左移/右移操作。也就是说左移/右移32位相当于不进行位移操作（32%32 = 0），左移/右移42位相当于左移/右移10位（42%32 = 10） 。当long类型进行左移/右移操作时，由于long对应的二进制是64位，因此求余操作的基数变成了64.
>> 也就是说：x<<42等同于x<<10，x>>42等同于x>>10，x>>> 42等同于x >>>10
>> 左移运算符代码示例：
>```java
>int i = -1;
>System.out.println("初始数据:" + i);
>System.out.println("初始数据对应的二进制字符串：" + >Integer.toBinaryString(i));
>i <<= 10;
>System.out.println("左移10位后的数据 " + i);
>System.out.println("左移十位后的数据对应的二进制字符 " + >Integer.toBinaryString(i));
>```
>>输出
>```text
>初始数据：-1
>初始数据对应的二进制字符串：11111111111111111111111111111111
>左移 10 位后的数据 -1024
>左移 10 位后的数据对应的二进制字符 11111111111111111111110000000000
>```
>>由于左移位数大于等于32位操作时，会先求余（%）后在进行左移操作，所以下面的代码左移42位相当于左移10位（42%32=10），输出结果和前面的代码一样。
>```java
>int i = -1;
>System.out.println("初始数据：" + i);
>System.out.println("初始数据对应的二进制字符串：" + >Integer.toBinaryString(i));
>i <<= 42;
>System.out.println("左移 10 位后的数据 " + i);
>System.out.println("左移 10 位后的数据对应的二进制字符 " + >Integer.toBinaryString(i));
>```

### continue、break和return的区别是什么？
>在循环结构中，当前循环条件不满足或者循环次数达到要求时，循环会正常结束。但是，有时候可能需要在循环的过程中，当发生了某种条件后，提前终止循环，这就需要用到下面几个关键词：
>- **continue** :指跳出当前的这一次循环，继续下一次循环。
>- **break**：指跳出整个循环体，继续执行循环下面的语句。
>- **return** ：用于跳出所在的方法，结束该方法的运行。return一般有两种用法：
>>- **return**：直接使用return结束方法执行，用于没有返回值函数的方法。
>>- **return value** ：return一个特定的值，用于有返回值函数的方法。
>思考一下：下列语句的运行结果是什么？
>```java
>public static void main(String[] args) {
>       boolean flag = false;
>       for (int i = 0; i <= 3; i++) {
>           if (i == 0) {
>               System.out.println("0");
>           } else if (i == 1) {
>               System.out.println("1");
>               continue;
>           } else if (i == 2) {
>               System.out.println("2");
>               flag = true;
>           } else if (i == 3) {
>               System.out.println("3");
>               break;
>           } else if (i == 4) {
>              System.out.println("4");
>           }
>           System.out.println("xixi");
>       }
>       if (flag) {
>           System.out.println("haha");
>           return;
>       }
>       System.out.println("heihei");
>  }
>```

>运行结果：
>```text
>0
>xixi
>1
>2
>xixi
>3
>haha
>```

## 基本数据类型
### java中的基本数据类型
>java 中有8种基本数据类型，分别为
>- 6种数字类型：
>>- 4种整数型：byte、short、int、long
>>- 2种浮点型：float、double
>- 1种字符类型：char
>- 种布尔型：boolean
>这8种基本数据类型的默认值以及所占空间大小如下：

| 基本类型 | 位数 | 字节 | 默认值  |                  取值范围                  |
| :-------: | :--: | :--: | :-----: | :----------------------------------------: |
|   byte   |  8   |  1   |    0    |                  -128~127                  |
|  short   |  16  |  2   |    0    |                -32768~32767                |
|   int    |  32  |  4   |    0    |           -2147483648~2157483647           |
|   long   |  64  |  8   |   0L    | -9223372036854775808 ~ 9223372036854775807 |
|   char   |  16  |  2   | 'u0000' |                 0 ~ 65535                  |
|  float   |  32  |  4   |   0f    |           1.4E-45 ~ 3.4028235E38           |
|  double  |  64  |  8   |   0d    |     4.9E-324 ~ 1.7976931348623157E308      |
| boolean  |  1   |      |  false  |                true、false                 |

>对于boolean,官方文档未明确定义，它依赖于JVM厂商的具体表现。逻辑上理解是占用1位，但是实际中会考虑到计算机高效存储因素。
>另外，Java的每种基本类型所占存储空间的大小不会像其他大多数语言那样随机器硬件架构的变化而变化。这种所占存储空间的大小的不变性是java程序比用其他大多数语言编写的程序更具有可移植性的原因之一（《java 编程思想》2.2节有提到）
>注意：
>- java 里使用long 类型的数据一定要在数值后加上 **L**,否则将作为整形解析。
>- char a = 'h' char：单引号，String a = "hello" :双引号
>这八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean。

### 基本类型和包装类型的区别？
> ![基本类型vs包装类型](https://github.com/pangeq/doctument/blob/uat/image/java/primitive-type-vs-packaging-type.png)
>- 用途：除了定义一些常量和局部变量之外，我们在其他地方比如方法参数、对象属性中很少会使用基本类型来定义变量。并且，包装类型可用于泛型，而基本类型不可以。
>- 存储方式：基本数据类型的局部变量存放在Java虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被static修饰）存放在java虚拟机的堆中。包装类属于对象类型，我们知道几乎所有的对象实例都存在于堆中。
>- 占比空间：相比于包装类型（对象类型），基本数据类型占用的空间往往非常小。
>- 默认值：成员变量包装类型不赋值就是null,而基本类型有默认值且不是null.
>- 比较方式：对于基本数据类型，==比较的是值。对于包装类型来说，==比较的是对象的内存地址。所有整形包装类对象之间值的比较，全部使用equals()方法。
>为什么说几乎所有的对象实例都存在于堆中呢？
>这是因为HotSpot虚拟机引入了JIT优化之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换线上来实现栈上分配，而避免堆上分配内存。
>⚠️注意：基本数据类型存放在栈中是一个常见的误区！基本数据类型的成员变量如果没有被static修饰的话（不建议这么使用，应该要使用基本数据类型对应的包装类型），就存放在堆中。
>```java
>class BasicTypeVar{
>  private int x;
>}
>```

### 包装类的缓存机制了解吗？
> Java基本数据类型的包装类型大部分都用到了缓存机制来提升性能。
> Byte、Short、Integer、long这4种包装类默认创建了数值[-128,127]的相应类型的缓存数据，Character创建了数值在[0,127]范围的缓存数据，Boolean直接返回TRUE or FALSE。
> Integer 缓存源码：
> ```java
> public static Integer valueOf(int i) {
>     if (i >= IntegerCache.low && i <= IntegerCache.high)
>         return IntegerCache.cache[i + (-IntegerCache.low)];
>     return new Integer(i);
> }
> private static class IntegerCache {
>     static final int low = -128;
>     static final int high;
>     static {
>         // high value may be configured by property
>         int h = 127;
>     }
> }
> 
> ```

>Character 缓存源码：
>```java
>public static Character valueOf(char c) {
>    if (c <= 127) { // must cache
>      return CharacterCache.cache[(int)c];
>    }
>    return new Character(c);
>}
>
>private static class CharacterCache {
>    private CharacterCache(){}
>    static final Character cache[] = new Character[127 + 1];
>    static {
>        for (int i = 0; i < cache.length; i++)
>            cache[i] = new Character((char)i);
>    }
>
>}
>```

>Boolean 缓存源码：
>```java
>public static Boolean valueOf(boolean b) {
>return (b ? TRUE : FALSE);
>}
>```
>如果超出对应范围仍然会创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。
>两种浮点数类型的包装类 Float、Double 并没有实现缓存机制。
>```java
>Integer i1 = 33;
>Integer i2 = 33;
>System.out.println(i1 == i2);// 输出 true
>
>Float i11 = 333f;
>Float i22 = 333f;
>System.out.println(i11 == i22);// 输出 false
>
>Double i3 = 1.2;
>Double i4 = 1.2;
>System.out.println(i3 == i4);// 输出 false
>```
>下面我们来看一个问题：下面的代码的输出结果是true还是false呢？
>```java
>Integer i1 = 40;
>Integer i2 = new Integer(40);
>System.out.println(i1==i2);
>```
>Integer i1 = 40 这一行代码会发生装箱，也就是说这行代码等价于 Integer i1 = Integer.valueOf(40)。因此，i1直接使用的是缓存中的对象，而 Integer i2 = new Integer(40);会直接创建新的对象。
>因此，答案是false。
>记住：所有整型包装类对象之间值的比较，全部使用equals 方法比较。
>![](https://github.com/pangeq/doctument/blob/uat/image/java/up-1ae0425ce8646adfb768b5374951eeb820d.webp)

### 自动装箱和自动拆箱了解吗？原理是什么？
>什么是自动拆装箱？
>- 装箱：将基本类型用他们对应的引用类型包装起来
>- 拆箱：将包装类型转换为基本数据类型。
>举例：
>```java
>Integer i = 10;  //装箱
>int n = i;   //拆箱
>```
>上面这两行代码对应的字节码为：
>```java
>  L1
>
>   LINENUMBER 8 L1
>
>   ALOAD 0
>
>   BIPUSH 10
>
>   INVOKESTATIC java/lang/Integer.valueOf (I)Ljava/lang/Integer;
>
>   PUTFIELD AutoBoxTest.i : Ljava/lang/Integer;
>
>  L2
>
>   LINENUMBER 9 L2
>
>   ALOAD 0
>
>   ALOAD 0
>
>   GETFIELD AutoBoxTest.i : Ljava/lang/Integer;
>
>   INVOKEVIRTUAL java/lang/Integer.intValue ()I
>
>   PUTFIELD AutoBoxTest.n : I
>
>   RETURN
>```
>从字节码中，我们发现装箱其实就是调用了包装类的valueOf()方法，拆箱其实就是调用了XXXValue()方法。
>因此：
>- Integer i = 10等价于 Integer i = Integer。valueOf(10);
>- int n = i 等价于 int n = i.intValue();
>注意：如果频繁拆装箱的话，也会严重影响系统性能。我们应该尽量避免不必要的拆装箱操作。
>```java
>private static long sum() {
>    // 应该使用 long 而不是 Long
>    Long sum = 0L;
>    for (long i = 0; i <= Integer.MAX_VALUE; i++)
>        sum += i;
>    return sum;
>}
>```

### 为什么浮点数运算时候会有精度丢失风险？
>浮点数运算精度丢失代码示例：
>```java
>float a = 2.0f - 1.9f;
>float b = 1.8f - 1.7f;
>System.out.println(a);// 0.100000024
>System.out.println(b);// 0.099999905
>System.out.println(a == b);// false
>```
>为什么会出现这个问题呢？
>这个和计算机保存浮点数的机制有很大的关系。我们都知道计算机是二进制的，而且计算机在表示一个数字时，宽度是有限的，无限循环小数存储在计算机时，只能被截断，所以就会导致小数精度发生损失的情况。这也就解释了为什么浮点数没有办法用二进制精确表示。
>就比如说十进制下的0.2就没办法转换成二进制小数：
>```java
>// 0.2 转换为二进制数的过程为，不断乘以 2，直到不存在小数为止，
>// 在这个计算过程中，得到的整数部分从上到下排列就是二进制的结果。
>0.2 * 2 = 0.4 -> 0
>0.4 * 2 = 0.8 -> 0
>0.8 * 2 = 1.6 -> 1
>0.6 * 2 = 1.2 -> 1
>0.2 * 2 = 0.4 -> 0（发生循环）
>...
>```
>关于浮点数的更多内容，建议看一下[计算机系统基础（四）浮点数](http://kaito-kidd.com/2018/08/08/computer-system-float-point/)这篇文章

### 如何解决浮点数运算的精度丢失问题？
> BigDecimal 可以实现对浮点数的运算，不会造成精度丢失。通常情况下，大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过**Bigdecimal** 来做的。
> ```java
> BigDecimal a = new BigDecimal("1.0");
> BigDecimal b = new BigDecimal("0.9");
> BigDecimal c = new BigDecimal("0.8");
> 
> BigDecimal x = a.subtract(b);
> BigDecimal y = b.subtract(c);
> 
> System.out.println(x); /* 0.1 */
> System.out.println(y); /* 0.1 */
> System.out.println(Objects.equals(x, y)); /* true */
> ```
>关于 **Bigdecimal** 下边会有详细介绍

### 超过long整型的数据应该如何表示？
>基本数值类型都有一个表达范围，如果超过这个范围就会有数值溢出的风险。
>在Java中，64位long 整型是最大的整数类型。
>```java
>long l = Long.MAX_VALUE;
>System.out.println(l + 1); // -9223372036854775808
>System.out.println(l + 1 == Long.MIN_VALUE); // true
>```
>BigInteger 内部使用int[] 数组来存储任意大小的整型数据。
>相对于常规的整型类型来说，BigInteger 运算的效率相对会较低。

## 变量
### 成员变量与局部变量的区别？
>![成员变量vs局部变量](https://github.com/pangeq/doctument/blob/uat/image/java/member-var-vs-local-var.png)
>- 语法形式：从语法上来看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 public、private、static等修饰符所修饰，而局部变量不能被访问控制修饰符及static所修饰；但是，成员变量和局部变量都能被final修饰。
>- 存储方式：从变量在内存中的存储方式来看，如果成员变量是使用static修饰的，那么这个成员变量是属于类的，如果没有使用static修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
>- 生存时间：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。
>- 默认值：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外：被final修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。
>成员变量和局部变量代码示例：
>```java
>public class VariableExample {
>
>    // 成员变量
>    private String name;
>    private int age;
>
>    // 方法中的局部变量
>    public void method() {
>        int num1 = 10; // 栈中分配的局部变量
>        String str = "Hello, world!"; // 栈中分配的局部变量
>        System.out.println(num1);
>        System.out.println(str);
>    }
>
>    // 带参数的方法中的局部变量
>    public void method2(int num2) {
>        int sum = num2 + 10; // 栈中分配的局部变量
>        System.out.println(sum);
>    }
>
>    // 构造方法中的局部变量
>    public VariableExample(String name, int age) {
>        this.name = name; // 对成员变量进行赋值
>        this.age = age; // 对成员变量进行赋值
>        int num3 = 20; // 栈中分配的局部变量
>        String str2 = "Hello, " + this.name + "!"; // 栈中分配的局部变量
>        System.out.println(num3);
>        System.out.println(str2);
>    }
>}
>```

### 静态变量有什么作用？
>静态变量也就是被 static 关键字修饰的变量，它可以被类的所有实例共享，无论一个类创建了多少个对象，他们都共享一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。
>静态变量是通过类名来访问的，例如 StaticVariableExample.staticVar (如果被private关键字修饰就无法这样访问了)。
>```java
>public class StaticVariableExample {
>// 静态变量
>public static int staticVar = 0;
>}
>```
>通常情况下，静态变量会被final关键字修饰成为常量。
>```java
>public class ConstantVariableExample {
>    // 常量
>    public static final int constantVar = 0;
>}
>```

### 字符型常量和字符串常量的区别？
>- 形式：字符常量是单引号引起的一个字符，字符串常量是双引号引起的0个或若干个字符。
>- 含义：字符常量相当于一个整型值（ASCII值），可以参加表达式运算；字符串常量代表一个地址值（该字符串在内存中存放位置）。
>- 占内存大小：字符常量只占两个字节；字符串常量占若干个字节。
>⚠️注意 char 在Java中占两个字节。
>字符型常量和字符串常量代码示例：
>```java
>public class StringExample {
>   // 字符型常量
>   public static final char LETTER_A = 'A';
>
>   // 字符串常量
>   public static final String GREETING_MESSAGE = "Hello, world!";
>   public static void main(String[] args) {
>       System.out.println("字符型常量占用的字节数为："+Character.BYTES);
>       System.out.println("字符串常量占用的字节数为："+GREETING_MESSAGE.getBytes().length);
>   }
>}
>```
>输出
>```java
>字符型常量占用的字节数为：2
>字符串常量占用的字节数为：13
>```

## 方法
### 什么是方法的返回值？方法有哪几种类型？
>方法的返回值：是指我们获取到某个方法中的代码执行后产生的结果！（前提是该方法可能产生结果）。返回值的作用是接收结果，使得它可用于其它操作。
>我们可以按照方法的返回值和参数类型将方法分为下面几种：
>1、无参数无返回值方法
>```java
>public void f1() {
>//......
>}
>// 下面这个方法也没有返回值，虽然用到了 return
>public void f(int a) {
>if (...) {
>// 表示结束方法的执行,下方的输出语句不会执行
>return;
>}
>System.out.println(a);
>}
>```
>2、有参无返回值的方法
>```java
>public void f2(Parameter 1, ..., Parameter n) {
>//......
>}
>```
>3、有返回值无参数的方法
>```java
>public int f3() {
>//......
>return x;
>}
>```
>4、有参数有返回值的方法
>```java
>public int f4(int a, int b) {
>    return a * b;
>}
>```

### 静态方法为什么不能调用非静态成员？
>这个要结合JVM 的相关知识，主要原因如下：
>1、静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名.静态方法名的的方式直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。
>2、在类的非静态成员不存在的时候静态方法已经存在了，此时调用在内存中还不存在的非静态成员是非法操作。
>```java
>public class Example {
>// 定义一个字符型常量
>public static final char LETTER_A = 'A';
>
>// 定义一个字符串常量
>public static final String GREETING_MESSAGE = "Hello, world!";
>
>public static void main(String[] args) {
>   // 输出字符型常量的值
>   System.out.println("字符型常量的值为：" + LETTER_A);
>
>   // 输出字符串常量的值
>   System.out.println("字符串常量的值为：" + GREETING_MESSAGE);
>}
>}
>```

### 静态方法和实例方法有何不同？
>1、调用方式
>在外部调用静态方法时，可以使用 类名、方法名的方式，也可以使用对象、方法名的方式，而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象。
>不过，需要注意的是一般不建议使用 对象、方法名的方式来调用静态方法。这种方法非常容易造成混淆，静态方法不属于类的某个对象而属于这个类。
>因此，一般建议使用类名.方法名的方式来调用静态方法。
>```java
>public class Person {
>    public void method() {
>      //......
>    }
>
>    public static void staicMethod(){
>      //......
>    }
>    public static void main(String[] args) {
>        Person person = new Person();
>        // 调用实例方法
>        person.method();
>        // 调用静态方法
>        Person.staicMethod()
>    }
>}
>```
>2、访问类成员是否存在限制
>静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制。

### 重载和重写的区别
>重载：发生在同一个类中（或者父类和子类之间），方法名必须相同，参数类型不同、个数不同，方法返回值和修饰符可以不同。
>《Java核心技术》这本书是这样介绍重载的：
>> 如果多个方法（比如 StringBuilder 的构造方法）有相同的名字、不同的参数，便产生了重载。
>>
>```java
>StringBuilder sb = new StringBuilder();
>StringBuilder sb2 = new StringBuilder("HelloWorld");
>```

>> 编译器必须挑选出具体执行哪个方法，它通过用各个方法给出的参数类型与特定方法调用所使用的值类型进行匹配来挑选出相应的方法。如果编译器中不到匹配的参数，就会产生编译时的错误，因为根本不存在匹配，或者没有一个比其他更好（这个过程被称为重载解析（overloading resolution））。
>> java允许重载任何方法，而不只是构造器方法。
>综上：重载就是同一个类中多个同名方法根据不同传参来执行不同的逻辑处理。
>重写：重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。
>- 方法名、参数列表必须相同，子类方法返回值类型应比父类方法返回值类型更小或相等，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。
>- 如果父类方法访问修饰符为 private/final/static 则子类就不能重写该方法，但是被static修饰的方法能够被再次声明。
>- 构造方法无法被重写。
>
>总结：
>综上：重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变。
>

|   区别点   | 重载方法 | 重写方法                                                     |
| :--------: | :------: | :----------------------------------------------------------- |
|  发生范围  | 同一个类 | 子类                                                         |
|  参数列表  | 必须修改 | 一定不能修改                                                 |
|  返回类型  |  可修改  | 子类方法返回值类型应比父类方法返回值类型更小或相等           |
|    异常    |  可修改  | 子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等 |
| 访问修饰符 |  可修改  | 一定不能做更严格的限制（可以降低限制）                       |
|  发生阶段  |  编译期  | 运行期                                                       |
> 方法的重写要遵循“两同两小一大”（以下内容摘录自《疯狂Java讲义》，[issue#892](https://github.com/Snailclimb/JavaGuide/issues/892)）