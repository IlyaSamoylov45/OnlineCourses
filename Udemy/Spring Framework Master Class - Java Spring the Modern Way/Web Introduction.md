Java Web Application
  - Three main files :
    - pom.xml
    - servlet
    - web.xml

POM
  - tomcat is run by a plugin.
   - Most of the work in maven is done using plugins; whereas dependency is just a Jar file which will be added to the classpath while executing the tasks.
   - For example, you use a compiler-plugin to compile the java files. You can't use compiler-plugin as a dependency since that will only add the plugin to the classpath, and will not trigger any compilation. The Jar files to be added to the classpath while compiling the file, will be specified as a dependency.
   - So, we can say, plugin is a Jar file which executes the task, and dependency is a Jar which provides the class files to execute the task.

Servlet
  - Simple Java class that takes input and give output.
  - Take HttpRequest, Output : HttpResponse.
  - Servelts must extend HttpServlet which is a JEE6 specification.
  - @WebServlet(urlPatterns = "/login")
    - Adds more info to servlet.
    - to handle get requests we must make a doGet method.

JSP - Jakarta Server Pages ; formerly JavaServer Pages
  - JSPs should be in WEB-INF folder in views.
  - HTML default is HTML 5
  - request.getRequestDispatcher("jsp location.jsp").forward(request, response);
  - To give the path of a jsp we start at the WEB-INF
  - request.getParameter("name");
    - get a query parameter from url
  - we can get those in the jsp with request attributes.
    - request.setAttribute("name", name)
    - To get the attribute in jsp use ${name}
  - Internally jsp's are turned into servlets.
  - You can write any java code in jsp but it's not recommended.
  - <% %> is how you can write in jsp using scriplets.
  - To write in  code we can do <%= %>
  - In jsp you import using page. Same importing rules are applied.
  - You should not have business logic in JSP!!!
  - Do not use scriplets in real world applications.
  - You should not use parameters for password with get.
    - You can create a form and put for better security on sensitive information
  - 405 means error not allowed

Spring MVC
  - spring-web provides core HTTP integration, including some handy Servlet filters, Spring HTTP Invoker, infrastructure to integrate with other web frameworks and HTTP technologies e.g. Hessian, Burlap.
  - spring-webmvc is an implementation of Spring MVC. spring-webmvc depends on on spring-web, thus including it will transitively add spring-web. You don't have to add spring-web explicitly.
  - Servlets are usually added in web.xml or Annotation @Controller is used.
  - pom.xml :
  ```xml
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.2.2.RELEASE</version>
  </dependency>
  ```
  - web.xml:

  ```xml
  <servlet>
      <servlet-name>dispatcher</servlet-name>
      <servlet-class>
          org.springframework.web.servlet.DispatcherServlet
      </servlet-class>
      <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>/WEB-INF/todo-servlet.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
      <servlet-name>dispatcher</servlet-name>
      <url-pattern>/spring-mvc/*</url-pattern>
  </servlet-mapping>
  ```
  - todo-servlet.xml

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.in28minutes" />

    <mvc:annotation-driven />

    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/WEB-INF/views/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>

  </beans>
  ```

  - @Controller tells spring to handle web requests.
  - @GetMapping("path")
  - @RequestMapping(value = "/login")
  - @ResponseBody
  - DispatcherServlet -> Front Controller
    - The job of the DispatcherServlet is to take an incoming URI and find the right combination of handlers (generally methods on Controller classes) and views (generally JSPs) that combine to form the page or resource that's supposed to be found at that location.
  - View Resolver
    - The two interfaces which are important to the way Spring handles views are ViewResolver and View. The ViewResolver provides a mapping between view names and actual views. The View interface addresses the preparation of the request and hands the request over to one of the view technologies.
  - The view is basically a jsp file. Or the screens.

Spring MVC Request Flow
  - DispatcherServlet receives HTTP Request.
  - DispatcherServlet identifies the right Controller based on the URL.
  - Controller executes Business Logic.
  - Controller returns a) Model b) View Name Back to DispatcherServlet.
  - DispatcherServlet identifies the correct view (ViewResolver).
  - DispatcherServlet makes the model available to view and executes it.
  - DispatcherServlet returns HTTP Response Back.
  - To restrict requests of a method we use method = RequestMethod.GET in @RequestMapping.
  - Can have different methods to respond to different Requests but same paths.
  - To get param from request we use @RequestParam. We should use the same name.
  - Essentially, a DispatcherServlet handles an incoming HttpRequest, delegates the request, and processes that request according to the configured HandlerAdapter interfaces that have been implemented within the Spring application along with accompanying annotations specifying handlers, controller endpoints, and response objects

Log4j
  - groupId : log4j
  - artifact log4j
  - Log4j is good for logging.
  - Can have log4j.properties in resources file.
  - Levels :
    - TRACE
    - DEBUG
    - INFO
    - WARN
    - ERROR

Spring MVC Model
  - To send param to jsp we use model.
  - A model is used to pass information between the model and the view.
  ```java
  public String handleLogin(@RequestParam String name, ModelMap model){
    model.put("name", name);
    return "welcome";
  }
  ```
  - Spring makes the ModelMap available to the dispatcher servlet.
