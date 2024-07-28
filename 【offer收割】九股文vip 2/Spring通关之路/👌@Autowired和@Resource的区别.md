@Autowired和@Resource都是用于依赖注入的注解。
### @Autowired

- **来源**：Spring 框架。
- **注入方式**：默认按类型注入。
- **用法**：可以用于字段、构造器、Setter 方法或其他任意方法。
- **可选性**：可以与@Qualifier一起使用，以指定具体的 Bean。
- **处理机制**：Spring 的AutowiredAnnotationBeanPostProcessor处理@Autowired注解。
### @Resource

- **来源**：由 Java EE 提供。
- **注入方式**：默认按名称注入，如果按名称找不到，则按类型注入。
- **用法**：可以用于字段或 Setter 方法。
- **属性**：可以指定name和type属性。
- **处理机制**：Spring 的CommonAnnotationBeanPostProcessor处理@Resource注解。
### 详细比较

1. **注入方式**：
   - @Autowired：默认按类型注入。如果需要按名称注入，可以结合@Qualifier注解使用。
   - @Resource：默认按名称注入。如果名称匹配失败，则按类型注入。
2. **属性**：
   - @Autowired：没有name和type属性，但可以使用@Qualifier指定名称。
   - @Resource：有name和type属性，可以明确指定要注入的 Bean 名称或类型。
3. **兼容性**：
   - @Autowired：是 Spring 框架特有的注解。
   - @Resource：是 Java 标准注解，兼容性更广，适用于任何支持 JSR-250 的容器。
4. **使用场景**：
   - @Autowired：在 Spring 应用中更为常见，尤其是在需要按类型注入的场景中。
   - @Resource：在需要兼容标准 Java EE 规范的应用中更为常见，或者在需要明确指定 Bean 名称时使用。
