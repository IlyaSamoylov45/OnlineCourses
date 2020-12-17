Module Introduction
  - Angular ships with its own router.

Why do we need a Router?
  - To go to different pages.

Setting up and Loading Routes
  - Register the router in the AppModule.
  ```ts
  const appRoutes: Routes = [
    {path: '', component: HomeComponent},
    {path: 'users', component: UsersComponent},
    {path: 'servers', component: ServersComponent}
  ];
  ```
  - import {Routes} from @angular/router
  - Each must follow a path.
  - in imports add RouterModule.forRoot(appRoutes) under @NgModule.
  - ```<router-outlet></router-outlet>``` directive for where we want angular to show the selected route.

Navigating with Router Links
  - having the path in href is not a good idea because the whole app/page reloads.
  - Instead use routerLink instead of href
  - can do [routerLink]="['/users']" helps you create more complex links.

Understanding Navigation Paths
  - server vs /server is relative path vs absolute path.
  - You can also do ../server

Styling Active Router Links
  - routerLinkActive directive.
  - [routerLinkActiveOptions]="{exact:true}" do show which tab is currently active.

Navigating Programmatically
  - We can inject a router into our ts code.
  - constructor(private router : Router){}
  - this.router.navigate(['/servers']);

Using Relative Paths in Programmatic Navigation
  - Unlike the router link the navigate method doesn't know which route you're currently on.
  - Need to pass second param to method.
  - this.router.navigate(['/servers'], {relativeTo: this.route});
  - need to inject -> private route : ActivatedRoute

Passing Parameters to Routes
  - path : 'users/:id', component: UserComponent.

Fetching Route Parameters
  - we can use ActivatedRoute
  ```ts
  ngOnInit(){
    this.user = {
      id : this.route.snapshot.params['id'],
      name : this.route.snapshot.params['name']
    }
  }
  ```

Fetching Route Parameters Reactively
  - If you are on the same component you need to reinstantiate the component if you want to rerender.
  ```ts
  ngOnInit(){
    this.user = {
      id : this.route.snapshot.params['id'],
      name : this.route.snapshot.params['name']
    };
    this.route.params.subscribe(
      (params : Params) => {
        this.user.id = params['id'];
        this.user.name = params['name'];
      }
    );
  }
  ```
  - Observables allow you to easily work async tasks.

An Important Note about Route Observables
  - If a component is destroyed the subscription won't so it is good to implement ngOnDestroy, if you make your own observables.

Passing Query Parameters and Fragments
  - [queryParams]="{allowEdit: '1'}"
    - key value pairs
  - this is just another bindable property or the routerLink directive.
  - [fragment]="loading"
  - this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'})

Practicing and some Common Gotchas
  - this.route.snapshot.params['id'] will always return a path string!
  - +this.route.snapshot.params['id']

Setting up Child (Nested) Routes
  ```ts
  const appRoutes: Routes = [
  { path : 'servers', component : ServersComponent, children : [
    {path : ':id', component : ServerComponent},
    {path : ':id/edit', component : EditServerComponent}
    ]
  }
  ]
  ```
  - Child routes need a separate router because they cannot override.
  - ```<router-outlet></router-outlet>```

Using Query Parameters - Practice
```ts
  this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'preserve'})
```

Redirecting and Wildcard Routes
  - Handle all routes.
  - Add new component PageNotFound
  - {path: 'not-found', component: PageNotFound}
  - {path: '**', redirectTo: '/not-found'}
  - ** is catch all wildcards and make sure it is the last in the list of routes.

Outsourcing the Route Configuration
  - If you have a lot of routes you can make an app-routing.module.ts file
  - Remove RouteModule.forRoot(appRoutes) from imports.
  ```ts
  import {} from ...

  const appRoutes: Routes = [
    ...
    ...
    ...
  ];
  @NgModule({
    imports: [
      RouterModule.forRoot(appRoutes);
    ],
    exports: [RouterModule]
  })
  export class AppRoutingModule{

  }
  ```
  - In app.module.ts add AppRoutingModule into imports

An Introduction to Guards
  - Can be used to protect some of our routes.

Protecting Routes with canActivate
  - auth-guard.service.ts
  - normal service.
  ```ts
  import { CanActivate, ActivatedRouteSnapShot, RouterStateSnapshot } from "@angular/router";
  import { Observable } from "rxjs/Observable";
  import { Injectable } from '@angular/core'
  import { AuthService } from './auth.service'
  @Injectable()
  export class AuthGuard implements CanActivate {

    canActivate(route : ActivatedRouteSnapShot,
                state: RouterStateSnapshot, private router: Router) : Observable<boolean> | Promise<boolean> | boolean {
      return this.authService.isAuthenticated().then(
        (authenticated: boolean) => {
          if(authenticated){
            return true;
          }else{
            this.router.navigate(['/']);
          }
        }
      )

    }
  }
  ```
  - Add canActivate: [AuthGuard] to the appRoutes
  - AuthGuard is a service.

Protecting Child (Nested) Routes with canActivateChild
  - CanActivateChild in the AuthGuard implementation
  - Add canActivateChild: [AuthGuard] to the appRoutes

Using a Fake Auth Service

Controlling Navigation with canDeactivate
  - Can control whether you can leave a component.

Passing Static Data to a Route
  - {path: 'not-found', component: ErrorPageComponent, data: {message: 'Page not found!'}}

Resolving Dynamic Data with the resolve Guard
  - {path : ':id', component : ServerComponent, resolve : {server : ServerResolver}},

Understanding Location Strategies
  - with 404 your server will return to index.html. You need to make sure it works.
  - RouterModule.forRoot(appRoutes, {useHash: true});
    - Allows angular to take over.
