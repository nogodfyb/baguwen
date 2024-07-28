在 Java 中，类加载器（ClassLoader）是负责将类文件加载到 JVM 中的组件。实现自定义类加载器可以让你控制类加载的过程，例如从非标准位置加载类文件、解密类文件等。
### 实现自定义类加载器的步骤

1. **继承ClassLoader类**：
   - 自定义类加载器需要继承java.lang.ClassLoader类。
2. **重写findClass方法**：
   - 重写findClass(String name)方法，这是自定义类加载器的核心方法，用于定义类的加载逻辑。
3. **调用defineClass方法**：
   - 在findClass方法中，通过defineClass方法将字节数组转换为Class对象。
### 示例：从文件系统加载类
下面是一个简单的示例，展示如何从文件系统加载类文件。
#### 1. 创建自定义类加载器
```
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class MyClassLoader extends ClassLoader {

    private String classPath;

    public MyClassLoader(String classPath) {
        this.classPath = classPath;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        try {
            // 将类名转换为文件路径
            String fileName = classPath + name.replace('.', '/') + ".class";
            // 读取类文件的字节码
            byte[] classBytes = Files.readAllBytes(Paths.get(fileName));
            // 将字节码转换为 Class 对象
            return defineClass(name, classBytes, 0, classBytes.length);
        } catch (IOException e) {
            throw new ClassNotFoundException(name, e);
        }
    }
}
```
#### 2. 使用自定义类加载器加载类
```
public class CustomClassLoaderDemo {
    public static void main(String[] args) {
        try {
            // 创建自定义类加载器，指定类文件所在路径
            MyClassLoader classLoader = new MyClassLoader("/path/to/classes/");
            // 加载类
            Class<?> clazz = classLoader.loadClass("com.example.MyClass");
            // 创建类的实例
            Object instance = clazz.getDeclaredConstructor().newInstance();
            // 调用方法
            clazz.getMethod("myMethod").invoke(instance);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
### 示例：从网络加载类
下面是一个从网络加载类文件的示例。
#### 1. 创建自定义类加载器
```
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.net.URL;

public class NetworkClassLoader extends ClassLoader {

    private String baseUrl;

    public NetworkClassLoader(String baseUrl) {
        this.baseUrl = baseUrl;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        try {
            // 将类名转换为 URL
            String url = baseUrl + name.replace('.', '/') + ".class";
            // 从网络读取类文件的字节码
            InputStream inputStream = new URL(url).openStream();
            ByteArrayOutputStream byteStream = new ByteArrayOutputStream();
            int nextValue = 0;
            while ((nextValue = inputStream.read()) != -1) {
                byteStream.write(nextValue);
            }
            byte[] classBytes = byteStream.toByteArray();
            // 将字节码转换为 Class 对象
            return defineClass(name, classBytes, 0, classBytes.length);
        } catch (IOException e) {
            throw new ClassNotFoundException(name, e);
        }
    }
}
```
#### 2. 使用自定义类加载器加载类
```
public class NetworkClassLoaderDemo {
    public static void main(String[] args) {
        try {
            // 创建自定义类加载器，指定类文件所在的基 URL
            NetworkClassLoader classLoader = new NetworkClassLoader("http://example.com/classes/");
            // 加载类
            Class<?> clazz = classLoader.loadClass("com.example.MyClass");
            // 创建类的实例
            Object instance = clazz.getDeclaredConstructor().newInstance();
            // 调用方法
            clazz.getMethod("myMethod").invoke(instance);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
### 总结
自定义类加载器可以让你控制类的加载过程，实现从非标准位置加载类文件的需求。通过继承ClassLoader并重写findClass方法，你可以实现自己的类加载逻辑。
