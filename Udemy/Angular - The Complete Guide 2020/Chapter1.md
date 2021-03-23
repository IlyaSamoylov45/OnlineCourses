Chapter 1
  Angular
    - JS changes the DOM
    - Angular, Angular 2, Angular 8
    - Angular 2 was complete rewrite of AngularJS (Angular 1)
    - Angular 3 was skipped.
    - New Angular version released every 6 months.
    - Small incremental changes
  NodeJS
    - node npm to manage dependencies.
    ```
    {sudo} npm install -g @angular/cli@latest
    ```
      - sudo probably  required if not windows
      - install angular globally.
      - latest is optional.
    ```
    ng new my-first-app
    ```
      - creates new angular project.
    - Angular uses typescript which is a superset of Javascript.
    ```
    ng serve
    ```
      - runs on localhost  port 4200 by default
    - environments folder for environmental variables.
    - assets folder for images that your project serves.  
  ```html
  <input type="text" [(ngModel)="name"]>
  <p>{{name}}</p>
  ```
  TypeScript
    - More robust code, does not run in browser and compiled to JS.
    
