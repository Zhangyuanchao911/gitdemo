## SpringMVC

### 介绍

springMVC 基于MVC设计模式的轻量级的web框架，通过将modle、view、controller分离。将web层进行解耦

### springMVC主要的组件

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

![img](https://img-blog.csdnimg.cn/20200208211439106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RoaW5rV29u,size_16,color_FFFFFF,t_70)

### 常用注解

#### 注解原理

注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。

#### @Controller  控制器 

控制类要加上这个注解

#### @RequestMapping

用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

属性

value： 指定请求的实际地址；

method： 指定请求的method类型， GET、POST、PUT、DELETE等；

#### @RequestBody

接受前端传给后台的请求体内容  是以json格式传来的

由于GET请求无请求体 ，所以用此注解接受请求时，前端不能用get请求方法提交数据，而是使用POST请求

#### @ResponseBody

作用： 该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。

使用：返回的数据不是html标签的页面，而是其他某种格式的数据时（如json、xml等）使用；    

#### @RestController

相当于@ResponseBody ＋ @Controller合在一起的作用

#### @PathVariable和@RequestParam的区别

#### @PathVariable 

请求路径上有个id的变量值，可以通过@PathVariable来获取 @RequestMapping(value = “/page/{id}”, method = RequestMethod.GET)

#### @RequestParam

用来获得静态的URL请求入参 spring注解时action里用到。

例如：<form action="user.do?id=2113" method="post"></form>

#### @ModelAttribute

作用在方法上，会在控制器方法执行之前先执行。可以修饰没有返回值的方法，也可以修饰有返回值的方法（一般放在Map中返回）

应用：但表单提交数据不是完整的实体数据时，可以保证没有提交数据字段使用数据库对象原来的数据

### 文件的上传和下载

##### 1.导入相关的jar包依赖

common-io 和common-fileupload 

```jsp
<!--文件上传-->
<form method="post" action="textFileLoad" enctype="multipart/form-data">
    选择文件:<input type="file" name="upload" value="选择文件上传">
    <br>
    <input type="submit" value="提交">
</form>
```

##### 2.配置文件中配置文件解析器对象  

```xml
<!--    上传文件的配置-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="utf-8"/>
    <!-- 设置上传文件的大小上限，单位：byte，1mb=1024k 1k=1024b -->
    <!-- 设置10M大小 10M=10*1024*1024b=10485760b -->
    <property name="maxUploadSize" value="10485760"/>
</bean>
```

##### 3.Controller类编写文件上传和下载方法	

文件上传

```java
/**
 * 文件上传
 *
 * @param upload
 * @param request
 * @return
 * @throws IOException
 */
@RequestMapping(value = "/textFileLoad", method = RequestMethod.POST)
public ModelAndView textFileLoad(MultipartFile upload, HttpServletRequest request) throws IOException {
    ModelAndView modelAndView = new ModelAndView();
    //获取上传文件名
    String originalFilename = upload.getOriginalFilename();
    //获取文件后缀
    String suffixName = originalFilename.substring(originalFilename.lastIndexOf("."));
    //新文件名
    String newFileName = UUID.randomUUID().toString() + suffixName;
    //校验文件目录
    String uploadDir = request.getServletContext().getRealPath("/uploads");
    File file = new File(uploadDir);
    if (!file.exists()) {
        file.mkdir();
    }
    System.out.println(uploadDir);
    String suffixDir = uploadDir.substring(uploadDir.lastIndexOf("\\"));
    System.out.println(suffixDir);
    //保存文件到指定位置
    upload.transferTo(new File(uploadDir, newFileName));

    modelAndView.addObject("originalFilename", originalFilename);
    modelAndView.addObject("newFilename", newFileName);
    modelAndView.addObject("suffixDir", suffixDir);
    modelAndView.setViewName("success");
    return modelAndView;
}
```

文件下载

```java
/**
 * 文件下载
 * @param request
 * @param newImgName
 * @param originalImgName
 * @return
 * @throws IOException
 */
@RequestMapping(value = "download")
public ResponseEntity<byte[]> download(HttpServletRequest request, String newImgName, String originalImgName) throws IOException {
    //图片所在文件夹路径
    String path = request.getServletContext().getRealPath("/upload/");
    //图片路径
    String imgFullPath = path + newImgName;
    //将图片读入内存
    //        InputStream is = new FileInputStream(new File(imgFullPath));
    //        byte[] arr = new byte[1024 * 1024];
    //        is.read(arr);
    byte[] imgBody = FileUtils.readFileToByteArray(new File(imgFullPath));
    //下载显示的文件名，中文乱码
    String downloadImgName = new String(originalImgName.getBytes("UTF-8"), "iso-8859-1");
    //设置响应头
    HttpHeaders headers = new HttpHeaders();

    headers.setContentDispositionFormData("attachment", downloadImgName);//通知浏览器，以attachment（下载方式）打开图片
    headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
    ResponseEntity<byte[]> responseEntity = new ResponseEntity<byte[]>(imgBody, headers, HttpStatus.OK);
    return responseEntity;
}
```



### 解决POST和GET请求乱码的问题

#### 1.POST请求   

在web.xml中配置一个CharacterEncodingFilter过滤器，设置成utf-8；

```xml
<filter>

    <filter-name>CharacterEncodingFilter</filter-name>

    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

    <init-param>

        <param-name>encoding</param-name>

        <param-value>utf-8</param-value>

    </init-param>

</filter>

<filter-mapping>

    <filter-name>CharacterEncodingFilter</filter-name>

    <url-pattern>/*</url-pattern>

</filter-mapping>
```

#### 2.GET请求

①修改tomcat配置文件添加编码与工程编码一致，在tomcat目录下的servlet.xml修改默认的字符集

> <ConnectorURIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>

 ②另外一种方法对参数进行重新编码：

String userName = new String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")

ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码。

### SpringMvc拦截器

有两种写法,一种是实现HandlerInterceptor接口，另外一种是继承适配器类，接着在接口方法当中。

实现处理逻辑；然后在SpringMvc的配置文件中配置拦截器即可。

```java
public class MyInterceptor1 implements HandlerInterceptor {
    /**
     * 预处理  controller方法执行之前
     * return true 表示放行，执行下一个拦截器；如果没有执行controller的方法
     * return false 不放行，
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return false;
    }
}
```

```xml
 <!--    配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!-- 要拦截的方法-->
            <mvc:mapping path="com.xdd.controller/*"/>
            <!--配置拦截器对象-->
            <bean class="com.xdd.iterceptor.MyInterceptor1"/>
            <!--  不要拦截的方法-->
            <!-- <mvc:exclude-mapping path=""/>-->
        </mvc:interceptor>
    </mvc:interceptors>

```

### SpringMVC 异常处理

将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。

简单来说就是自定义一个类实现`HandlerExceptionResolver`接口并编写其中的方法`resolveException`

##### 1.代码分析

其主要就是自定义一个异常类和对应异常页面，核心则是`HandlerExceptionResolver`异常处理器，需要自定义一个类实现`HandlerExceptionResolver`接口并编写其中的方法`resolveException`，然后在`springmvc.xml`中配置。最后就是一些请求响应的业务逻辑以及页面的搭建了。

##### 2.在springmvc.xml中配置异常处理器

```xml
  <!--配置异常处理器-->
  <bean id="sysExceptionResolver" class="com.xdd.exception.SysExceptionResolver"/>
```

##### 3.编写自定义异常处理器

```java

  package com.Exception;

import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SysExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        // 获取到异常对象
        SysException sysException = null;
        if(e instanceof SysException){
            sysException = (SysException) e;
        }else{
            sysException = new SysException("系统正在维护升级ing....");
        }
        // 创建ModelAndView对象
        ModelAndView mv = new ModelAndView();
        //使用“errorMsg”存入提示信息
        mv.addObject("errorMsg",sysException.getMessage());
        mv.setViewName("error");
        return mv;
    }
}
```

##### 4.编写异常处理的Error.jsp页面