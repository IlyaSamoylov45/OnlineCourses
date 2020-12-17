Module Introduction
  - Attribute vs Structural Directives
    - Attribute Directives sit on elements like attributes
      - Look like a normal HTML attribute (possibly with databinding or event binding)
      - Only affect / change the element they are added to
    - Structural Directives change the structure around the DOM of the element.
      - Look like a normal HTML Attribute but have a leading * (for desugaring)
      - Affect a whole area in the DOM  (elements get added/removed)

ngFor and ngIf Recap

ngClass and ngStyle Recap

Creating a Basic Attribute Directive
  - import Directive from angular core.
  - @Directive({})
  - requires selector
  - We attach directive to template.
  - Contructor should have ElementRef
  - Directive has Onitit and ondestroy
  - Need to inform angular about directive in app module.

Using the Renderer to build a Better Attribute Directive
  - Not a good practice to access your elements directly.
  - Renderer2 is better process for DOM access.
  - private renderer : Renderer2
  - this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue')

Using HostListener to Listen to Host Events
  - @HostListener('mouseenter') for mouseover for something else.

Using HostBinding to Bind to Host Properties
  - @HostBinding('style.backgoundColor') backgroundColor : string;
  - Don't need renderer if you use hostBinding.

Binding to Directive Properties
  - you can use @Input to set property binding.

What Happens behind the Scenes on Structural Directives?
  - *ngIf is basically ng-template [ngIf]

Building a Structural Directive
  ```ts
  @Input() set appUnless(condition: boolean){
    if(!condition){
      this.vcRef.createEmbeddedView(this.templateRef);
    }else{
      this.vcRef.clear();
    }
  }

  constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef){

  }
  ```
  ```HTML
  <div *appUnless="onlyOdd">
    ...
  </div>
  ```

 - Structural directives—change the DOM layout by adding and removing DOM elements.  
 - Attribute directives—change the appearance or behavior of an element, component, or another directive.
 - The * before the attribute selector indicates that a structural directive should be applied instead of a normal attribute directive or property binding. Angular2 internally expands the syntax to an explicit <template> tag.

Understanding ngSwitch
```html
<div [ngSwitch]="value">
  <p *ngSwitchCase="5"><p>
  <p *ngSwitchCase="10"><p>
  <p *ngSwitchCase="100"><p>
  <p *ngSwitchDefault=""><p>
</div>
```
