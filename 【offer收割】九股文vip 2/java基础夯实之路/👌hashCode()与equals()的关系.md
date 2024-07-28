在Java中，hashCode()和equals()方法密切相关，尤其在使用哈希表（如HashMap、HashSet等）时。
### hashCode()方法
hashCode()方法返回一个整数，称为哈希码。哈希码用于在哈希表中快速查找对象。Object类中的默认实现返回对象的内存地址转换成的整数。
### equals()方法
equals()方法用于比较两个对象的内容是否相同。Object类中的默认实现比较的是对象的引用。
### hashCode()与equals()的合同
为了确保哈希表中的对象行为正确，Java规范定义了hashCode()与equals()方法之间的合同（契约）：

1. **一致性**：
   - 如果两个对象根据equals()方法比较是相等的，那么它们的hashCode()方法必须返回相同的整数。
   - 如果两个对象根据equals()方法比较是不相等的，那么它们的hashCode()方法不一定要返回不同的整数，但不同的对象返回不同的哈希码可以提高哈希表的性能。
2. **稳定性**：
   - 在程序执行期间，hashCode()方法返回的整数值应该是一致的，只要对象的状态没有改变。也就是说，同一个对象多次调用hashCode()方法应该返回相同的值。
### 实现hashCode()与equals()方法的示例
假设有一个自定义类Person，我们需要重写equals()和hashCode()方法。
```
import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        Person person = (Person) obj;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```
### 分析

1. **equals()方法**：
   - 首先检查是否与自身比较（this == obj），如果是，返回true。
   - 然后检查传入的对象是否为空或类型是否匹配（obj == null || getClass() != obj.getClass()），如果不匹配，返回false。
   - 最后，比较类的字段是否相等（age == person.age && Objects.equals(name, person.name)）。
2. **hashCode()方法**：
   - 使用Objects.hash()方法生成哈希码，该方法根据传入的字段生成一个哈希码。
### 重要提示

- 重写equals()方法时，必须同时重写hashCode()方法，以确保两个相等的对象具有相同的哈希码。
- 如果只重写equals()方法而不重写hashCode()方法，可能会导致在哈希表中查找、插入和删除对象时出现问题。
