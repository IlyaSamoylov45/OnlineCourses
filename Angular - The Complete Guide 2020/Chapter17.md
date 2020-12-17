Introduction & Why Pipes are Useful
  - Pipes are good for transforming data.

Using Pipes
  - Transforming the output
  - {{ username | uppercase }}

Parametrizing Pipes
  - Can make parameters
  - {{ server.started | date:'fullDate' }}
  - Can add more parameters separated by ':'


Where to learn more about Pipes
  - Angular pipes

Chaining Multiple Pipes
  - Can chain pipes using |
  - Order can matter like uppercase -> date

Creating a Custom Pipe
  - shorten.pipe.ts
  - needs certain method to become a pipe: PipeTransform
  ```ts
  import { PipeTransform } from '@angular/core';

  @Pipe({
    name:'shorten'
  })
  export class ShortenPipe implements PipeTransform{
    transform(value : any){
      return value.substr(0, 10);
    }
  }
  ```
  - To use it we need to add it to app.module declarations.

Parametrizing a Custom Pipe
  - By adding another argument we can pass parameters
  ```ts
  import { PipeTransform } from '@angular/core';

  @Pipe({
    name:'shorten'
  })
  export class ShortenPipe implements PipeTransform{
    transform(value : any, limit: number){
      if(value.length > limit){
        return value.substr(0, limit) + ' ...';
      }
      return value;
    }
  }
  ```
  - You can add multiple parameters

Example: Creating a Filter Pipe
  - Can use pipes to Filter
  - ng g p
  ```ts
  import { Pipe, PipeTransform } from '@angular/core';

  @Pipe({
    name:'filter'
  })
  export class FilterPipe implements PipeTransform{
    transform(value : any, filterString: string, propName: string) : any {
      if(value.length === 0 || filterString === ''){
        return value;
      }
      const resultArr = [];
      for(const item of value){
        if(item[propName] === filterString){
          resultArr.push(item);
        }
      }
      return resultArr;
    }
  }
  ```
  - Add to app.module
  - <input type="text" [(ngModel)]='filteredStatus'>
  - *ngFor = "let server of servers | filter:filterStatus:'status'"

Pure and Impure Pipes (or: How to "fix" the Filter Pipe)
  - Angular does not rerun filter when data changes.
  - High performance cost for creating filter if you want it to change dynamically on added data.
  - You can add pure : false in the @Pipe decorator to enable recalculating.
  - Will recalculate whenever any data is changed on the page.
  - CAN HIT PERFORMANCE ISSUES!!!

Understanding the "async" Pipe
  - Helps with async data.
  - {{ status | async}}
