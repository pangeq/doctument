# Java基础常见面试题总结（中）
## 面向对象基础
### 面向对象和面向过程的区别
>两者的主要区别在于解决问题的方式不同：
>- 面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
>- 面向对象会先抽象出对象，然后用对象执行方法的方式解决的问题。
>另外，面向对象开发的程序一般更易维护、易复用、易扩展。
>相关issue：[面向过程：面向过程性能比面向对象高？？](https://github.com/Snailclimb/JavaGuide/issues/431)
>x下面是一个求圆的面积和周长的示例，简单分别展示了面向对象和面向过程两种不同的解决方案。
>**面向对象：**
>```java
>public class Circle {
>    // 定义圆的半径
>    private double radius;
>
>    // 构造函数
>    public Circle(double radius) {
>        this.radius = radius;
>    }
>
>    // 计算圆的面积
>    public double getArea() {
>        return Math.PI * radius * radius;
>    }
>
>    // 计算圆的周长
>    public double getPerimeter() {
>        return 2 * Math.PI * radius;
>    }
>
>    public static void main(String[] args) {
>        // 创建一个半径为3的圆
>        Circle circle = new Circle(3.0);
>
>        // 输出圆的面积和周长
>        System.out.println("圆的面积为：" + circle.getArea());
>        System.out.println("圆的周长为：" + circle.getPerimeter());
>    }
>}
>```
>我们直接定义了圆的半径，并使用半径直接计算出圆的面积和周长
>

### 创建一个对象用什么运算符？对象实体与对象引用有何不同？
>new 运算符，new 创建对象实例（对象实例在堆内存），对象引用指向对象实例（对象引用存放在栈内存中）。
>- 一个对象引用可以指向0个或1个对象（一根绳子可以不系气球，也可以系一个气球）
>- 一个对象可以有n个引用指向它（可以用n根绳子系住一个气球）。

### 对象的相对和引用相等的区别
>- 对象的相等一般比较的是内存中存放的内容是否相等
>- 引用相等一般比较的是他们指向的内存地址是否相等。
>这里举个例子：
>```java
>String str1 = "hello";
>String str2 = new String("hello");
>String str3 = "hello";
>// 使用 == 比较字符串的引用相等
>System.out.println(str1 == str2);
>System.out.println(str1 == str3);
>// 使用 equals 方法比较字符串的相等
>System.out.println(str1.equals(str2));
>System.out.println(str1.equals(str3));
>```
>输出结果：
>```text
>false
>true
>true
>true
>```
>从上面的代码输出结果可以看出：
>- str1和str2不相等，而str1和str3相等。这是因为==运算符比较的是字符串的引用是否相等。
>- str1、 str2、str3 三者的内容都相等。这是因为equals 方法比较的是字符串的内容，即使这些字符串的对象引用不同，只要它们的内容相等，就认为它们是相等的。

### 如果一个类没有声明构造方法，该程序能正确执行吗？
>构造方法是一种特殊的方法，主要作用是完成对象的初始化工作。
>如果一个类没有声明构造方法，也可以执行！因为一个类即使没有声明构造方法也会有默认的不带参数的构造方法。如果我们自己添加了类的构造方法（无论是否有参），Java 就不会添加默认的无参数的构造方法了。
>我们一直在不知不觉地使用构造方法，这也是为什么我们在创建对象的时候后面要加一个括号（因为要调用无参的构造方法）。如果我们重载了有参的构造方法，记得都要把无参的构造方法也写出来（无论是否用到），因为这可以帮助我们在创建对象的时候少踩坑。

### 构造方法有哪些特点？是否可被override？
>构造方法特点如下：
>- 名字与类名相同。
>- 没有返回值，但不能用 void 声明构造函数。
>- 生成类的对象时自动执行，无需调用。
>构造方法不能被 override（重写）,但是可以 overload（重载）,所以你可以看到一个类中有多个构造函数的情况。

### 面向对象三大特征
>封装
>>封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。就好像我们看不到挂在墙上的空调的内部的零件信息（也就是属性），但是可以通过遥控器（方法）来控制空调。如果属性不想被外界访问，我们大可不必提供方法给外界访问。但是如果一个类没有提供给外界访问的方法，那么这个类也没有什么意义了。就好像如果没有空调遥控器，那么我们就无法操控空凋制冷，空调本身就没有意义了（当然现在还有很多其他方法 ，这里只是为了举例子）。
>```java
>public class Student {
>private int id;//id属性私有化
>private String name;//name属性私有化
>
>//获取id的方法
>public int getId() {
>   return id;
>}
>
>//设置id的方法
>public void setId(int id) {
>   this.id = id;
>}
>
>//获取name的方法
>public String getName() {
>   return name;
>}
>
>//设置name的方法
>public void setName(String name) {
>   this.name = name;
>}
>}
>```
>继承
>>不同类型的对象，相互之间经常有一定数量的共同点。例如，小明同学、小红同学、小李同学，都共享学生的特性（班级、学号等）。同时，每一个对象还定义了额外的特性使得他们与众不同。例如小明的数学比较好，小红的性格惹人喜爱；小李的力气比较大。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。
>**关于继承如下 3 点请记住：**
>- 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，只是拥有
>
>- 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
>
>- 子类可以用自己的方式实现父类的方法。（以后介绍）。

>多态
>>多态，顾名思义，表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。
>**多态的特点:**
>- 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
>
>- 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；
>
>- 多态不能调用“只在子类存在但在父类不存在”的方法；
>
>- 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。

### 接口和抽象类有什么共同点和区别？
>共同点：
>- 都不能被实例化。
>- 都可以包含抽象方法。
>- 都可以有默认实现的方法（Java 8 可以用 default 关键字在接口中定义默认方法）。
>
>区别：
>- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系。
>- 一个类只能继承一个类，但是可以实现多个接口。
>- 接口中的成员变量只能是 public static final 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。

### 深拷贝和浅拷贝区别了解吗？什么是引用拷贝？
>关于深拷贝和浅拷贝区别，我这里先给结论：
>- **浅拷贝** ：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
>- **深拷贝** ：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。
>上面的结论没有完全理解的话也没关系，我们来看一个具体的案例！
>
>浅拷贝
>>浅拷贝的示例代码如下，我们这里实现了 Cloneable 接口，并重写了 clone() 方法。
>clone() 方法的实现很简单，直接调用的是父类 Object 的 clone() 方法。
>```java
>public class Address implements Cloneable{
>  private String name;
>  // 省略构造函数、Getter&Setter方法
>  @Override
>  public Address clone() {
>      try {
>          return (Address) super.clone();
>      } catch (CloneNotSupportedException e) {
>          throw new AssertionError();
>      }
>  }
>}
>
>public class Person implements Cloneable {
>  private Address address;
>  // 省略构造函数、Getter&Setter方法
>  @Override
>  public Person clone() {
>      try {
>          Person person = (Person) super.clone();
>          return person;
>      } catch (CloneNotSupportedException e) {
>          throw new AssertionError();
>      }
>  }
>}
>```
>测试：
>```java
>Person person1 = new Person(new Address("武汉"));
>Person person1Copy = person1.clone();
>// true
>System.out.println(person1.getAddress() == person1Copy.getAddress());
>```
>从输出结构就可以看出， person 1 的克隆对象和 person 1 使用的仍然是同一个 Address 对象。

>深拷贝
>>这里我们简单对 Person 类的 clone() 方法进行修改，连带着要把 Person 对象内部的 Address 对象一起复制。
>
>```java
>@Override
>public Person clone() {
>try {
>   Person person = (Person) super.clone();
>   person.setAddress(person.getAddress().clone());
>   return person;
>} catch (CloneNotSupportedException e) {
>   throw new AssertionError();
>}
>}
>
>```
>测试：
>```java
>Person person1 = new Person(new Address("武汉"));
>Person person1Copy = person1.clone();
>// false
>System.out.println(person1.getAddress() == person1Copy.getAddress());
>```

>从输出结构就可以看出，虽然 `person1` 的克隆对象和 `person1` 包含的 `Address` 对象已经是不同的了。
> **那什么是引用拷贝呢？**  简单来说，引用拷贝就是两个不同的引用指向同一个对象。
>![浅拷贝、深拷贝、引用拷贝](https://github.com/pangeq/doctument/blob/uat/image/java/shallow&deep-copy.png)