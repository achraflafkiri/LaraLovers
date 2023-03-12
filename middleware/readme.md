## 3 - Middlewares

Middleware in Laravel is a type of function or set of functions that sit between the incoming HTTP request and the application's response. Middleware can modify or transform the incoming request, perform validation or authentication, or apply other types of logic before passing the request along to the next middleware layer or the application's core business logic.

__3-1 - There are several commands related to middlewares in laravel that you can use in the command line. Here some of the most common ones:__

`php artisan make:middleware <middleware-name>` 

This command generates a new middleware class file in the `app/Http/Middleware` directory.

`php artisan make:middleware <middleware-name> --api` 

This command generates a new middleware class file for `api` in the `app/Http/Middleware` directory.

`php artisan route:list`

This command display a list of routes including the middleware applied to each route

`php artisan make:middleware <middleware-name> --model=<model-class>`

This command generates a new middleware class that receives a model instance in the constructor, which can be used to perform authorization checks.

`php artisan make:middleware <middleware-name> --only=<http-methods>`

This command generates a new middleware class that applies only to the specified HTTP methods.

`php artisan make:middleware <middleware-name> --except=<http-methods>:`

This command generates a all HTTP methods except the specified ones

`php artisan route:cache`

This command caches the application's routes, including the middleware applied to each route, to improve the application's performance

`php artisan route:clear`

This command clears the application's route cache

*These are just a common examples of Artisan commands related with middleware, there are many more available depending on your application's specific needs.*

__3-2 There are several kindes of middlwares in Laravel:__

* 1 - Global middleware :

> That middleware applied in all HTTP request in your app 

o  registered in the `$middleware` property of the `app/Http/Kernel.php`

* 2 - Route Middleware :

> that are applied to specific routes or groups of routes*

o defined them in the `$routeMiddleware` property of the `app/Http/Kernel` php file 

* 3 - Controller Middleware :

> that are applied to specific controller methods

o defined them in the controller class 
o using the `$middleware` property.

* 4 - Terminate Middleware:

> This middleware executed after the response is sent to the browser They can perform tasks such as logging or sending emails.

* 5 - Custom Middleware

> You can create your own custom middleware in Laravel by defining a class that implements the handle method. You can then register your middleware in the $routeMiddleware property of the app/Http/Kernel.php file and apply it to your routes.


*** some examples of middlewares in laravel ***

1 - Authentication 

2 - CSRF Middleware

3 - Session Middleware

4 - Throttle Middleware 

5 - Maintenance Mode Middleware

6 - CORS Middleware

... and more

