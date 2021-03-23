How Does Angular Interact with Backends?
  - You do not connect to database directly.

The Anatomy of a Http Request
  - url (API Endpoint)
  - HTTP Verb : POST, GET, PUT, DELETE...
  - Headers : (Metadata)
  - Body : information of data

Sending a POST Request
  - HttpClientModule in the imports array.
  - HttpClient to send requests.
  - have to add .json to firebase.
  - this.http.post(url, data)
  - Have to add subscribe to get the data.
  - POST sends two requests, the first is OPTIONS to see whether it is allowed to send the request.
  - GET methods have no request body.

Using RxJS Operators to Transform Response Data
  - Good practice to use observables and pipe.

Using Types with the HttpClient
  - We can set what kind of object angular should expect when making a request.
  - ? for optional fields.

Showing a Loading Indicator
  - Can add a fetching indicator.

Using a Service for Http Requests
  - Services do the heavy lifting.
  - Best to put http posts in services.
  - map is in rxjs.

Sending a DELETE Request
  - Can delete posts.

Handling Errors
  - Different ways to handle errors.
  - Can have an error message instead of breaking.
  - There is an angular error key

Using Subjects for Error Handling
  - We can handle errors with subjects to emit data.
  - Good practice to unsubscribe subjects after using them.

Using the catchError Operator
  - Can use catchError for internal errors as an observable.
  - You should properly handle errors.

Setting Headers
  - There is an option where you configure requests like headers.
  - Key value pairs of headers as new HttpHeaders({})

Adding Query Params
  - You can also add query params. Using HttpParams
  - Can make a params object with multiple params.

Observing Different Types of Responses
  - You can change the way the response is read.
  - Can set observe : response.
  - You also have observe : events
  - Can execute code without changing the data.
  - Events are good for ui If you want to change based on the response.

Changing the Response Body Type
  - You can change the response type.

Introducing Interceptors
  - Good for headers where we create headers multiple times.
  - implements HttpInterceptor class.
  - providers : [{provide: HTTP_INTERCEPTORS, useClass: AuthInterceptorService, multi: true}]
  - You can modify request objects in the interceptors.
  - Can be used for authentication. A call for every request.

Response Interceptors
  - You can modify the response in the interceptor as well.
  - In the interceptor you always recieve an event.

Multiple Interceptors
  - Order matters.
