# ld
# 个人易错总结
## 1.对象的初始化顺序
1.静态代码块
#在java中使用static关键字和{}进行声明   static{}
#它在类被加载的时候就运行且只运行一次，并且优于各种代码块和构造函数
#如果一个类中存在多个代码块则按照书写的顺序执行
#作用：加载配置文件
#2.构造代码块
#使用{}进行声明
#它在创建对象的时候被调用，每次创建对象都会调用一次
#构造代码块依托于构造函数，且优先于构造函数执行
#作用：统计对象创建的次数
#3.构造函数
#构造函数的命名必须与类名完全相同
#不能直接调用,必须通过new运算符创建对象的时候才会调用

#4.执行顺序
#静态代码块----构造代码块----构造函数----普通代码块

#5.父类和子类的执行顺序
#父类静态代码块
#子类静态代码块
#父类构造代码块
#父类构造函数
#子类构造代码块
#子类构造函数

#总结：静态代码块的内容先执行，再执行父类的构造代码块和构造函数，
#然后执行子类的构造代码块和构造函数
