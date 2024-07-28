### this关键字
this关键字用于引用当前对象的实例。它有以下几种主要用途：

1. **引用当前对象的实例变量**： 当局部变量和实例变量同名时，this关键字可以用于区分它们。
```
public class Example {
    private int value;

    public Example(int value) {
        this.value = value; // this.value指的是实例变量，value指的是参数
    }
}
```

1. **调用当前对象的方法**： 可以使用this关键字来调用当前对象的另一个方法。
```
public class Example {
    public void method1() {
        System.out.println("Method 1");
    }

    public void method2() {
        this.method1(); // 调用当前对象的method1
    }
}
```

1. **调用当前对象的构造函数**： 在一个构造函数中，可以使用this关键字调用同一个类中的另一个构造函数。必须是构造函数的第一行。
```
public class Example {
    private int value;
    private String text;

    public Example(int value) {
        this(value, "default text"); // 调用另一个构造函数
    }

    public Example(int value, String text) {
        this.value = value;
        this.text = text;
    }
}
```
### super关键字
super关键字用于引用父类（超类）的成员。它有以下几种主要用途：

1. **调用父类的构造函数**： 在子类的构造函数中，可以使用super关键字调用父类的构造函数。必须是构造函数的第一行。
```
public class Parent {
    public Parent() {
        System.out.println("Parent constructor");
    }
}

public class Child extends Parent {
    public Child() {
        super(); // 调用父类的构造函数
        System.out.println("Child constructor");
    }
}
```

1. **引用父类的实例变量**： 当子类和父类有同名的实例变量时，可以使用super关键字访问父类的实例变量。
```
public class Parent {
    protected int value = 10;
}

public class Child extends Parent {
    private int value = 20;

    public void printValues() {
        System.out.println(super.value); // 访问父类的value
        System.out.println(this.value);  // 访问子类的value
    }
}
```

1. **调用父类的方法**： 可以使用super关键字调用父类的方法。
```
public class Parent {
    public void method() {
        System.out.println("Parent method");
    }
}

public class Child extends Parent {
    @Override
    public void method() {
        super.method(); // 调用父类的方法
        System.out.println("Child method");
    }
}
```
### 区别总结
| 用途 | this | super |
| --- | --- | --- |
| 引用实例变量 | 引用当前对象的实例变量 | 引用父类的实例变量 |
| 调用方法 | 调用当前对象的方法 | 调用父类的方法 |
| 调用构造函数 | 调用当前类的另一个构造函数 | 调用父类的构造函数 |
| 访问范围 | 当前类的实例 | 父类的实例 |

this主要用于当前对象的引用，而super主要用于父类的引用。
