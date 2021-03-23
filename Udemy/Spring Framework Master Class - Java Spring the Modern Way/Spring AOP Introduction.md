AOP Introduction
  - AOP dependency is not available on the Spring Initializr website anymore.
  - Dependency :
  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
  </dependency>
  ```
  - Can intercept any calls to bean and do some things around that.
  - Aspect Oriented Programming.
    - Cross cutting concern.
    - Security, logging etc.
    - Not single layer concerns.
  - AOP is the best approach for implementing cross cutting concerns.
  - implements CommandLineRunner
    - invoked as soon as application is launched.
    - Need to implement a run.
    - The good thing about CommandLineRunner is that we can autowire things in because we no longer need to write code in a static way.
  - @Before can be used as Advice.
    - Need to define method calls to intercept.
    - At top of class :
      - @Configuration
      - @Aspect -> AOP defined stuff
    - @Before("execution(\* PACKAGE.\*.\*(...))")
      - public void before(JoinPoint joinPoint){}
    - Before intercepts before a call happens.
    - You can use it for instances to check whether a user has access to something.

Terminology
  - PointCut
    - Defines the methods that I would want to intercept
    - "execution(\* PACKAGE.\*.\*(...))"
  - Advice
    - The logic that happens when you intercept a method.
  - Aspect
    - The combination of pointcut and advice is referred to as aspect.
  - Join Point
    - Specific interception of a method call. Specific execution instance.
  - Weaving
    - The processes of implementing the AOP around your method calls.
    - The framework that implements it is called a weaver.

After, AfterReturning and AfterThrowing advices
  - @AfterReturning
    - After returning the method.
    - The result can be mapped to Object result.
  - @AfterThrowing
    - Intercept an exception that is thrown.
  - @After
    - Called in both scenarios where something returns or something is thrown.

Around
  - @Around advice is more advanced
    - Define @Around then the pointcut.
    - ProceedingJoinPoint is used instead of JoinPoint
      - You allow the method to proceed before doing something.
      - Example:
        - dosomething
        - Allow the execution of method.
          - joinPoint.proceed();
        - do something else.

AOP Best Practice
  - Have a separate file where you define all the pointcuts.
    - Make a class and annotate the method with pointcut.

Other Things
  - Can have && in pointcut.
    - within the execution thing.
  - Can define beans in pointcut.
  - Can use within to intercept all calls within a layer.
    - Dont need a \*
    - "within(PACKAGE..\*)"

Custom Annotation and Aspect
  - @TrackTime
  ```java
  @Target(ElementType.METHOD) // Methods
  @Retention(RetentionPolicy.RUNTIME) // Runtime
  public @interface TrackTime{

  }
  ```
  ```java
  @PointCut("@annotation(package.TrackTime)")
  public trackTimeAnnotation(){

  }
  ```
