### 1. 主动引用
类的初始化是在遇到主动引用时触发的。以下是几种主动引用的情况：
#### 1.1 创建类的实例
当使用new关键字创建类的实例时，类会被初始化。例如：
```
MyClass obj = new MyClass();
```
#### 1.2 访问类的静态变量或静态方法
当访问类的静态变量或调用静态方法时，类会被初始化。例如：
```
System.out.println(MyClass.staticVar);
MyClass.staticMethod();
```
#### 1.3 反射
通过反射 API 对类进行反射调用时，类会被初始化。例如：
```
Class.forName("com.example.MyClass");
```
#### 1.4 初始化子类
当初始化一个类的子类时，父类会被初始化。例如：
```
class Parent {
    static {
        System.out.println("Parent initialized");
    }
}

class Child extends Parent {
    static {
        System.out.println("Child initialized");
    }
}

public class Main {
    public static void main(String[] args) {
        Child child = new Child(); // 输出：Parent initialized, Child initialized
    }
}
```
#### 1.5 Java 虚拟机启动时
包含main方法的类在虚拟机启动时会被初始化。例如：
```
public class Main {
    static {
        System.out.println("Main class initialized");
    }

    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
### 2. 被动引用（Passive Reference）
以下几种情况不会触发类的初始化，称为被动引用：
#### 2.1 通过子类引用父类的静态字段
通过子类引用父类的静态字段，不会导致子类初始化。例如：
```
class Parent {
    static int value = 42;
}

class Child extends Parent {
    static {
        System.out.println("Child initialized");
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Child.value); // 输出：42，不会触发 Child 的初始化
    }
}
```
#### 2.2 定义对象数组
定义类的对象数组不会触发类的初始化。例如：
```
MyClass[] array = newMyClass[10]; // 不会触发 MyClass 的初始化
```
#### 2.3 常量引用
引用常量不会触发类的初始化，因为常量在编译阶段会存入调用类的常量池中。例如：
```
class MyClass {
    static final int CONSTANT = 42;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(MyClass.CONSTANT); // 不会触发 MyClass 的初始化
    }
}
```
### 总结
Java 类的初始化时机主要是在类的主动引用时，包括创建实例、访问静态变量或方法、反射调用、初始化子类以及虚拟机启动时。而在一些被动引用的情况下，如通过子类引用父类的静态字段、定义对象数组和引用常量，则不会触发类的初始化。
