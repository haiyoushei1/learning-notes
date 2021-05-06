SpringMVC的整个工作都建立在DispatcherServlet上，它是后端与Web连接的桥梁  
原本的Web.xml配置Servlet就是配它的映射路径和视图解析器，控制器，Tomcat通过Web.xml获得Servlet映射路径在进行管理  
不过通过WebApplication可以将整个Web.xml去掉

在servlet3.0以后容器如(Tomcat)会在类路径中寻找实现javax.servlet.ServletContainerInitializer接口的类 使容器可以不查找web.xml   
这个原理就是
```
Servlet3新规范:
Tomcat启动时会扫描classpath:META-INF.services.javax.servlet.ServletContainerInitializer文件  
里面写的接口的所有实现类,执行onStartup()方法
```

```java
public interface WebApplicationInitializer {
    void onStartup(ServletContext var1) throws ServletException;
}

```
Spring提供了ServletContainerInitializer的实现，SpringServletContainerInitializer
![](image-kkq9p7ab.png)
```java
里面只有一行内容:org.springframework.web.SpringServletContainerInitializer
```
  这个类功能查找实现WebApplicationInitializer的类来消除web.xml。  
Spring提供了WebApplicationInitializer的简单实现AbstractAnnotationConfigDispatcherServletInitializer
```java
public class SpittrWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {  // 设置业务逻辑，数据层配置文件？
        return new Class[] {RootConfig.class};  // 返回带有@Configuration注解用来配置ContextloaderListener的应用上下文bean
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {  // 设置控制器，页面解析器，处理器映射的配置文件
        return new Class[] {WebConfig.class};  // // 返回带有@Configuration注解用来配置DispatcherServlet的应用上下文bean
    }

    @Override
    protected String[] getServletMappings() {  // 配置DispatcherServlet的映射路径
        System.out.println("====sssss=====");
        return new String[] {"/"};
    }
}
```
SpittrWebAppInitializer继承自AbstractAnnotationConfigDispatcherServletInitializer，容器会自动发现它并且用它配置上下文  
通过重写方法，实现DispatcherServlet的映射路径