Module Introduction

Splitting Apps into Components

Property & Event Binding Overview
  - We can use Property and event binding on HTML Elements and Directives as well as components.
  - All properties of components are only accessible in these components.
  - @Input() from angular/core, now any parent component can bind to that component.

Assigning an Alias to Custom Properties
  - You can assign an alias : @Input('srvElement')

Binding to Custom Events
  - (serverCreated)="onServerAdded($event)"
  - In the child component you can us new EventEmitter<{serverName:string, serverContent: string}>(); as a property.
  - this.serverCreated.emit({serverName: this.newServerName, serverContent: this.newServerContent});
  - @Output() decorator is used as well.

Assigning an Alias to Custom Events
  - You can assign alias just like @Input()
  - There are instances where the distance between two components that need to communicate are too great that chaining like this is too inconvenient.

Understanding View Encapsulation
  - styling is specific to each component.

More on View Encapsulation
  - You can override the encapsulation.
  - in @Component({}) add encapsulation: ViewEncapsulation.None. This will now apply styles globally.

Using Local References in Templates
  - You can use references anywhere in your templates.
  - You can use them anywhere in the HTML. #example

Getting Access to the Template & DOM with @ViewChild()
  ```HTML
  <input type="text" class="form-control" #serverContentInput/>
  ```
  - @ViewChild() is in angular core
  ```ts
  @ViewChild('serverContentInput', {static:true}) serverContent: ElementRef;
  ```
  - The above gets the access to the local value reference.
  - Type of ElementRef. It is in angular core. Has native element property.
  - this.serverContentInput.nativeElement.value;

Projecting Content into Components with ng-content
  - There is a special directive you can add in a component using ng-content. So html wont be lost in angular to open and closing tag.
  - ng-content hook.

Understanding the Component Lifecycle
  - ngOnChanges : Called after a bound property changes.
  - ngOnInit : Called once the component is initialized. Runs after constructor.
  - ngDoCheck : Called during every change detection run.
  - ngAfterContentInit : Called after content (ng-content) has been projected into view.
  - ngAfterContentChecked : Called every time the projected content has been checked.
  - ngAfterViewInit : called after the component's view (and child views) has been initialized.
  - ngAfterViewChecked
  - ngDestroy : Called once the component is about to be destroyed.

Seeing Lifecycle Hooks in Action
  - ngChanges needs argument. changes:SimpleChanges
  - Order :
    1. constructor
    2. ngOnChanges
    3. ngOnInit
    4. ngDoCheck gets called when angular checks for any changes.
    5. ngAfterContentInit after ng-content.
    6. ngAfterContentChecked : After ngAfterContentInit
    7. ngAfterViewInit : After Content has been checked
    8. ngAfterViewChecked : After Content has been checked
    9. ngDestroy : When content gets destroyed.

Lifecycle Hooks and Template Access

Getting Access to ng-content with @ContentChild('...', {static:true}
