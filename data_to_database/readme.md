# Insert Data with Simple Form in Database using Laravel 9

This repository provides step-by-step instructions on how to insert data with a simple form in a database using Laravel 9.
Installation

1 - create a new laravel project:
    composer create-project laravel/laravel my-app

2 - Create a database:
**_We need to create a database for our project. You can use tools like phpMyAdmin or the command line or workbench to create a database._**

3 - Configure Database Connection:
**_Laravel 9 uses the .env file to store environment variables, including database configuration. We need to set the database name, username, password, and host in the .env file. Here's an example_**:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=database_name <- put your database name here
DB_USERNAME=username
DB_PASSWORD=password
```

4 - Create a Model:
**_Models are used to represent database tables. We can create a model using the following command_**
php artisan make:model Task -m
This command creates a **model** named Task with a **migration** file

5 - Create a Migration:
**_Migrations are used to create database tables. We can create a migration using the following command:_**
php artisan make:migration create_tasks_table
This command creates a **migration** file named **create_tasks_table**

6 - Define the Database Schema:
**_In the migration file, we need to define the database schema for the tasks table. Here's an example:_**
    Schema::create('tasks', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('description');
    $table->timestamps();
    });
This code creates a **tasks table** with id, title, description, and timestamps columns.

7 - Run Migrations:
**_We need to run the migration to create the tasks table in the database. We can run the migration using the following command:_**

    php artisan migrate

8 - Create a Controller:
**_Controllers are used to handle user requests. We can create a controller using the following command:_**

    php artisan make:controller TaskController

This command creates a controller named TaskController.

9 - Define Routes:
**_Routes are used to map user requests to controller methods. We can define a route for the create task form using the following code in the routes/web.php file:_**

    Route::get('/tasks/create', [TaskController::class, 'create']);
    Route::post('/tasks', [TaskController::class, 'store']);

This code defines two routes: one for the create task form and one for storing the task data.

10 - Define Controller Methods:
**_In the TaskController, we need to define two methods: create and store. The create method returns the create task form, and the store method stores the task data in the database. Here's an example:_**

    public function create()
    {
        return view('tasks.create');
    }

    public function store(Request $request)
    {
        $task = new Task;
        $task->title = $request->title;
        $task->description = $request->description;
        $task->save();

        return redirect('/tasks');
    }

**_This code defines the create and store methods. The create method returns the create task form, and the store method creates a new task instance, sets its properties using the request data, and saves it to the database._**

11 - Create a View:
**_Views are used to display the create task form. We can create a view named create.blade.php in the resources/views/tasks directory using the following code:_**

    <form method="POST" action="/tasks">
        @csrf
        <div>
            <label for="title">Title:</label>
            <input type="text" name="title" id="title">
        </div>
        <div>
            <label for="description">Description:</label>
            <textarea name="description" id="description"></textarea>
        </div>
        <div>
            <button type="submit">Create Task</button>
        </div>
    </form>

12 - Test the Application:

     php artisan serve
