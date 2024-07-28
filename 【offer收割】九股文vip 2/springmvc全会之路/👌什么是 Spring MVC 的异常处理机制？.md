### 一、异常处理机制概述
在 Spring MVC 中，无论是 DAO 层、Service 层还是 Controller 层，都有可能抛出异常。这些异常可以通过 `throws Exception` 向上抛出，并最终由 Spring MVC 的前端控制器（DispatcherServlet）交给异常处理器进行处理。Spring MVC 提供了多种异常处理机制，以便开发者能够根据自己的需求选择最适合的方式。
### 二、异常处理的主要方式
#### 使用 SimpleMappingExceptionResolver

- 简介：SimpleMappingExceptionResolver 是 Spring MVC 提供的一个简单异常处理器，它实现了 HandlerExceptionResolver 接口。通过配置该处理器，开发者可以定义异常类型与视图之间的映射关系，从而指定当特定类型的异常发生时应该渲染哪个视图。

- 配置方式：在 Spring MVC 的配置文件中（如 XML 配置文件或 Java 配置类）配置 SimpleMappingExceptionResolver，并设置其属性，如 `defaultErrorView`（默认错误视图）、`exceptionMappings`（异常类型与视图的映射关系）等。
#### 实现 HandlerExceptionResolver 接口

- 简介：如果 SimpleMappingExceptionResolver 不能满足需求，开发者可以实现 HandlerExceptionResolver 接口来创建自定义的异常处理器。这种方式提供了更高的灵活性，允许开发者在异常处理过程中执行更复杂的逻辑。

- 实现方式：创建一个类实现 HandlerExceptionResolver 接口，并重写 `resolveException` 方法。在该方法中，可以根据异常类型决定如何处理异常，并返回一个 ModelAndView 对象，该对象指定了要渲染的视图和要传递给视图的模型数据。
#### 使用 @ControllerAdvice + @ExceptionHandler

- 简介：从 Spring 3.2 开始，Spring MVC 引入了 `@ControllerAdvice` 和 `@ExceptionHandler` 注解，提供了一种基于注解的异常处理机制。这种方式允许开发者将异常处理逻辑与具体的 Controller 分离，从而实现更加模块化和可重用的异常处理代码。

- 实现方式：首先，使用 `@ControllerAdvice` 注解标记一个类，该类将作为全局的异常处理类。然后，在该类中使用 `@ExceptionHandler` 注解标记方法，并指定该方法用于处理哪种类型的异常。当指定的异常发生时，Spring MVC 将自动调用该方法进行处理。
### 三、异常处理机制的优势

- 统一处理：能够将所有类型的异常处理从各处理过程解耦出来，既保证了相关处理过程的功能较单一，也实现了异常信息的统一处理和维护。
- 灵活性：提供了多种异常处理方式，允许开发者根据自己的需求选择最适合的方式。
- 用户体验：通过友好的错误页面或错误信息提示，可以提升用户体验。
