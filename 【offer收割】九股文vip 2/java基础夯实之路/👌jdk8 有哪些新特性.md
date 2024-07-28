### Lambda表达式
提供了一种简洁的方式来表示匿名方法（即没有名称的方法）。Lambda 表达式可以用来创建仅有一个抽象方法的接口的实例，这种接口被称为函数式接口。<br />优点：<br />简化代码：Lambda表达式允许我们以更简洁的方式表示匿名函数，减少了代码的冗余和复杂性。<br />函数式编程风格：支持函数式编程，使得代码更加易于理解和维护。<br />提高可读性：通过减少样板代码，提高了代码的可读性和可维护性。
### Stream API
Stream API 是对集合操作的一种高级抽象。这个 API 可以看作迭代器（Iterator）的一个高级版本，它允许你以声明性的方式处理数据集合。Stream API 支持顺序和并行两种模式的数据处理。<br />Stream 操作主要分为两种：

- 中间操作：这些操作都是惰性化的，意味着它们不会立即执行，只有在遇到终端操作时才会执行。中间操作会返回一个新的 Stream，允许多个中间操作可以链接起来。常见的中间操作包括 filter, map, flatMap, distinct, sorted, peek, limit, skip 等。
- 终端操作：这些操作会从 Stream 产生结果或副作用。当执行终端操作时，Stream 操作实际上会被执行，并且之后不能再被使用。常见的终端操作包括 forEach, forEachOrdered, toArray, reduce, collect, min, max, count, anyMatch, allMatch, noneMatch, findFirst, findAny 等。

优点：<br />声明式编程：Stream API提供了声明式编程风格，使得对集合的处理更加直观和简洁。<br />并行处理：支持并行处理，可以充分利用多核CPU的性能，提高程序的执行效率。<br />高效性：通过内部迭代和懒加载，Stream API在处理大量数据时具有更高的效率。
```
List<String> myList = Arrays.asList("apple", "banana", "cherry", "date");

// 使用 Stream API 进行操作
List<String> filteredList = myList.stream()       // 将集合转换为 Stream
.filter(s -> s.startsWith("a"))              // 中间操作：过滤出以字母 "a" 开头的字符串
.map(String::toUpperCase)                     // 中间操作：将字符串转换为大写
.sorted()                                     // 中间操作：排序
.collect(Collectors.toList());                // 终端操作：转换回列表

System.out.println(filteredList); // 输出: [APPLE]
```
### 函数式接口
函数式接口是只包含一个抽象方法的接口，它可以包含多个默认方法或静态方法。Java 8 之前，我们通常使用匿名内部类来实现这样的接口。但是，使用 Lambda 表达式，我们可以以更简洁、更易读的方式来实现它们。<br />Java 8 在 java.util.function 包下提供了一些内置函数式接口，以支持常见的函数式编程任务，包括：
```
Predicate<T>：接受一个输入参数，返回一个布尔值结果。
Consumer<T>：接受一个输入参数，无返回结果。
Function<T,R>：接受一个输入参数，返回一个结果。
Supplier<T>：无参数，返回一个结果。
UnaryOperator<T>：接受一个参数为类型T，返回值类型也为T。
BinaryOperator<T>：接受两个同类型的参数，返回一个同类型的结果。
BiPredicate<L,R>：接受两个输入参数，返回一个布尔值结果。
BiConsumer<T,U>：接受两个输入参数，无返回结果。
BiFunction<T,U,R>：接受两个输入参数，返回一个结果。
```
```
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
// 使用 Predicate 函数式接口过滤名字列表
List<String> filteredNames = names.stream()
.filter(name -> name.startsWith("A"))
.collect(Collectors.toList());
System.out.println(filteredNames); // 输出: [Alice]
```
### 默认方法和静态方法
默认方法是接口中带有默认实现的方法，使用 default 关键字修饰。默认方法允许在不破坏现有实现的情况下，向接口添加新的方法。<br />这解决了之前版本中的一个主要问题：一旦一个接口发布，如果要添加方法，就必须修改所有实现了该接口的类。通过默认方法，可以在接口中指定方法的默认实现，实现类可以选择覆盖这个默认实现，也可以使用它。
```
blic interface MyInterface {
  // 默认方法
  default void defaultMethod() {
    System.out.println("This is a default method.");
  }
}
```
### 静态方法
接口的静态方法与类的静态方法类似，使用 static 关键字定义在接口内部。静态方法可以直接通过接口名调用，不能被接口的实现类继承或覆盖。在接口中添加静态方法的好处是可以为接口提供工具方法，而不需要实现该接口。
```
public interface MyInterface {
  // 静态方法
  static void staticMethod() {
    System.out.println("This is a static method.");
  }
}
```
### 方法引用
简化Lambda表达式：方法引用提供了一种更简洁的方式来引用已有方法，减少了Lambda表达式的复杂性。<br />提高可读性：方法引用使得代码更加易于阅读和理解。<br />减少错误：通过直接引用方法名，减少了因手动编写Lambda表达式而可能引入的错误。
```
List<String> strings = Arrays.asList("hello", "world"); 
strings.forEach(String::toUpperCase); // 使用方法引用打印大写字符串
```
### 新的日期和时间API
新的日期和时间API提供了更加直观和易用的类和方法，使得日期和时间的处理更加简单。与旧的Date和Calendar类相比，新的API是线程安全的。提供了更多的功能，如时区处理、时间间隔计算等。
```
LocalDate date = LocalDate.now();
LocalTime time = LocalTime.now();
LocalDateTime dateTime = LocalDateTime.now();
```
### Optional类
Optional类提供了一种更好的方式来处理可能为null的值，避免了空指针异常的发生。使用Optional类可以使代码更加清晰和易于理解，减少了因处理null值而引入的复杂性。Optional类提供了一系列的方法来处理值的存在性和缺失性，使得代码更加易于阅读和维护。
```
Optional<String> optional = Optional.of("Hello");
optional.ifPresent(System.out::println);
```
### 并发增强
CompletableFuture 是 Java 8 引入的一个类，位于 java.util.concurrent 包中。它提供了一种异步编程的框架，允许你以声明式的方式编写复杂的异步逻辑。CompletableFuture 使得编写非阻塞的异步代码变得更加简单，支持以链式调用的方式组织任务流程，以及提供了丰富的工具来处理错误、合并结果等。<br />主要功能

- 异步执行任务：可以非常容易地启动一个异步任务，并立即返回一个 CompletableFuture 对象，不会阻塞调用者的线程。
- 结果转换和消费：可以对异步操作的结果进行转换或者直接消费。
- 组合异步操作：可以将多个 CompletableFuture 组合起来，无论是等待所有任务完成，还是只要有一个完成就继续执行。
- 处理错误：提供了异常处理的能力，允许你捕获异常并对其进行处理。
```
异步执行任务
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
// 模拟执行耗时任务
try { TimeUnit.SECONDS.sleep(1); } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
// 返回结果
return "Hello";
});

结果转换
CompletableFuture<String> futureWithTransformation = future.thenApply(result -> result + " World");

结果消费
future.thenAccept(System.out::println);

组合异步操作
等待所有任务完成：
CompletableFuture<Void> allFutures = CompletableFuture.allOf(future1, future2, future3);

任意一个完成即继续：
CompletableFuture<Object> anyOfFuture = CompletableFuture.anyOf(future1, future2, future3);

处理错误
CompletableFuture<String> futureWithErrorHandling = future.exceptionally(ex -> "Error: " + ex.getMessage());

手动完成（complete）
CompletableFuture<String> manualFuture = new CompletableFuture<>();

// 在未来某个时间点完成此 Future
manualFuture.complete("Manual result");

实际应用示例
CompletableFuture.supplyAsync(() -> {
// 模拟执行耗时任务
try { TimeUnit.SECONDS.sleep(1); } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
return "Hello";
}).thenApply(result -> {
// 对结果进行转换
return result + " World";
}).thenAccept(System.out::println); // 消费最终的结果

```


