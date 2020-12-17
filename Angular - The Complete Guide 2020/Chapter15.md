Module Introduction
  - Working with forms.

Why do we Need Angular's Help?
  - key : value pairs.

Template-Driven (TD) vs Reactive Approach
  - Two Approaches:
    - Template-Driven : Angular infers the Form Object from the DOM.
    - Reactive : Form is created programmatically and synchronized with the DOM.
      - Allows greater control.

TD: Creating the Form and Registering the Controls
  - Import forms module.
  - Need to register controls manually.
  - Need ngModel which is the same we use in two way data binding.

TD: Submitting and Using the Form
  - Add event listener on form  element (ngSubmit)="onSubmit(f)" #f="ngForm".
  - onSubmit(form : ngForm){}

TD: Accessing the Form with @ViewChild
  - We can use ViewChild with forms.
  - Useful if you need access to form before submitting.

TD: Adding Validation to check User Input
  - Can add required to html element.
  - Can add email to html element.
  - Angular dynamically adds classes.

TD: Using the Form State
  - [disabled]="!f.valid"
  - ng-invalid class is added to the entire form!

TD: Outputting Validation Error Messages
  - Add a local #email and link it to ngModel.
  - <span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email.</span>

TD: Set Default Values with ngModel Property Binding
  - We can use ngModel in the form group to bind it to a value using data binding.
  - [ngModel]="defaultQuestion"

TD: Using ngModel with Two-Way-Binding
  - You can still use two way binding with forms.

TD: Grouping Form Controls
  - ngModelGroup="userData"
  - Can be used to group form control data together.

TD: Handling Radio Buttons
  - Can use data binding to generate values for the radio button with ngForm.

TD: Setting and Patching Form Values
  - Can set default values using the ViewChild
  - signupForm.setValue({...});
  - The above is not the best;
  - this.signupForm.formpatchValue({}) is better for patching a specific value without overriding the others.

TD: Using Form Data
  - You can access your form data.

TD: Resetting Forms
  - this.signupForm.reset();

Introduction to the Reactive Approach
  - Now we will look at Reactive approach.

Reactive: Setup
  - Reactive form is generated programmatically
  - signupForm : FormGroup
  - imported from angular/forms
  - Need ReactiveFormsModule in app.module.ts.

Reactive: Creating a Form in Code
  - Best to initialize before the template is generated.
  - make in ngOnInIt.
  - FormControl(null)

Reactive: Syncing HTML and Form
  - need to add [formGroup]="" to the form element.
  - need to add FormControlName to fields.

Reactive: Submitting the Form
  - Add event binding in the same way but no need to get the form with a local reference.

Reactive: Adding Validation
  - Since you are using reactive forms youre not configuring the form in the template. You create validators in the ts code.
  - There are built in Validators.
  - Can add an array of validators.

Reactive: Getting Access to Controls
  - \*ngIf="signupForm.get('username').valid && signupForm.('username').touched"

Reactive: Grouping Controls
  - The in signupForm.get(...) get gets a path of the element
  - You can have form groups in a form group.

Reactive: Arrays of Form Controls (FormArray)
  - type = "button"
  - Add control to an array of control : (click)="onAddHobby()"
  - 'hobbies': new FormArray([]); <- In ts file
  ```ts
  onAddHobby() {
    (<FormArray>this.signupForm.get('hobbies')).push(new FormControl(null, Validators.required))
  }
  ```
  - \<div formArrayName="hobbies">\</div>

Reactive: Creating Custom Validators
  - We can make custom Validators.
  - forbiddenNames(control: FormControl): {[s : string] : boolean}){}
  - If validation must be null if it is invalid.
  - this.forbiddenNames.bind(this)

Reactive: Using Error Codes
  - Angular adds the error code to error.

Reactive: Creating a Custom Async Validator
  - Async Validator waits for a response from server.
  - forbiddenNames(control: FormControl): Promise<any> | Observable<any>{}
  - Creates a promise for a Validator.
  - added to the third list of FormControl arguments.
  - ng-pending.

Reactive: Reacting to Status or Value Changes
  - You can subscribe to your form state.
  - valueChanges is an observable.
  - We also have statusChanges
    - It doesn't have as much information.
    - Also an observable.

Reactive: Setting and Patching Values
  - setValue and patchValue are different.
  -  patchValue is for one value.
