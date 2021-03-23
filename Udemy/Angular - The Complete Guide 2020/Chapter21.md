Dynamic Components
  - Create at runtime
  - Like errors or overlays.
  - Components we load on runtime

Understanding the Different Approaches
  - Dynamic components are loaded Programmatically
  - Basically ngIf, but there is an alternative
  - Can use Dynamic Component Loader that is no longer in use.
  - This approach adds to the DOM using code.
  - Need to import ComponentFactoryResolver
  - entryComponents are needed when working with ComponentFactoryResolver in the appmodule.
