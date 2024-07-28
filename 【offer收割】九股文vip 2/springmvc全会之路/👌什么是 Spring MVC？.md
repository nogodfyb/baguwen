Spring MVC 是 Spring 框架中的一个模块，用于构建基于 Web 的应用程序。它遵循 Model-View-Controller（MVC）设计模式，将业务逻辑、用户界面和数据分离，以促进代码的可维护性和可扩展性。
### 1. **模型（Model）**
模型代表应用程序的数据和业务逻辑。它通常包含数据对象（如 POJO）和服务层（如 Spring 服务）来处理业务逻辑。模型负责从数据库或其他数据源获取数据，并将数据传递给视图以显示给用户。
### 2. **视图（View）**
视图负责展示数据，通常是 HTML 页面或其他类型的用户界面。Spring MVC 支持多种视图技术，包括 JSP、Thymeleaf、FreeMarker 等。视图从模型获取数据并将其呈现给用户。
### 3. **控制器（Controller）**
控制器处理用户请求并决定将数据传递给哪个视图。它接收用户输入，调用模型进行处理，并选择合适的视图来显示结果。控制器通常使用 `@Controller` 注解来标识，并使用 `@RequestMapping` 注解来映射 URL 请求。
### Spring MVC 的工作流程

1. **用户请求**：用户通过浏览器发送 HTTP 请求到服务器。
2. **前端控制器（DispatcherServlet）**：Spring MVC 的前端控制器 `DispatcherServlet` 拦截所有请求并进行分发。
3. **处理器映射（Handler Mapping）**：根据请求 URL，`DispatcherServlet` 查找相应的控制器。
4. **控制器处理**：控制器处理请求，调用服务层或数据访问层以获取数据，并将数据封装到模型中。
5. **视图解析器（View Resolver）**：控制器返回视图名称，`DispatcherServlet` 使用视图解析器将视图名称解析为实际的视图对象。
6. **视图渲染**：视图对象负责将模型数据渲染为用户界面，通常是 HTML 页面。
7. **响应返回**：渲染后的视图返回给 `DispatcherServlet`，`DispatcherServlet` 将最终的响应发送回用户浏览器。
### 核心组件

1. **DispatcherServlet**：前端控制器，负责接收并分发请求。
2. **Controller**：处理用户请求，包含业务逻辑。
3. **ModelAndView**：包含模型数据和视图名称的对象。
4. **View Resolver**：将视图名称解析为实际的视图对象。
5. **Handler Mapping**：根据请求 URL 查找相应的控制器。
