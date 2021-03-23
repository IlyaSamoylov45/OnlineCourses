Using Spring Framework to Manage Dependencies
  - Need to Spring tell :
    - What are the beans
    - What are the dependencies of a bean.
    - Where to search for beans?
  - You tell spring it is a bean by adding component annotation, afterwards it is managed by spring.
    - In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.
    - Simply put, Inversion of Control, or IoC for short, is a process in which an object defines its dependencies without creating them. This object delegates the job of constructing such dependencies to an IoC container.
  - In a component you need to tell spring something is a dependency.
    - You can do this with @Autowired
  - We can search for beans using component scan. Spring boot automatically does this.
  - All beans are managed by Application context.
    - This maintains all beans.
    - Need to get beans from Application context.

```java
ApplicationContext appContext = SpringApplication.run(SpringIn5StepsApplication); // This return the application context.

BinarSearchImpl binarySearch = appContext.getBean(BinarySearchImpl.class);


int res = binarySearch.binarySearch(array, value);
```

What happens in the background?
  - In application.properties : logging.level.org.springframework = debug
  - Spring starts by running a component scan.
  - Searches for all the component classes in the package specified.
  - Once spring identifies candidate classes.
    - Creates instances of the beans depending on dependencies.
    - Finishes creating instances that have no dependencies.
    - Autowirings by type via constructor.
  - Difference between constructor injection and setter injection.

Dynamic autowiring and troubleshooting
  - @Primary can be used if you have more than one component matching a specific type.
  - @Primary indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency.
  - @Component followed by @Primary.

Constructor and Setter Injection
  - Can use a setter injector to initialize the bean.
  - Three possibilities:
    - Constructor
    - Setter
    - No Setter or Constructor
  - Setter injection and not using anything is the same.
  - Before it was suggested that all mandatory dependencies should be autowired using a constructor.
  - If a class can run without a dependency then those are optional dependencies.
  - So if the class had mandatory dependencies then the recommendation was to use constructor injection.
  - For all other dependencies was to use setter injection.
  - Today there is not a big difference between optional and mandatory dependencies.
  - Setter injection kind of hides those dependencies when you autowire 5, 10, 15 things.
  - The biggest drawback of this approach of not using a constructor for dependency injection is that you can add a lot of dependencies, so it is easy to inject 5, 10, 15 for a specific thing.

Spring Modules
  - Spring is not one big framework.
  - Spring is built in a modular way which enables you to use specific modules without using the other modules of spring.
  - The core container modules are beans, core, context and SpEL
  - Database layer is the layer that talks to a database to retrieve data.
  - Has good JDBC and ORM integration.
  - Good integration with JMS (queue)
  - Spring OXM provides many features (Object to XML).
  - Spring has good transaction management (For database stuff).
  - Good connections with web frameworks like Struts.
  - Things that are applicable to more than one layer are considered cross cutting concerns.
    - Unit testing is an example of cross-cutting concerns because you want to be able to test things in the business, web and data layer.
    - The concern is the behavior we want to have in a particular module of an application. It can be defined as a functionality we want to implement.
    - The cross-cutting concern is a concern which is applicable throughout the application. This affects the entire application. For example, logging, security and data transfer are the concerns needed in almost every module of an application, thus they are the cross-cutting concerns.

Spring Projects
  - There are other things Spring does other then the Spring Framework and it's modules. These are called Spring Projects.
  - Spring Projects provide solutions for different problems faced by enterprise applications.
  - Some Spring projects :
    - Spring Boot
      - Quickly build applications.
    - Spring Cloud
      - Build cloud native apps
    - Spring Data
      - Consistent Data access
    - Spring Integration
      - Application Integration
    - Spring Batch
      - Batch applications
    - Spring Security
      - Protect your applications
    - Spring HATEOAS
      - HATEOAS compatible services.
    - Others

Why Spring is popular :
  - Enables testable code.
  - No plumbing code.
  - Flexible architecture.
    - If I use spring in my project my options are not restricted to choose other frameworks.
  - Staying current.

Dependency :
  - We develop applications in layers.
  - You would have separate layers each having functionality of its own.
  - The Web layer is typically concerned with how you are interfacing with the external world. If it is a web application then it will deal with how do you take the business logic and expose it to the UI. Or if it is a web service then it will deal with  how to convert the data that is coming from the business side into JSON or XML etc, which it can send to the outside world.
  - The business layer is where all your business logic is present.
  - Data layer is concerned with how to take the data and store it into the database. Business layer relies on the data layer.  
  - Anything that is present in the business layer is typically dependent on the Data layer.
  - enterprise applications are filled with dependencies.

Autowiring in depth - by name and @Property
  - When there are two implementations available in the component scan, it will fail.
  - You can do autowiring by name to solve this :
  ```java
  @Autowired
  SortAlgorithm bubbleSortAlgorithm;

  @Autowired
  SortAlgorithm sortAlgorithm // Autowiring by type.
  ```
  - The above is approach #1, you are using the name of the variable to solve the conflict which happens when you have more than two matches available for specific autowiring.
  - The advantage of the above is also that you are not changing the algorithms at all.
  - The other approach is to use @Primary.
  - @Primary has higher priority over the name of the variable.
  - @Qualifier annotation is another approach
    - @Qualifier("quick")
    - You say which qualifier to use:
    ```java
    @Autowired
    @Qualifier("bubble")
    private SortAlgorithm sortAlgorithm;
    ```
    - Qualifier over the component too.
    - Can use qualifier over multiple implementations unlike primary annotation.
  - If there is one clear implementation among the algorithms then use @Primary.
  - If there are situations for one implementation or another then use autowiring by name or the qualifier.

Bean Scope : default singleton.
  - Beans Scope :
    - singleton - one instance per spring context.
    - prototype - new bean whenever requested.
    - request - one bean per HTTP request.
    - session - one bean per HTTP session.
  - The container manages the beans.
  - The beans are created and the lifecycle is managed by the container.
  - applicationContex.getBean(BinarySearchImpl.class);
  - When I call the above multiple times, will I get a new bean or the same bean ?
    - When we try to get different instances in the application context, you get the same bean.
    - These types of beans are called singleton.
    - In spring, by default any bean is a singleton.
    - If you request 10 times for a bean in application context you get the same bean back.
  - Prototype :
    - New bean whenever it is requested.
    - To get a new bean each time we can add an annotation @Scope("prototype") under @Component.
    - hard coding "prototype" is not good practice and you can do : @Scope(ConfigurableBeanFatory.SCOPE_PROTOTYPE)
    - You can also specify singleton but that is the default.
  - Request and session are important for web applications.
  - Example of real world :
    - Singleton: It returns a single bean instance per Spring IoC container.This single instance is stored in a cache of such singleton beans, and all subsequent requests and references for that named bean return the cached object. If no bean scope is specified in the configuration file, singleton is default. Real world example: connection to a database.
    - Prototype: It returns a new bean instance each time it is requested. It does not store any cache version like singleton. Real world example: declare configured form elements (a textbox configured to validate names, e-mail addresses for example) and get "living" instances of them for every form being created
    - Request: It returns a single bean instance per HTTP request. Real world example: information that should only be valid on one page like the result of a search or the confirmation of an order. The bean will be valid until the page is reloaded.
    - Session: It returns a single bean instance per HTTP session (User level session). Real world example: to hold authentication information getting invalidated when the session is closed (by timeout or logout). You can store other user information that you don't want to reload with every request here as well.
    - GlobalSession: It returns a single bean instance per global HTTP session. It is only valid in the context of a web-aware Spring ApplicationContext (Application level session). It is similar to the Session scope and really only makes sense in the context of portlet-based web applications. The portlet specification defines the notion of a global Session that is shared among all of the various portlets that make up a single portlet web application. Beans defined at the global session scope are bound to the lifetime of the global portlet Session.
  - Application Context :
    - The ApplicationContext is the central interface within a Spring application that is used for providing configuration information to the application.Its created when at the start of application running.
  - @Scope(value = ConfigurableBeanFatory.SCOPE_PROTOTYPE, proxyMode = ScopedProxyMode.TARGET_CLASS)

Component Scan
  - A package might not be part of a component scan.
  - When you add @SpringBootApplication then the package it is defined in and all subpackages within it will be component scanned.
  - If the package is outside we need to do @ComponentScan("package.location") to add a component scan on them.

LifeCycle of bean : @PostConstruct and @PreDestroy
  - If you want to do something that needs all the dependencies to be populated and only then do certain actions then you use @PostConstruct.
    - Spring calls methods annotated with @PostConstruct only once, just after the initialization of bean properties. Keep in mind that these methods will run even if there is nothing to initialize.
    - One example usage of @PostConstruct is populating a database. During development, for instance, we might want to create some default users.
  - @PreDestroy is called right before the bean is removed from from the container.
    - A method annotated with @PreDestroy runs only once, just before Spring removes our bean from the application context.
    - Same as with @PostConstruct, the methods annotated with @PreDestroy can have any access level but can't be static.

CDI (Context and Dependency Injection)
  - Java EE dependency injection Standard (JSR-330)
  - Spring supports most annotaions:
    - @Inject (@Autowired)
    - @Named (@Component & @Qualifier)
    - @Singleton (Defines a scope of Singleton)
  - CDI is an interface defining how to do Dependency Injection.
  - javax.inject dependency
  - CDI only adds a few annotations.

Spring without spring boot
  - The base of spring is spring-core.
    - It is where your bean factory is defined. The basic management of your beans.
  - ApplicationContext is in spring-context.
  - The way to define an application configuration in spring is by using @Configuration.
  - ApplicationContext appContext = new AnnotationConfigApplicationContext(SpringIn5StepsBasicApplication.class);
  - Need to define a ComponentScan something spring boot does by default.
    - @ComponentScan

Notes:
  - We must make sure to close application.
  ```java
  //Not : ApplicationContext appContext = new AnnotationConfigApplicationContext(SpringIn5StepsBasicApplication.class);
  AnnotationConfigApplicationContext appContext = new AnnotationConfigApplicationContext(SpringIn5StepsBasicApplication.class);
  //The close is on the AnnotationConfigApplicationContext.
  appContext.close();
  ```
  - We can use the try catch included in Java 7 around the application context to close it automatically.
    - Implements auto closeable.
  - Logger might be broken from Spring boot to Spring if we dont included the proper implementation in POM.

Defining Spring Application Context using XML
  - Instead of using annotations we can use xml.
  - Before spring 2.5 annotaions had to be wired manually through xml.
  - applicationContext.xml under resources.
  - Must specify namespace.
  - Must specify beans in the xml.
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/context
             http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.package.another.location"/>
    <bean id="jdbcConnection" class="com.package.location.JdbcConnection">
      <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="personExample" class="com.package.location.PersonExample">
      <propery name="jdbcConnection" ref="jdbcConnection"/>
      <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

  </beans>
  ```
  - You copy the qualified name into the classname.
  - To use autowired you use the property tag. You specify which classes have the dependency.
  - With the above code we are manually creating the beans and autowiring them.
  - When using xml we need a ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("path.xml");
  - Can add context component scan in the xml. Need to add the context schema in the definitions.

JavaConfig vs XML Config
  - Benefits of JavaConfig :
    - Java is type safe. Compiler will report issues if you are configuring right bean class qualifiers.
    - XML based on configuration can quickly grow big. [Yes we can split and import but still]
    - Search is much simpler, refactoring will be bliss. Finding a bean definition will be far easier.

IOC Container vs Application Context vs Bean Factory
  - IOC Container
    - With spring we give the control of creating the bean to spring.
    - IOC controller manages the beans.
    - IoC is a generic term meaning that rather than having the application call the implementations provided by a library (also know as toolkit), a framework calls the implementations provided by the application.
    - DI is a form of IoC, where implementations are passed into an object through constructors/setters/service lookups, which the object will 'depend' on in order to behave correctly.
    - IoC containers are DI frameworks that can work outside of the programming language. In some you can configure which implementations to use in metadata files (e.g. XML) which are less invasive. With some you can do IoC that would normally be impossible like inject an implementation at pointcuts.
  - Application Context
    - An implementation of IoC container
    - Same as bean Factory with more features:
      - Spring AOP features
      - I18n capabilities
      - WebApplicationContext for web applications etc.
    - Use application context in most cases unless memory is a problem.
  - Bean Factory
    - Another implementaion of IoC container

@Service vs @Controller vs @Component vs @Repository
  - @Component Genetic Component
    - generic stereotype for any Spring-managed component
    - What’s special about @Component
      - <context:component-scan> only scans @Component and does not look for @Controller, @Service and @Repository in general. They are scanned because they themselves are annotated with @Component.
      - Thus, it’s not wrong to say that @Controller, @Service and @Repository are special types of @Component annotation. <context:component-scan> picks them up and registers their following classes as beans, just as if they were annotated with @Component.
      - Special type annotations are also scanned, because they themselves are annotated with @Component annotation, which means they are also @Components. If we define our own custom annotation and annotate it with @Component, it will also get scanned with <context:component-scan>
  - @Repository - encapuslation storage, retrieval, and search behavior typically from a relational database.
    - Data Layer
    - stereotype for persistence layer
    - What’s special about @Repository?
      - In addition to pointing out, that this is an Annotation based Configuration, @Repository’s job is to catch platform specific exceptions and re-throw them as one of Spring’s unified unchecked exception. For this, we’re provided with PersistenceExceptionTranslationPostProcessor, that we are required to add in our Spring’s application context like this:
  - @Service - Business Service Facade
    - Business layer
    - stereotype for service layer
    - What’s special about @Service?
      - Apart from the fact that it's used to indicate, that it's holding the business logic, there’s nothing else noticeable in this annotation; but who knows, Spring may add some additional exceptional in future.
  - @Controller - Controller in MVC pattern
    - Used to define a controller in web application.
    - Web Layer
    - stereotype for presentation layer (spring-mvc)
    - What’s special about @Controller?
      - We cannot switch this annotation with any other like @Service or @Repository, even though they look same. The dispatcher scans the classes annotated with @Controller and detects methods annotated with @RequestMapping annotations within them. We can use @RequestMapping on/in only those methods whose classes are annotated with @Controller and it will NOT work with @Component, @Service, @Repository etc...

Read Value from external properties file
  - application.properties
  - @Value("{value}")
  - @PropertySource(classpath:applications.properties) over the main class.
