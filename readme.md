# Love laravel

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
