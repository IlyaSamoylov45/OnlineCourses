Introduction
  - Spring Boot is good for microservices.
  - Spring Boot goals
    - Enable building production ready applcations quickly.
    - Provide common non-functional features
    - metrics
    - health checks
    - externalized configuration
  - NOT A WEB SERVER OR A WEB SERVER
  - Spring Boot comes with the context of Embedded Servers.

Embedded Servers
  - An embedded server is embedded as part of the deploy-able application. If we talk about Java applications, that would be a JAR. The advantage with this is you don't need the server pre-installed in the deployment environment. With SpringBoot, the default embedded server is Tomcat

Before Spring Boot
  - Before Spring Boot we had to implement a lot of deefault exception handing.
  - We needed to define component scan and configure view resolver to redirect the views to a jsp.
  - Needed to implement message source and locale resolver and configure web.xml.
  - Basically we had to do a lot more work to get a simple web application up and running.
  - Spring Boot does this automatically so you can focus on the business logic.

Web Annotations Examples
  - @RestController
  - @GetMapping("/uri")

Spring Boot
  - Spring Boot has a feature called auto configuration.
    - Frameworks available on the classpath
    - Existing configuration for the application.
  - Spring Boot has @SpringBootApplication

Spring Boot vs Spring MVC and Spring
  - They have their own troubleshooting
  - Most important feature of Spring Framework is DI. At the core of all Spring modules is DI or IOC Inversion of Control.
  - Spring
    - Spring Framework is a widely used Java EE framework for building applications.
    - It aims to simplify Java EE development that makes developers more productive.
    - The primary feature of the Spring Framework is dependency injection.
    - It helps to make things simpler by allowing us to develop loosely coupled applications.
    - The developer writes a lot of code (boilerplate code) to do the minimal task.
    - To test the Spring project, we need to set up the sever explicitly.
    - It does not provide support for an in-memory database.
    - Developers manually define dependencies for the Spring project in pom.xml.
  - Spring Boot
    - Spring Boot Framework is widely used to develop REST APIs.
    - It aims to shorten the code length and provide the easiest way to develop Web Applications.
    - The primary feature of Spring Boot is Autoconfiguration. It automatically configures the classes based on the requirement.
    - It helps to create a stand-alone application with less configuration.
    - Spring Boot offers embedded server such as Jetty and Tomcat, etc.
    - It offers several plugins for working with an embedded and in-memory database such as H2.
    - Spring Boot comes with the concept of starter in pom.xml file that internally takes care of downloading the dependencies JARs based on Spring Boot Requirement.
    - It reduces boilerplate code.
    - Spring Boot is a module of Spring for packaging the Spring-based application with sensible defaults.
    - There is no requirement for a deployment descriptor.
    - There is no need to build configuration manually.
    - It provides default configurations to build Spring-powered framework.		
    - It avoids boilerplate code and wraps dependencies together in a single unit.
    - It reduces development time and increases productivity.
  - Spring MVC
    - Spring MVC is a model view controller-based web framework under the Spring framework.
    - It provides ready to use features for building a web application.
    - A Deployment descriptor is required.
    - It specifies each dependency separately.
    - Takes more time than Spring boot.
    - It requires build configuration manually.

Spring Boot Starter Projects
  - spring-boot-starter-web-services - SOAP Web Services
  - There are more.

Spring Boot Actuator:
  - For monitoring app.
  - A lot of meta-data regarding application.
    - How many times you called a specific service.
    - Bean information.
  - Exposes a lot of rest services.
  - Spring Boot Actuator is a sub-project of the Spring Boot Framework. It includes a number of additional features that help us to monitor and manage the Spring Boot application. It contains the actuator endpoints (the place where the resources live).
  - In application.properties -> management.endpoints.web.exposure.include=*

Spring Boot developer tools
  - Allows autorecompiling on code changes.
  - Takes less time then manually.
  - Only application related things are taken into consideration.
