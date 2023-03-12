# Love laravel


## Steps :




## 1 - Views

### Blade includes :

#### layout.blade.php

    <!DOCTYPE html>
    <html>
        <head>
            <title>@yield('title')</title>
        </head>
        <body>
            <div class="container">
                @yield('content')
            </div>
        </body>
    </html>

#### home.blade.php

    @extends('app')

    @section('title', 'Welcome')

    @section('content')
        <h1>Welcome to my website!</h1>
        <p>Here are some of my latest blog posts:</p>
        <ul>
            @foreach ($posts as $post)
                <li>{{ $post->title }}</li>
            @endforeach
        </ul>
    @endsection


### conditions

###### if & elseif & else
        
    @if($user->isAdmin)
        <p>Welcome, Admin!</p>
    @endif

###### unless 

    @unless($user->isAdmin)
        <p>You are not an admin.</p>
    @endunless

###### isset 
  
    @isset($user->name)
        <p>Welcome, {{ $user->name }}!</p>
    @endisset

###### empty  

    @empty($user->email)
        <p>You don't have an email address.</p>
    @endempty

###### auth

    @auth
        <p>Welcome, {{ auth()->user()->name }}!</p>
    @endauth

###### guest 

    @guest
        <p>Please login to continue.</p>
    @endguest

###### switch 

    @switch($user->role)
        @case('admin')
            <p>Welcome, Admin!</p>
            @break
        @case('editor')
            <p>Welcome, Editor!</p>
            @break
        @default
            <p>Welcome, user!</p>
    @endswitch

#### pre-defined
    Forms
    Buttons
    Cards
    Alerts
    Pagination
    Icons: (Font Awesome or Material Icons).
  
app.blade.php
auth.blade.php
guest.blade.php
mail.blade.php
pdf.blade.php

#### CSRF protection

    <form method="POST" action="{{ route('submit_form') }}">
        @csrf

        <div>
            <label for="name">Name</label>
            <input type="text" name="name" id="name" value="{{ old('name') }}" required>
            @error('name')
                <div class="alert alert-danger">{{ $message }}</div>
            @enderror
        </div>

        <div>
            <label for="email">Email</label>
            <input type="email" name="email" id="email" value="{{ old('email') }}" required>
            @error('email')
                <div class="alert alert-danger">{{ $message }}</div>
            @enderror
        </div>

        <div>
            <label for="message">Message</label>
            <textarea name="message" id="message" cols="30" rows="10">{{ old('message') }}</textarea>
            @error('message')
                <div class="alert alert-danger">{{ $message }}</div>
            @enderror
        </div>

        <button type="submit">Submit</button>
    </form>

#### URL generation and asset management

    <link rel="stylesheet" href="{{ asset('css/app.css') }}">

    <a href="{{ route('users.index') }}">View Users</a>


## 2 - Controllers

__2-1 - There are several commands related to controllers in Laravel that you can use in the command line. Here are some of the most common ones:__



#### Create a new controller  
    php artisan make:controller MyController

#### Create a new resource controller with CRUD methods

    php artisan make:controller MyResourceController --resource

#### Create a new API controller with CRUD methods

    php artisan make:controller MyApiController --api

#### Create a new controller and its associated model

    php artisan make:controller MyController --model=MyModel

#### Create a new controller and its associated resource views

    php artisan make:controller MyController --resource --model=MyModel

#### Show a list of all registered routes, including the routes for your controllers

    php artisan route:list

#### Clear the cache for your controllers

    php artisan cache:clear

#### Optimize your controllers for faster performance

    php artisan optimize

#### Show the current status of your controllers

    php artisan status

#### Generate documentation for your controllers:

    php artisan make:docs MyController

**kindes of controllers files**


__2-2 In Laravel, there are several types or kinds of controllers that you can use depending on your application's needs. Here are some of the most common types:__

### Resource Controllers

* php artisan make:controller

* CRUD system

### API Controllers

* php artisan make:controller --api

* CRUD system

* return JSON responses

### Controller Middleware (type of middleware)

applied to a specific controller or group of controllers
allowed to perform something (actions) before the controller method is executed
You can define controller middleware in the ____construct()__ method of your controller

### Invokable Controllers
allows you to define a __single method__

    php artisan make:controller --invokable 

#### Controller Traits

Controller traits are a way to reuse common controller logic across multiple controllers. You can define a trait that contains methods for things like handling form input or handling errors, and then include that trait in your other controllers. To create a controller trait, simply create a PHP file in your app/Http/Controllers/Traits directory.

#### Controller Namespacing
Controller Namespacing: Controller namespacing is a way to organize your controllers into separate namespaces based on your application's logic. For example, you might have a Admin namespace for controllers that are only accessible by administrators. To create a namespaced controller, simply create a directory with the same name as your namespace in your app/Http/Controllers directory, and then place your controller file inside that directory.

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


## 3 - Routing

1 - basic route

    Route::get('/', function () {
        return view('welcome');
    });

2 - routes with parameters

    Route::get('/user/{id}', function ($id) {
        return 'User '.$id;
    });

3 - Named route

    Route::get('/user/{id}', function ($id) {
        return 'User '.$id;
    })->name('user.profile');

4 - Route with middleware

    Route::get('/admin', function () {
        //
    })->middleware('admin');

5 - Route with multiple middleware

    Route::get('/admin', function () {
        //
    })->middleware(['auth', 'admin']);

6 - Route group

    Route::prefix('admin')->group(function () {
        Route::get('/', function () {
            // Matches the "/admin" URL
        });
        Route::get('/users', function () {
            // Matches the "/admin/users" URL
        });
    });

7 - Route with controller method

    Route::get('/user/{id}', 'UserController@show');

    Route::get('/user/{id}', [UserController::class, 'show'])

8 - Route with controller method + middleware

    Route::get('/user/{id}', [UserController::class, 'show'])->middleware("The middleware")

