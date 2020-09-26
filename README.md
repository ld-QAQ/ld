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

在 JDK8 中，接口也可以定义静态方法，可以直接用接口名调用。实现类和实现是不可以调用的。如果同时实现两个接口，接口中定义了一样的默认方法，则必须重写，不然会报错。  
jdk9 的接口被允许定义私有方法 。  

在 jdk 7 或更早版本中，接口里面只能有常量变量和抽象方法。这些接口方法必须由选择实现接口的类实现。  
jdk8 的时候接口可以有默认方法和静态方法功能。  
Jdk 9 在接口中引入了私有方法和私有静态方法。  

