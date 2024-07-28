# 问题背景
我们有这样一个场景，先去查符合条件的商户 id，然后再根据商户 id 去查订单信息，采取的是 sql 的 in 的方式，来查询。<br />select * from order where shop_id in (xx,xx,xx);<br />类似上面的这个情况。in 中的数据比较大。在这个过程中外面业务层，通过ist.partition对数据进行拆分。<br />然后执行 sql。发现偶然会产生报错。
```
List<List<Long>> splitList = Lists.partition(shopIdList, 50);
for (List<Long> shopIds : splitList ) {
   List<PO> shopInfoList = mapper.query(shopIds);
}
```
其中的 sql 判断如下：
```
 <if test="shopIds != null and shopIds.size() > 0">
 </if>
```
报错信息
```
Error evaluating expression 'shopIds != null and shopIds.size() > 0'. 
 Cause: org.apache.ibatis.ognl.MethodFailedException: 
 Method "size" failed for object [123，456，789] 
 [java.lang.IllegalAccessException: Class org.apache.ibatis.ognl.OgnlRuntime can not access a member of class java.util.ArrayList$SubList with modifiers "public"]
```
# 具体分析
先给大家附上 github 的 bug 原地址～<br />[https://github.com/mybatis/mybatis-3/pull/384](https://github.com/mybatis/mybatis-3/pull/384)<br />1、报错信息分析<br />类org.apache.ibatis.ognl.OgnlRuntime不能访问类java.util的成员。ArrayList$SubList与修饰符"public"]<br />2、奇怪的点<br />既然无法访问对象subList，应当每次报错，而不是偶发性。<br />3、源码分析<br />myBatis版本3.2.3<br />![](https://cdn.nlark.com/yuque/0/2024/png/29413969/1718345710243-7305b682-05f4-4100-825c-852cc1a6a0f8.png#averageHue=%231f2024&clientId=u5746e257-10fd-4&from=paste&id=uc8ab2d75&originHeight=642&originWidth=1773&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=udd0cc2ea-7191-49f7-a0fa-fb5ac81e30d&title=)

subList 对象修饰符为private，size() 方法修饰符为public。<br />java.lang.reflect.Method.getModifiers()方法返回由此Method对象表示的方法的Java语言修饰符转为的整数值。<br />java.lang.reflect.AccessibleObject#isAccessible返回改对象的 accessible 修饰符<br />例如：<br />线程1执行完Modifier.isPublic(method.getModifiers()为true,setAccessible（true）, 暂停线程1<br />线程2执行 method.isAccessible()为false, setAccessible（false）<br />线程1继续执行method.invoke导致报错。

```
     if (syncInvoke) {
        synchronized(method) {
            method.setAccessible(true);
            try {
                result = method.invoke(target, argsArray);
            } finally {
                method.setAccessible(false);
            }
        }
    } else {
        result = method.invoke(target, argsArray);
    }
```
myBatis版本3.5.0新增synchronized(method)避免线程并发问题，解决这个问题。
# 总结
1、mybatis传入对象的方法和类修饰符不一致，会存在并发问题<br />2、解决方法：升级 mybatis 版本 or 使用hutool拆分数组方法 or 其他方法new ArrayList()，避免传入subList
