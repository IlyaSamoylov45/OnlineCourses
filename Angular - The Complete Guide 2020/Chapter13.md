Module Introduction
  - Observables are various data sources. (User Input) Events, Http Requests, Triggered in Code, ...
  - Observer : You write the Code which gets executed -> Handle Data, Handle Error, Handle Completion
  - You use it to handle async tasks.
  - Observables are operators.

Analyzing Angular Observables
  - params is an observable. Angular heavily uses observables.

Getting Closer to the Core of Observables
  - Observable are added by rxjs.
  - There are observables that don't start emitting unless specified.
  - The above can lead to a memory leak.
  - Can implement OnDestory to remove subscriptions.

Building a Custom Observable
  ```ts
  Observable.create(observer => {
    setInterval( () =>{
      observer.next()
    }, 1000)
  })
  ```
  - The Observer waits for Observable

Errors & Completion
  - observer.next(count);
  - observer.error(new Error("Error"));
  - observer.complete();
    - complete doesn't fire when observer throws an error.

Observables & You!
  - You rarely make your own Observables.

Understanding Operators
  - Can be used to change observable data using pipe();
  - Can be used to transform data.

Subjects
  - Subject is the better EventEmitter.
  - Subjects are a special Observable but is more active.
  - Use Subject because they are better under the hood.
  - Only use Subjects when communicating between components 
