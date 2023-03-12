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

9 - compact

    Route::get('/', function () {
        $data = compact('var1', 'var2', 'var3');
        return view('my-view', $data);
    });
