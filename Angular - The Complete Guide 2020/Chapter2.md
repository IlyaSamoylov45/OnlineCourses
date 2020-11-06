Creating a New Component
   - most angular cli components will go in app folder.
   - naming convention : <folder>.component.ts
   - A component is a ts class that angular can instatiate.
   ```ts
   import { Component } from '@angular/core'; // core functionalities of angular

   // class decorator that angular doesnt know at start
   @Component({
     selector: 'app-server' // html tag where we can use this component should be unique. You can use this in other components. So you dont overwrite a default html element
     templateUrl: './server.component.html' // html file of code. Relative path!
   })
    export class ServerComponent{

    }
   ```
Understanding the Role of AppModule and Component Declaration
  - AppModule
    - modules bundle pieces into packages.
    - we transform it into something else using a module.
    - bootstrap tells the angular which components to be aware of at start.
  - by default angular will not scan all files so it wont know about the server component.
  - register it in the declarations array. And import it using typescript.
  - imports allow us to add more modules. Browser adds basic functionality of angular.

Using Custom Components
  - We use selector to display the the component.

Creating Components with the CLI & Nesting Components
  - You can use CLI to generate components:
  ```shell
  ng generate component <name>
  ng g c <name>
  ```
  - specs file is used for testing.
  - angular doe s the adding into app declarations automatically
Working with Component Templates
  - You can define html in component template
  - ' for one line
  - backtick form multiline `

Working with Component Styles
  - We can use <name>.component.css to do css styling
  - styleUrls: [] -> /sets styles sheets.
  - styles : for in line styling
  ```ts
  styles: [
    `
    h3{
      color : dodgerblue;
    }
    `
  ]
  ```

Fully Understanding the Component Selector
  - selectors are a cs selector
  ```
  <app-servers></app-servers>
  <div app-servers></div>
  <div class="app-servers"><div>
  ```
What is Databinding?
  - communication between component ts and template html.
  - Output data from ts to template :
    - String interpolation [{{data}}]
    - Property Binding [property] = "data";
  - React to user events from template to ts code :
    - Event Binding (event) = "expression"
  - Combination of Both is Two way Binding { [(ngModel)] = "data"}

String Interpolation
  ```ts
  <p> {{ 'Server' }} with Id {{ serverId }} is {{ serverStatus }} the service is {{ getServerStatus() }}</p>
  ```
  - Can use methods in string interpolation if it returns a string.
  - Only used of strings or things that can turn into strings.

Property Binding
  - constructor is executed when the component is createdby angular.
  ```ts
  constructor(){
    setTimeout(() => {
      this.allowNewServer = true;
    },2000)
  }
  ```
  - Square brackets tell angular this is property binding.
  ```html
  <button
  class="btn btn-primary" [disabled]="!allowNewServer">Add Server</button>
  ```
  - We are directly binding disabled to the properties.
  - You can also bind to other properties like directives.

Property Binding vs String Interpolation
  ```html
  <p>{{allowNewServer}}</p>
  <p [innerText]="allowNewServer"></p>
  ```
  - Do not mix property binding with string interpolation!

Event Binding
  - triggered within template.
  ```html
  <button
  class="btn btn-primary" (click)="onCreateServer()">Add Server</button>
  ```

Passing and Using Data with Event Binding
  - $event is kinda a reserved variable in event binding.
  ```html
  <input type="text"
    class="form-control"
    (input)= "onUpdateServerName($event)">

    <p>{{ serverName }}</p>
  ```
  ```ts
  onUpdateServerName(event: Event){
    console.log(event);
    this.serverName = (<HTMLInputElement>event.target).value;
  }
  ```

Two-Way-Databinding
  - Combine property and event binding
  ```html
  <input type="text"
    class="form-control"
    [(ngModel)]="serverName"
    >
  ```
  - This will do the following:
    1. It will trigger on the input event and update the value of server name in our component automatically.
    2. It will also update the value of the input element if we change the server name somewhere else.

Understanding Directives
  - Directives are instructions in the DOM.
  - Components are such instructions.
  - Components are Directives with a template.

Using ngIf to Output Data Conditionally
  - star is required because ngIf is a structural directive meaning it changes the structure of our DOM.
  - \*ngIf

Enhancing ngIf with an Else Condition
  ```html
  <p *ngIf="serverCreated"; else noServer> Server name : {{ serverName }} </p>
  <ng-template #noServer>
    <p>NO SERVER</p>
  </ng-template>
  ```
  - Another option is to use the two boolean in ngIf.

Styling Elements Dynamically with ngStyle
  - Unlike structural directives, attribute directives don't add or remove elements. They only change the element they were placed on.
  - Needs configuration to do something so use property binding.
  - property binding != styling
  ```html
  <p [ngStyle]= " {backgroundColor:getColor()} "> <p>
  ```

Applying CSS Classes Dynamically with ngClass
  ```html
  <p [ngStyle]= " {backgroundColor:getColor()}" [ngClass]="{online: serverStatus === 'online'}"> <p>
  ```
  - ngClass is key value pair.

Outputting Lists with ngFor
  - Last built in directive.
  - \*ngFor
  ```html
  <app-server *ngFor="let server of servers"></app-server>
  ```
  - In ng-class you are loading a CSS class defined in some place, like Bootstrap. In ng-style you are setting the CSS style that you want that element to have.
