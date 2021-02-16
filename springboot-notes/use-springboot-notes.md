### 一、springboot项目的构建
在new project 创建springboot工程
![](image-kl7tjsup.png)
选择web项目
![](image-kl7tl0ia.png)
就ok了

在pom.xml下添加
```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>springboot_test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot_test</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    <!--连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
    <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.22</version>
        </dependency>
    <!--引入mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.18</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
        </dependency>
        <!-- spring boot devtools 依赖包. -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
            <scope>true</scope>
        </dependency>


    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <!--热部署配置-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <!--fork :  如果没有该项配置，肯定devtools不会起作用，即应用不会restart -->
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

这个包括了springboot启动器，测试，lombok等  
刷新下就有了  
目录结构为  
![](image-kl7ty5f7.png)

当然，advice这些包是后加的，只有SpringTestApplication类。

###  二、springboot进行MVC开发
springboot是对spring，springmvc的整合，并且内置tomcat就不需要我们再打web包，交给tomcat启动。直接@SpringBootApplication的启动类就可以启动tomcat。  
在spring中我们可以使用xml进行ssm整合开发。springboot则砍掉了xml配bean的过程，直接使用注解开发，当然mybatis的接口捆绑还是要mapper.xml的。  
1、储存数据的类  
与spring一模一样，没什么可说的  
2、整合mybatis  
和spring注解整合mybatis差不多。在spring注解整合mybatis需要配置DataSource，指定mapper.xml的路径。这些东西在```application.properties```都配置了
```
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/testmvc?serverTimezone=GMT
spring.datasource.username=root
spring.datasource.password=*******

spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

mybatis.type-aliases-package=com.example.springboot_test.dto
mybatis.mapper-locations=classpath:/mappers/*.xml
```
这个里面还是要指定mybatis.mapper-locations的路径，不然还是绑不上

3、业务逻辑层  
这个也没什么好说的，就是@Service注解，@AutoWired自动注入mapper实现类

4、异常处理  
springboot提供了五种异常处理的方案  
(1)、在resources的templates文件夹下创建error.html，这样所有的异常都会默认跳到这个异常处理的页面文件。  
(2)、在控制类中使用@ExceptionHandler注解注释异常处理方法，当控制类的方法出现异常时就会跳到对应的异常处理方法。
```java
    @ExceptionHandler(value = {ArithmeticException.class})
    public ModelAndView arithmeticExceptionHandler(Exception e){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("error", e.toString());
        modelAndView.setViewName("error1");
        return modelAndView;
    }

```
上面的代码处理了ArithmeticException异常出现时，会返回一个ModelAndView类。  
(3)、使用@ControllerAdvice+@ExceptionHandler注解配置异常处理类
```java
@ControllerAdvice
public class ServiceExceptionHandler {

    @ExceptionHandler(value = {ArithmeticException.class})
    public ModelAndView arithmeticExceptionHandler(Exception e){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("error", e.toString());
        modelAndView.setViewName("error1");
        return modelAndView;
    }
}
```
和方法2差不多，只不过这种形式就直接把各种异常的处理都放在一个类里，解耦合  
(4)、实现HandlerExceptionResolver接口
```java
public class HandlerExceptionResolverImpl implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        if(e instanceof ArithmeticException)
            modelAndView.setViewName("error1");
        modelAndView.addObject("error", e.toString());
        return modelAndView;
    }
}

```
这种多了Servlet的请求与相应的类，更自由。  
(5)、配置SimpleMappingExceptionResolve类进行异常处理(这个我觉得不是很好用)  

5、Controller  
与springmvc的注解开发差不多，通过@Controller注解标识控制bean，以及@RequestMapper，@ResponseBody，处理请求，完成简单的处理访问，以及在返回处使用"redirect" "forword"实现重定向与转发功能
```java
@Controller
public class HelloWorldController {
    @RequestMapping("/hello")
    @ResponseBody
    public Map<String, Integer> helloWorld(){
        Map<String, Integer> map = new HashMap<>();
        map.put("Hello", 123);
        return map;
        // return "redirect:*******"
    }
}
```

6、视图层  
这个我暂时还没学

7、热部署
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
    <scope>true</scope>
</dependency>
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
    	<!--fork :  如果没有该项配置，肯定devtools不会起作用，即应用不会restart -->
        <fork>true</fork>
    </configuration>
</plugin>
```
![](image-kl7zi8pg.png)

将Build project automatically勾选上   
再ctrl+shift+alt+/选择Registry
![](image-kl7zkh0j.png)

