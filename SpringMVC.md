## SpringMVC

### 介绍

springMVC 基于MVC设计模式的轻量级的web框架，通过将modle、view、controller分离。将web层进行解耦

### springMVC核心组件

#### Dispatcher Servlet  前端控制器

作用：处理前端http请求和相应请求、响应结果，相当于转发器，有了DispatcherServlet 就减少了其它组件之间的耦合度。

#### Handler Mapping 处理器映射器

作用：根据请求的url查找Handler 处理器

#### HanderAdapter  处理器适配器

作用：执行对应的Handler 处理器

#### 视频解析器

作用：进行视图的解析，根据视图逻辑名解析成真正的视图

### SpringMVC工作流程

（1）用户发送请求至前端控制器DispatcherServlet；
（2） DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handle；
（3）处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet；
（4）DispatcherServlet 调用 HandlerAdapter处理器适配器；
（5）HandlerAdapter 经过适配调用 具体处理器(Handler，也叫后端控制器)；
（6）Handler执行完成返回ModelAndView；
（7）HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；
（8）DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析；
（9）ViewResolver解析后返回具体View；
（10）DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）
（11）DispatcherServlet响应用户。

### 常用注解

#### @Controller

#### @RequestMapping

#### @RestController

@Auto Attribute

@Request B    

