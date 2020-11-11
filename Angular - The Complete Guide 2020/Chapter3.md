Creating a "Recipe" Model
  - A model is a ts file.

Adding Content to the Recipes Components

Outputting a List of Recipes with ngFor
 - [src] ="recipe.path"
 - src = {{recipe.path}}

Displaying Recipe Details

Working on the ShoppingListComponent

Creating an "Ingredient" Model
- There is a constructor shortcut in TS :
  ```ts
  export class Ingredient{
    constructor(public name : string, public amount : number){}
  }
  ```

Creating and Outputting the Shopping List

Adding a Shopping List Edit Section

Wrap Up & Next Steps
