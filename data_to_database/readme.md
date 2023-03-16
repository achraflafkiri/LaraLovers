# Insert data to database

## create view file `form.blade.php`

```
<form action="{{ route('upload') }}" method="POST" enctype="multipart/form-data">
    @csrf
    <input type="file" name="file">
    <input type="text" name="username">
    <button type="submit">Upload</button>
</form>
```

**_If you you want to upload a **File** the **enctype** attribute is required to use_**
**_name attribute is required for receive the value of the inputs_**

## create controller using this command:

```
php artisan make:controller formController --resource
```

**_we use `--resource` for create Controller File with CRUD system (show, store, destroy ...)_**

CRUD (create read update and delete)

#### The role of all functions of `resource Controller`:

`index()` - Displays a list of resources (resources is a **data** you want to send to the database)
`create()` - Displays a form to create a new resource
`store(Request $request)` - Stores a new resource
`show(string $id)` - Displays a specific resource
`edit(string $id)` - Displays a form to edit a specific resource
`update(Request $request, string $id)` - Updates a specific resource
`destroy(string $id) `- Deletes a specific resource

## Add this code to `formController.php` file

```
public function index(){
    return view("welcome");
}

public function store(Request $request)
{
    // Validate the file upload
    $request->validate([
        'file' => 'required|mimes:txt,pdf,doc,docx|max:2048'
    ]);

    // Get the uploaded file
    $file = $request->file('file');

    // Generate a unique name for the file
    $filename = time() . '_' . $file->getClientOriginalName();

    // Move the file to the uploads directory
    $file->move(public_path('uploads'), $filename);

    // Store the file path in the database
    DB::table('files')->insert([
        'name' => $file->getClientOriginalName(),
        'path' => $filename,
        'created_at' => now(),
        'updated_at' => now(),
    ]);

    // Return a success response
    return back()->with('success', 'File uploaded successfully.');
}
```

**_Now we need 2 functions `index()` for display **list of resources** and `store(Request $request)` for Add new data to the database_**

**_`Request $request` use this params for receive data from the inputs (The inputs should be have a `name` attribute)_**

## Create model using this command

```
php artisan make:model User -m
```

## Add this Schema to users table (migration)

```
    Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->file("file");
            $table->timestamps();
    });
```

## Edit the `web.php`

```
use App\Http\Controllers\formController;
use Illuminate\Support\Facades\Route;

Route::get("/", [formController::class, "index"]);
Route::post('/upload', [formController::class, 'upload'])->name('upload');
```
