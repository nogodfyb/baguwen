类的卸载通常发生在以下几种情况下：
### 1. 类加载器被卸载
类的卸载与类加载器的生命周期密切相关。只有当一个类加载器没有任何活动的引用时，JVM 才会考虑卸载由该加载器加载的所有类。因此，类的卸载通常发生在类加载器被卸载的时候。具体条件包括：

- **类加载器没有活动的引用**：即没有任何线程或静态变量引用该类加载器。
- **类加载器加载的所有类都没有活动的引用**：即这些类的实例、静态字段和方法都不再被引用。
### 2. 没有对类的实例的引用
为了卸载一个类，JVM 需要确保没有对该类的实例的引用。这意味着：

- 没有该类的对象实例在堆中。
- 没有对该类的静态字段的引用。
- 没有活动线程在执行该类的方法。
### 3. 没有对类的静态方法和静态字段的引用
如果一个类的静态方法或静态字段仍然被引用，那么该类将不会被卸载。因此，JVM 必须确保：

- 没有线程在执行该类的静态方法。
- 没有对该类的静态字段的引用。
### 4. 没有对类加载器的引用
类的卸载需要确保类加载器本身也没有被引用。这意味着：

- 没有其他类加载器或对象引用该类加载器。
- 该类加载器加载的所有类都可以被卸载。
### 5. 完成垃圾回收
类的卸载通常发生在垃圾回收过程中。垃圾回收器会检查类加载器及其加载的类是否符合卸载条件。如果符合条件，垃圾回收器会卸载这些类并释放相关内存。
### 示例代码
```
public class ClassUnloadingExample {
    public static void main(String[] args) throws Exception {
        // 创建一个新的类加载器
        CustomClassLoader classLoader = new CustomClassLoader();

        // 加载一个类
        Class<?> clazz = classLoader.loadClass("MyClass");

        // 创建类的实例
        Object instance = clazz.newInstance();

        // 清除对类加载器和类实例的引用
        classLoader = null;
        instance = null;

        // 请求垃圾回收
        System.gc();

        // 让垃圾回收器有时间运行
        Thread.sleep(1000);

        System.out.println("Class unloading example completed.");
    }

    static class CustomClassLoader extends ClassLoader {
        @Override
        public Class<?> loadClass(String name) throws ClassNotFoundException {
            if ("MyClass".equals(name)) {
                byte[] classData = getClassData();
                return defineClass(name, classData, 0, classData.length);
            }
            return super.loadClass(name);
        }

        private byte[] getClassData() {
            // 模拟加载类数据
            return new byte[]{/* class data */};
        }
    }
}
```
我们创建了一个自定义的类加载器CustomClassLoader，并使用它加载一个类MyClass。然后，我们清除对类加载器和类实例的引用，并请求垃圾回收。垃圾回收器在条件满足时会卸载MyClass类。
### 总结
类的卸载在 JVM 中是一个严格控制的过程，通常发生在以下情况下：

1. 类加载器被卸载。
2. 没有对类的实例的引用。
3. 没有对类的静态方法和静态字段的引用。
4. 没有对类加载器的引用。
5. 完成垃圾回收。
