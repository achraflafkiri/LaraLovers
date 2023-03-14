# Authentication

## Install the jwt
    composer require tymon/jwt-auth

## configure the package by running the following command
    php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"

## create File `config/jwt.php`
    php artisan make:model User -m

## create a User model 
    php artisan make:model User -m

## Add HasApiTokens
In User model add the `HasApiTokens` trait (سمة) provided by the `laravel/sanctum` package to handle JWT authentication

    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Factories\HasFactory;
    use Illuminate\Foundation\Auth\User as Authenticatable;
    use Laravel\Sanctum\HasApiTokens;
    class User extends Authenticatable
    {
        use HasApiTokens, HasFactory;
        // ...
    }

## Now define your login and register routes and controllers 

    <?php
    namespace App\Http\Controllers;

    use Illuminate\Http\Request;
    use Illuminate\Support\Facades\Auth;
    use Illuminate\Support\Facades\Hash;
    use App\Models\User;
    use Tymon\JWTAuth\Facades\JWTAuth;
    use Tymon\JWTAuth\Exceptions\JWTException;

    class AuthController extends Controller
    {
        public function register(Request $request)
        {
            $validatedData = $request->validate([
                'name' => 'required|max:255',
                'email' => 'required|email|unique:users',
                'password' => 'required|confirmed',
            ]);

            $user = User::create([
                'name' => $validatedData['name'],
                'email' => $validatedData['email'],
                'password' => Hash::make($validatedData['password']),
            ]);

            $token = JWTAuth::fromUser($user);

            return response()->json([
                'message' => 'User registered successfully',
                'token' => $token,
            ], 201);
        }

        public function login(Request $request)
        {
            $credentials = $request->only('email', 'password');

            try {
                if (! $token = JWTAuth::attempt($credentials)) {
                    return response()->json([
                        'error' => 'Invalid credentials',
                    ], 401);
                }
            } catch (JWTException $e) {
                return response()->json([
                    'error' => 'Could not create token',
                ], 500);
            }

            return response()->json([
                'message' => 'Logged in successfully',
                'token' => $token,
            ]);
        }
        public function logout()
        {
            JWTAuth::parseToken()->invalidate();

            return response()->json([
                'message' => 'Logged out successfully',
            ])->withCookie(Cookie::forget('token'));
        }
    }

