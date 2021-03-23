What are Modules?
  - AppModules are a way to bundle angular building blocks together.
  - App router uses ngModule as well as appmodule.
  - declarations is an array of all directives, components, and pipe.
  - imports array is important to split your app into multiple modules.
  - Injectable is a faster way to adding to ngModule.
  - bootstrap is for the root components.
  - entryComponents is for importing components in code.
  - Splitting modules can make it cleaner and leaner.
  - Every module works on their own in angular
  - We should split up modules into FEATURE modules.

Understanding Shared Modules
  - Can create shared modules that use multiple modules.
  - Can't declare multiple declarations in multiple modules.
  - Can import them though.

Understanding the Core Module
  - Can set the provided in in the core modules. For providers.
  - Have the injectables in their own core module
  - Good for services.
  - Don't need to export since services work different.
  - export into the declarations of app.module

Understanding Lazy Loading
  - Lazy loading is good for loading modules only when we visit the module. App can start faster.
  - Lazy loading needs its own routes.
