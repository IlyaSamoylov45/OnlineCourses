Module Introduction
  - NgRx Module for state management for larger applications.

What is Application State?
  - Application state is lost whenever your application refreshes.
  - To solve refresh problem we normally use a backend.
  - Persistent state on backend.
  - The bigger your app gets and the more complex your state gets then you can end up with a state management nightmare.
  - RxJS to the Resue (Partly)
  - (User) Event in UI/ APP => State Changing Event => Observable => Operators => Listener => Update UI
  - Subscriptions are good for unidirectional state.

What is NgRx?
  - Issues with RxJS Approach:
    - State can be updated anywhere
    - State is (possibly mutable)
    - Handling side effects is unclear.

Redux to the Rescue
  - Store Application state
  - Have one large datastore.
  - Services and components communicate but receive their state from the store.
  - Actions are sent to reducers which reduces / combines state.
  - Reducer returns a new state according to the action which saves the reduced state (immutably)
  - You can use redux in Angular, but NgRx is angulars implementation of Redux.
  - All state is managed as one large observable.
  - NgRx provides a tool to make dealing with side effects easier.

Getting Started with Reducers
  - It is fine to work with services and subjects.
  - State should be a js object.
