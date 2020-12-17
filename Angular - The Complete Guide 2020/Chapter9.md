Module Introduction
  - Services are for Components that may use similar code.

Why would you Need Services?

Creating a Logging Service
  - Service doesn't need @Service. It is a normal typescript class.
  - DO NOT CREATE SERVICES IN COMPONENTS MANUALLY.

Injecting the Logging Service into Components
  - Dependency is something our class will depend on.
  - We add a constructor where we want to use our service.
  - need to add providers in component in @Component.
  - When it builds component it will know how to give the instance of the service.

Creating a Data Service

Understanding the Hierarchical Injector
  - AppModule : Same Instance of Service is available Application-wide.
  - AppComponent : Same Instance of service is available for all Components(but not for other services.)
  - Any other Component : Same Instance of Service is available for the Component and all its child components.

How many Instances of Service Should It Be?
  - If you want a child to use the same service dont add the service to providers.

Injecting Services into Services
  - Need to add Service in the App Module.
  - A service does not have any metadata like @Component or @Directive so you need to add @Injectable() so something can now be injected into the service. Only add it if you plan to inject something into it.
  - It is best practice in @Injectable for angular 8 for all services.
  - A @Component requires a view whereas a @Directive does not.
  - Directives add behaviour to an existing DOM element or an existing component instance.

Using Services for Cross-Component Communication
  
