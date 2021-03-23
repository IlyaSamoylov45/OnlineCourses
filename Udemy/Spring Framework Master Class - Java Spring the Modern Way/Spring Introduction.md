What is Spring?
  - Spring is a dependency injection framework

Web Layers:
  - Typically a web layer:
    - Concerned with UI
    - Dependednt on the business layer.
  - Business Layer:
    - The business logic
    - If business layer needs some data it calls some class which is present in the data layer.
    - Business layer dependent on the data layer.
  - Data Layer:
    - Talking to external interfaces and Database.

What is Dependency?
  - The concept of dependency is simple : whatever the service needs to be provided to in able to provide its service.
  - With thousands of classes there could be thousands of dependencies.
  - Before you had to instantiate the dependencies directly.
    - This resulted in tight coupling.
    - Good code has loose coupling.
  - You remove the instantiate you can provide a constructor which provides the implementation.
  - You create the service and pass whatever classes it should use / implement.
  ```java
  SortAlgo sortAlgo = new BubbleSort();
  ComplexBusinessService businessService = new ComplexBusinessService(SortAlgo sortAlgo);
  ```
  - The service above is not dependent on a specific sort algorithm.

Spring Framework
  - Spring Framework instantiates objects and populates the dependencies.
  - You tell spring framework what the objects you need to manage and what the dependencies are of each class.
  - Two important annotations :
    - @Autowired
      - State which component is a dependency for the service.
      - The @Autowired annotation allows the container to automatically inject the message property from the application-context.xml file. The container searches for String bean object in the xml file and injects it here. If multiple String beans exist, then we will get an error.
    - @Component
      - Means that Spring starts to manage instances of this class.
      - The @Component tells that Message is a component, a bean whose object has to be created.
  - Spring Framework does Dependency injection.
  - The Spring Framework understands the different annotations that you are putting on top of your classes.

Terminology
  - Beans
    - The instances that spring manages are called beans.
    - Beans are different objects that are managed by spring framework.
  - Autowiring
    - The process where spring identifies the dependencies, identifies the matches for the dependencies and populates them.
  - Dependency Injection
    - Injecting one instance as a dependency into another instance.
  - Inversion of Control
    - Taking the control from the class that need the dependency and giving the control to the framework.
  - IOC Container
    - Generic terminology to represent anything that is implementing IoC.
    - In case of Spring Framework the typical IOC container is the application context.
  - Application Context
    - Where all the beans are created and managed.
