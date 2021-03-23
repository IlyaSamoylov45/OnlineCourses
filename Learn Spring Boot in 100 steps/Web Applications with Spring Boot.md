Project metadata
  - group : similar to package.
  - artifact : like the name of the class
  - dependencies : Things your project depends on.
  - version : version
  - packaging : How you want to package.

Advantages of SpringBoot:
  - No need of creating boilerplate configuration
  - Plenty of SpringBoot Starter to quickly get up and running
  - DevTools to autorestart server on code/config updates
  - Embedded Tomcat/Jetty/Undertow support
  - Easier customization of application properties
  - Easy management of profile specific properties
  - Easier dependency management using platform-bom

POM.xml
  - parent means it is the parent of the pom file.
    - Spring Boot starter parent brings in a ton of dependencies
    - Kind of like inheritance.
    - Has the default configuration for a lot of plugins.
    - We can override parent.
  - plugin helps us to run the application.
  - spring-boot-starter-web
    - Brings in tomcat.
    - We are using an embedded server. Now we don't need to install tomcat when we move this project into a linux computer or deploy.

SpringBootApplication
  - @SpringBootApplication initializes Spring (Component Scan) and Spring Boot ( Auto Configuration )
  - Automatically has @ComponentScan

application.properties
  - Used as config file.
  - By default spring logs at a logging level of INFO
    - logging.level.org.springframework.web=DEBUG

DevTools
  - is an important dependency for re initializing project without stopping. (Auto restarts).
  - Faster restart.

Spring Boot Controller:
  - @RequestMapping("/login")
  - @Controller -> Need to annotate for Spring to pick up.
  - @ResponseBody
    - Has message converters into JSON

JSP
  - Is a view, jsp renders it.
  - src/main/webapp/WEB-INF/jsp is where we want to store the jsp files.
  - In spring boot we need tomcat embed jasper dependency for jsp to work.

Dispatcher Servlet
  - The DispatcherServlet is the front controller in Spring web applications. It's used to create web applications and REST services in Spring MVC. In a traditional Spring web application, this servlet is defined in the web.xml file.
  - We can remove web.xml and use DispatcherServlet instead in Sping Boot application.
  - Before the Servlet 3.x specification, DispatcherServlet would be registered in the web.xml file for a Spring MVC application. Since the Servlet 3.x specification, we can register servlets programmatically using ServletContainerInitializer.
  - Spring Boot provides the spring-boot-starter-web library for developing web applications using Spring MVC. One of the main features of Spring Boot is autoconfiguration. The Spring Boot autoconfiguration registers and configures the DispatcherServlet automatically. Therefore, we don’t need to register the DispatcherServlet manually.
  - By default, the spring-boot-starter-web starter configures DispatcherServlet to the URL pattern “/”. So, we don't need to complete any additional configuration for the above DispatcherServlet example in the web.xml file. However, we can customize the URL pattern using server.servlet.* in the application.properties file.
  - Spring :
  ```xml
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
  </servlet>

  <servlet-mapping>
      <servlet-name>dispatcher</servlet-name>
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```
  - Spring Boot :
  ```yaml
  server.servlet.context-path=/demo
  spring.mvc.servlet.path=/location
  ```
    - With these customizations, DispatcherServlet is configured to handle the URL pattern /location and the root contextPath will be /demo. Thus, DispatcherServlet listens at http://localhost:8080/demo/location/.
  - Need to tell the dispatcher where to find the jsp to return and that job is designated to the view resolver.

Model Map
  - @RequestParam binds a reqest  param.
  - Model is used to pass data from controller to the view (jsp)
  - Add parameter ModelMap model
    - model.put("name", name)
    - Can be accessed with expression language ${ }

MVC
  - Model View Controller

@Controller vs @RestController
  - @Controller is used to mark classes as Spring MVC Controller.
  - @RestController is a convenience annotation that does nothing more than adding the @Controller and @ResponseBody annotations (see: Javadoc)
  - So the following two controller definitions should do the same
  ```java
  @Controller
  @ResponseBody
  public class MyController { }

  @RestController
  public class MyRestController { }
  ```
  - If you use @RestController you cannot return a view (By using Viewresolver in Spring/springboot) and yes @ResponseBody is not needed in this case.
  - If you use @Controller you can return a view in Spring web MVC.

Architecture of Web Applications
  - Web
    - jsp and views.
  - Integration
    - Integrate with other systems.
  - Business
    - Does all business logic.
    - Talks to integration and data layer
  - Data
    - Database communication
  - Front Controller  
    - Handles Data Binding, Handler Mapping, View Resolvers and a lot of other functionality.
    - Evolution of model 2 architecture.
    - This is a dispatcher servlet.
    - All data goes through Front controller in a model 2 architecture.

Session vs Model vs Request.
  - Spring documentation states that the @SessionAttributes annotation “list the names of model attributes which should be transparently stored in the session or some conversational storage.”
  - @RequestParam is not avilable for subsequent requests.
  - @SessionAttributes("name")
    - Under @Controller
  - Why use Model? "adding elements directly to the HttpServletRequest (as request attributes) would seem to serve the same purpose. The reason to do this is obvious when taking a look at one of the requirements we have set for the MVC framework: It should be as view-agnostic as possible, which means we’d like to be able to incorporate view technologies not bound to the HttpServletRequest as well."
  - Session is the way to store values across the user session.
  - Session should be as small as possible.
  - Can use <a href="/login"> can be picked up by the controller !
  - return 'redirect:/login'

JSTL Tags
  - Java standard tag Library:
    ```xml
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
    </dependency>
    ```
    ```html
    <table class="table table-striped">
			<caption>Your todos are</caption>
			<thead>
				<tr>
					<th>Description</th>
					<th>Target Date</th>
					<th>Is it Done?</th>
				</tr>
			</thead>
			<tbody>
				<c:forEach items="${todos}" var="todo">
					<tr>
						<td>${todo.desc}</td>
						<td>${todo.targetDate}</td>
						<td>${todo.done}</td>
					</tr>
				</c:forEach>
			</tbody>
		</table>
    ```
  - JavaServer Pages Tag Library (JSTL) is a set of tags that can be used for implementing some common operations such as looping, conditional formatting, and others.

BootStrap
  - We can add bootstrap and jquery into dependencies:
  ```xml
  <dependency>
    <groupId>org.webjars</groupId>
    <artifactId>bootstrap</artifactId>
    <version>3.3.6</version>
  </dependency>
  <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>1.9.1</version>
  </dependency>
  ```

Command Bean
  - Command bean or Form Backing Bean allows use to map directly to an object.
  - form:form for using form library.
  - Better to do validation on server side.
  - Spring has its own form library. We need to included it on jsp.
    - The major use of spring:form tag is formbacking object . If you wish to bind the model attribute object with the view fields
    - Unlike other form/input tag libraries, Spring's form tag library is integrated with Spring Web MVC, giving the tags access to the command object and reference data your controller deals with. The form tags make JSPs easier to develop, read and maintain.

Validation
  - We can add validation using bean validation Applications.
    - @Size(min=10, message="Enter at least 10 Characters.")
    - You add this to a parameter in a class.
    - Add @Valid in the controller to enable this validation (added to parameter).
  - To ensure the validation really succeeded we would use something known as binding result.
    - Whenever we add @Valid another bean named BindingResult is populated.
    ```java
    @RequestMapping(value = "/add-todo", method = RequestMethod.POST)
	  public String addTodo(ModelMap model, @Valid Todo todo, BindingResult result) {

  		if(result.hasErrors()){
  			return "todo";
  		}

  		service.addTodo((String) model.get("name"), todo.getDesc(), new Date(),
  				false);
  		return "redirect:/list-todos";
  	}
    ```

    ```html
    <html>

    <head>
      <title>First Web Application</title>
      <link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
    	rel="stylesheet">
    </head>

    <body>
    	<div class="container">
    		<form:form method="post" commandName="todo">
    			<fieldset class="form-group">
    				<form:label path="desc">Description</form:label>
    				<form:input path="desc" type="text"
    					class="form-control" required="required"/>
    				<form:errors path="desc" cssClass="text-warning"/>
    			</fieldset>

    			<button type="submit" class="btn btn-success">Add</button>
    		</form:form>
    	</div>

    </body>

    </html>
    ```

InitBinder  
  - You can create a method using @InitBinder to set the date format across the controller
  ```java
  @InitBinder
	public void initBinder(WebDataBinder binder) {
		// Date - dd/MM/yyyy
		SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
		binder.registerCustomEditor(Date.class, new CustomDateEditor(
				dateFormat, false));
	}
  ```
  - in jsp :
  ```xml
  <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>

	<fmt:formatDate pattern="dd/MM/yyyy" value="${todo.targetDate}" />

  <script>
		$('#targetDate').datepicker({
			format : 'dd/mm/yyyy'
		});
	</script>
  ```

JSP Fragments and Navigation Bar
  - You can add jsp snippets to move similar code into jspf files and then include that in the jsp files.
  - This removes a lot of duplicate code.

Spring Security
  - Basic authentication for users.
  ```java
  @Configuration
  public class SecurityConfiguration extends WebSecurityConfigurerAdapter{
  	//Create User - in28Minutes/dummy
  	@Autowired
      public void configureGlobalSecurity(AuthenticationManagerBuilder auth)
              throws Exception {
          auth.inMemoryAuthentication()
              .passwordEncoder(NoOpPasswordEncoder.getInstance())
          		.withUser("in28Minutes").password("dummy")
                  .roles("USER", "ADMIN");
      }

  	@Override
      protected void configure(HttpSecurity http) throws Exception {
          http.authorizeRequests().antMatchers("/login", "/h2-console/**").permitAll()
                  .antMatchers("/", "/*todo*/**").access("hasRole('USER')").and()
                  .formLogin();

          http.csrf().disable();
          http.headers().frameOptions().disable();
      }
  }
  ```
  ```java
  private String getLoggedInUserName(ModelMap model) {
  Object principal = SecurityContextHolder.getContext()
      .getAuthentication().getPrincipal();

  if (principal instanceof UserDetails)
    return ((UserDetails) principal).getUsername();

  return principal.toString();
  }

  @RequestMapping(value = "/logout", method = RequestMethod.GET)
  public String logout(HttpServletRequest request,
      HttpServletResponse response) {
    Authentication auth = SecurityContextHolder.getContext()
        .getAuthentication();
    if (auth != null) {
      new SecurityContextLogoutHandler().logout(request, response, auth);
    }
    return "redirect:/";
  }

  ```
  ```xml
  <ul class="nav navbar-nav navbar-right">
    <li><a href="/logout">Logout</a></li>
  </ul>
  ```

Exception
  - @ExceptionHandler in Controller class can throw custom exception pages.
