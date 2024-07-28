在 Java 中，函数参数传递的机制是基于值传递（pass-by-value）的，但这可能会引起一些混淆，因为对于不同类型的参数（基本类型和引用类型），其表现形式有所不同。
### 基本类型参数传递
对于基本类型（如`int`,`float`,`char`等），传递的是值的拷贝。这意味着在函数内部对参数的任何修改都不会影响到原始变量。
```
public class Main {
    public static void main(String[] args) {
        int a = 10;
        modifyValue(a);
        System.out.println(a); // 输出 10
    }

    public static void modifyValue(int x) {
        x = 20;
    }
}
```
在这个例子中，`modifyValue`函数内部修改了`x`的值，但这不会影响到`main`函数中的`a`。
### 引用类型参数传递
对于引用类型（如对象、数组等），传递的也是值的拷贝，但这个值是对象的引用（即指针）。这意味着在函数内部可以通过引用修改对象的内容，但不能改变引用本身指向的对象。
```
public class Main {
    public static void main(String[] args) {
        MyObject obj = new MyObject();
        obj.value = 10;
        modifyObject(obj);
        System.out.println(obj.value); // 输出 20
    }

    public static void modifyObject(MyObject o) {
        o.value = 20;
    }
}

class MyObject {
    int value;
}
```
在这个例子中，`modifyObject`函数可以通过引用`o`修改`obj`的内容，因此`main`函数中的`obj.value`被修改为`20`。<br />但是，如果试图在函数内部改变引用本身指向另一个对象，这种修改不会影响到原始引用。
```
public class Main {
    public static void main(String[] args) {
        MyObject obj = new MyObject();
        obj.value = 10;
        changeReference(obj);
        System.out.println(obj.value); // 输出 10
    }

    public static void changeReference(MyObject o) {
        o = new MyObject();
        o.value = 20;
    }
}

class MyObject {
    int value;
}
```
在这个例子中，`changeReference`函数试图改变引用`o`指向一个新的对象，但这不会影响到`main`函数中的`obj`。
### 总结

- **基本类型**：传递的是值的拷贝，函数内部的修改不会影响到原始变量。
- **引用类型**：传递的是引用的拷贝，函数内部可以通过引用修改对象的内容，但不能改变引用本身指向的对象。

这种传递机制有时会被误解为“引用传递”（pass-by-reference），但实际上 Java 中所有参数传递都是值传递。只不过对于引用类型，传递的是引用的值。
