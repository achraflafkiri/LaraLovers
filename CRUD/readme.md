# CRUDS 

## Create the Task model and table
```
php artisan make:model Task -m
```

## Edit tasks migration  
```
public function up(): void
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->id();
            $table->string("name");
            $table->text("description");
            $table->timestamps();
        });
    }
```

## routes/web.php

```
Route::get('/', [TaskController::class, 'index']);
Route::get('/tasks/create', [TaskController::class, 'create']);
Route::post('/tasks', [TaskController::class, 'store']);
Route::get('/tasks/{task}', [TaskController::class, 'show']);
Route::get('/tasks/{task}/edit', [TaskController::class, 'edit']);
Route::put('/tasks/{task}', [TaskController::class, 'update']);
Route::delete('/tasks/{task}', [TaskController::class, 'destroy']);
```

## create TaskController
```
php artisan make:controller TaskController
```

## Edit the TaskController file 
```
namespace App\Http\Controllers;

use App\Models\Task;
use Illuminate\Http\Request;

class TaskController extends Controller
{
    public function index()
    {
        $tasks = Task::all();
        return view('tasks.index', ['tasks' => $tasks]);
    }

    public function create()
    {
        return view('tasks.create');
    }

    public function store(Request $request)
    {
        $task = new Task;
        $task->name = $request->input('name');
        $task->description = $request->input('description');
        $task->save();
        return redirect('/');
    }

    public function show(Task $task)
    {
        return view('tasks.show', ['task' => $task]);
    }

    public function edit(Task $task)
    {
        return view('tasks.edit', ['task' => $task]);
    }

    public function update(Request $request, Task $task)
    {
        $task->name = $request->input('name');
        $task->description = $request->input('description');
        $task->save();
        return redirect('/');
    }

    public function destroy(Task $task)
    {
        $task->delete();
        return redirect('/');
    }
}
```

## views 

resources/views/tasks/index.blade.php

```
@foreach($tasks as $task)
    <h3>{{ $task->name }}</h3>
    <p>{{ $task->description }}</p>
@endforeach

<a href="/tasks/create">Create New Task</a>
```

resources/views/tasks/create.blade.php
```
<form method="POST" action="/tasks">
    @csrf
    <label for="name">Name</label>
    <input type="text" name="name"><br>

    <label for="description">Description</label>
    <textarea name="description"></textarea><br>

    <button type="submit">Create Task</button>
</form>
```

resources/views/tasks/show.blade.php

```
<h3>{{ $task->name }}</h3>
<p>{{ $task->description }}</p>

<a href="/tasks/{{ $task->id }}/edit">Edit</a>

<form method="POST" action="/tasks/{{ $task->id }}">
    @csrf
    @method('DELETE')
    <button type="submit">Delete</button>
</form>

```

resources/views/tasks/edit.blade.php

@method('PUT')
    <label for="name">Name</label>
    <input type="text" name="name" value="{{ $task->name }}"><br>

    <label for="description">Description</label>
    <textarea name="description">{{ $task->description }}</textarea><br>

    <button type="submit">Update Task</button>
</form>

<a href="/">Back to Task List</a>
