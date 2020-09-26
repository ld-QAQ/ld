# ld
# 个人易错总结
## 1.对象的初始化顺序
1.静态代码块  
在java中使用static关键字和{}进行声明   static{}  
它在类被加载的时候就运行且只运行一次，并且优于各种代码块和构造函数  
如果一个类中存在多个代码块则按照书写的顺序执行  
作用：加载配置文件  
2.构造代码块  
使用{}进行声明  
它在创建对象的时候被调用，每次创建对象都会调用一次  
构造代码块依托于构造函数，且优先于构造函数执行  
作用：统计对象创建的次数  
3.构造函数  
构造函数的命名必须与类名完全相同  
不能直接调用,必须通过new运算符创建对象的时候才会调用  

4.执行顺序  
静态代码块----构造代码块----构造函数----普通代码块  

5.父类和子类的执行顺序  
父类静态代码块  
子类静态代码块  
父类构造代码块  
父类构造函数  
子类构造代码块  
子类构造函数  

总结：静态代码块的内容先执行，再执行父类的构造代码块和构造函数,然后执行子类的构造代码块和构造函数  

## 2.接口与抽象类
接口和抽象类的区别是什么？  
接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），而抽象类可以有非抽象的方法。  
接口中除了 static、final 变量，不能有其他变量，而抽象类中则不一定。  
一个类可以实现多个接口，但只能实现一个抽象类。接口自己本身可以通过 extends 关键字扩展多个接口。  
接口方法默认修饰符是 public，抽象方法可以有 public、protected 和 default 这些修饰符（抽象方法就是为了被重写所以不能使用 private 关键字修饰！）。  
从设计层面来说，抽象是对类的抽象，是一种模板设计，而接口是对行为的抽象，是一种行为的规范。  

在 JDK8 中，接口也可以定义静态方法，可以直接用接口名调用。实现类和实现是不可以调用的。  
jdk1.8后，接口方法可以有方法体，需要用default或static修饰。实现类无需重新。当然default方法重写也是可以的。如果一个类实现多个接口，多个接口有相同default方法，则子类必须重写该方法  
jdk9 的接口被允许定义私有方法 。  

在 jdk 7 或更早版本中，接口里面只能有常量变量和抽象方法。这些接口方法必须由选择实现接口的类实现。  
jdk8 的时候接口可以有默认方法和静态方法功能。  
Jdk 9 在接口中引入了私有方法和私有静态方法。  

## 3.String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的?
简单的来说：String 类中使用 final 关键字修饰字符数组来保存字符串，private final char value[]，所以 String 对象是不可变的。  

在 Java 9 之后，String 类的实现改用 byte 数组存储字符串 private final byte[] value;  

而 StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串char[]value 但是没有用 final 关键字修饰，所以这两种对象都是可变的。  

StringBuilder 与 StringBuffer 的构造方法都是调用父类构造方法也就是AbstractStringBuilder 实现的。  

### 线程安全性

String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。  

### 性能

每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。  

对于三者使用的总结：  

操作少量的数据: 适用 String
单线程操作字符串缓冲区下操作大量数据: 适用 StringBuilder  
多线程操作字符串缓冲区下操作大量数据: 适用 StringBuffer  
## 4.Object 类的常见方法总结
Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：  

public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。  

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。  
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。  

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。  

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。  

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。  

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。  

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。  

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。  

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念  

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作  
## 5. == 与 equals(重要)
== : 它的作用是判断两个对象的地址是不是相等。即，判断两个对象是不是同一个对象(基本数据类型==比较的是值，引用数据类型==比较的是内存地址)。  

equals() : 它的作用也是判断两个对象是否相等。但它一般有两种使用情况：  

情况 1：类没有覆盖 equals() 方法。则通过 equals() 比较该类的两个对象时，等价于通过“==”比较这两个对象。  
情况 2：类覆盖了 equals() 方法。一般，我们都覆盖 equals() 方法来比较两个对象的内容是否相等；若它们的内容相等，则返回 true (即，认为这两个对象相等)。  
举个例子：  

public class test1 {  
    public static void main(String[] args) {  
        String a = new String("ab"); // a 为一个引用  
        String b = new String("ab"); // b为另一个引用,对象的内容一样  
        String aa = "ab"; // 放在常量池中  
        String bb = "ab"; // 从常量池中查找  
        if (aa == bb) // true  
            System.out.println("aa==bb");  
        if (a == b) // false，非同一对象  
            System.out.println("a==b");  
        if (a.equals(b)) // true  
            System.out.println("aEQb");  
        if (42 == 42.0) { // true  
            System.out.println("true");  
        }  
    }  
}  
说明：  

String 中的 equals 方法是被重写过的，因为 object 的 equals 方法是比较的对象的内存地址，而 String 的 equals 方法比较的是对象的值。  
当创建 String 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。  
## 6.hashCode 与 equals (重要)
面试官可能会问你：“你重写过 hashcode 和 equals 么，为什么重写 equals 时必须重写 hashCode 方法？”  

6.1. hashCode（）介绍  
hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。hashCode() 定义在 JDK 的 Object.java 中，这就意味着 Java 中的任何类都包含有 hashCode() 函数。  

散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）  

6.2. 为什么要有 hashCode  
我们先以“HashSet 如何检查重复”为例子来说明为什么要有 hashCode： 当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与该位置其他已经加入的对象的 hashcode 值作比较，如果没有相符的 hashcode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals()方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。（摘自我的 Java 启蒙书《Head first java》第二版）。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。  

通过我们可以看出：hashCode() 的作用就是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。hashCode()在散列表中才有用，在其它情况下没用。在散列表中 hashCode() 的作用是获取对象的散列码，进而确定该对象在散列表中的位置。  

6.3. hashCode（）与 equals（）的相关规定  
如果两个对象相等，则 hashcode 一定也是相同的  
两个对象相等,对两个对象分别调用 equals 方法都返回 true  
两个对象有相同的 hashcode 值，它们也不一定是相等的  
因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖  
hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）  
