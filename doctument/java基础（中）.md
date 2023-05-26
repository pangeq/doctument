# java基础常见面试题总结（中）
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