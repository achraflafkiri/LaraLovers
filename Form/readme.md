# FORM DATA TO DATABASE + HANDLE ERROR 

## Edit `welcome.blade.php` :

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous">
    </script>
    <link rel="stylesheet" href={{ asset('css/style.css') }}>
</head>

<body>

    <section class="wrapper">

        @if ($errors->any())
            <div class="alert alert-danger">
                <ul>
                    @foreach ($errors->all() as $error)
                        <li>{{ $error }}</li>
                    @endforeach
                </ul>
            </div>
        @endif

        <br>
        <br>

        <form action="{{ route('send') }}" method="POST" enctype="multipart/form-data">
            @csrf
            <div>
                <label for="name">Name:</label>
                <input type="text" class="form-control" value="{{ old("name", "") }}" id="name" name="name">
            </div>
            <div>
                <label for="email">Email:</label>
                <input type="email" class="form-control" value="{{ old("email", "") }}" id="email" name="email">
            </div>
            <div>
                <label for="password">Password:</label>
                <input type="password" class="form-control" value="{{ old("password", "") }}" id="password" name="password">
            </div>
            <div>
                <label for="confirm_password">Confirm Password:</label>
                <input type="password" class="form-control" value="{{ old("confirm_password", "") }}" id="confirm_password" name="confirm_password">
            </div>
            <div>
                <label for="phone">Phone:</label>
                <input type="tel" class="form-control" value="{{ old("phone", "") }}" id="phone" name="phone">
            </div>
            <div>
                <label for="address">Address:</label>
                <textarea id="address" class="form-control" value="{{ old("address", "") }}" name="address"></textarea>
            </div>
            <div>
                <label for="gender">Gender:</label>
                <input type="radio" id="male" name="gender" value="male">
                <label for="male">Male</label>
                <input type="radio" id="female" name="gender" value="female">
                <label for="female">Female</label>
            </div>
            <div>
                <label for="interests">Interests:</label>
                <input type="checkbox" id="interest1" name="interests[]" value="reading">
            </div>
            <div>
                <label for="cv">CV:</label>
                <input type="file" name="file" class="form-control" id="cv" name="cv">
            </div>
            <br>
            <br>
            <div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </div>
        </form>
    </section>

</body>

</html>

```


## add model 

```
php artisan make:model formController -m
```

## add this code to Form model `Form.php` 

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Form extends Model
{
    use HasFactory;

    protected $fillable = [
        'file',
        'name',
        'email',
        'password',
        'confirm_password',
        'phone',
        'address',
        'gender',
        'interests',
    ];
}

```

## Add this code in `create_forms_table.php`

```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('forms', function (Blueprint $table) {
            $table->id();
            $table->string("name");
            $table->string("email");
            $table->string("password");
            $table->string("confirm_password");
            $table->string("phone");
            $table->string("address");
            $table->string("gender");
            $table->string("interests");
            $table->string("cv");
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('forms');
    }
};

```



## add this code to `web.php`

```
<?php

use App\Http\Controllers\formController;
use Illuminate\Support\Facades\Route;

Route::get("/", [formController::class, "index"]);
Route::post("/", [formController::class, "store"])->name("send");
```


## add formController file

```
<?php

namespace App\Http\Controllers;

use App\Exceptions\AppError;
use App\Models\Form;
use Illuminate\Http\Request;

class formController extends Controller
{

    public function index()
    {
        return view("welcome");
    }

    public function store(Request $request)
    {

        $request->validate([
            'file' => 'required|mimes:jpeg,png,pdf|max:2048',
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users|max:255',
            'password' => 'required|string|min:8|confirmed',
            'phone' => 'required|string|max:20',
            'address' => 'required|string|max:255',
            'gender' => 'required|in:male,female,other',
            'interests' => 'required|array|min:1',
        ]);

        $file = $request->file("file");
        $filename = $file->getClientOriginalName();

        $name = $request->input("name");
        $email = $request->input("email");
        $password = $request->input("password");
        $confirm_password = $request->input("confirm_password");
        $phone = $request->input("phone");
        $address = $request->input("address");
        $gender = $request->input("gender");
        $interests = $request->input("interests[]");

    
        $data = new Form();
        $data->file = $filename;
        $data->name = $name;
        $data->email = $email;
        $data->password = $password;
        $data->confirm_password = $confirm_password;
        $data->phone = $phone;
        $data->address = $address;
        $data->gender = $gender;
        $data->interests = $interests;
        $data->save();

        // $data = Form::create([
        //     'file' => $filename,
        //     'name' => $name,
        //     'email' => $email,
        //     'password' => $password,
        //     'confirm_password' => $confirm_password,
        //     'phone' => $phone,
        //     'address' => $address,
        //     'gender' => $gender,
        //     'interests' => $interests,
        // ]);
        

        return view("dashboard");
    }
}
```

## Create a new exception class by running the following command in your terminal

```php artisan make:exception AppError```

## add this code to `AppError`

```
<?php

namespace App\Exceptions;

use Exception;

class AppError extends Exception
{
    protected $message = 'An error occurred.';
    protected $code = 500;
}

```

## add code to `handler.php`


```
<?php

namespace App\Exceptions;

use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Throwable;

class Handler extends ExceptionHandler
{
    /**
     * A list of exception types with their corresponding custom log levels.
     *
     * @var array<class-string<\Throwable>, \Psr\Log\LogLevel::*>
     */
    protected $levels = [
        //
    ];

    /**
     * A list of the exception types that are not reported.
     *
     * @var array<int, class-string<\Throwable>>
     */
    protected $dontReport = [
        //
    ];

    /**
     * A list of the inputs that are never flashed to the session on validation exceptions.
     *
     * @var array<int, string>
     */
    protected $dontFlash = [
        'current_password',
        'password',
        'password_confirmation',
    ];

    /**
     * Register the exception handling callbacks for the application.
     */
    public function register(): void
    {
        $this->reportable(function (Throwable $e) {
            //
        });
    }

    public function render($request, Throwable $exception)
    {
        if ($exception instanceof AppError) {
            return redirect()->back()->withErrors([
                'error' => $exception->getMessage(),
            ]);
        }

        return parent::render($request, $exception);
    }
}

```
