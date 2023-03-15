# UPLOAD 

For upload any file in Laravel 9 following this steps:

## Edit welcome page `welcome.blade.php` or create new page 
    <form action="{{ route('upload') }}" method="POST" enctype="multipart/form-data">
        @csrf
        <input type="file" name="file">
        <button type="submit">Upload</button>
    </form>

## create new controller
    php artisan make:controller fileController

## Add this fnuction to `fileController.php` 

    public function upload(Request $request)
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

***save data of file inside the database***

## Add routes to `web.php`

    use App\Http\Controllers\FileController;
    use Illuminate\Support\Facades\Route;

    Route::get("/", function(){
        return view('welcome');
    });

    Route::post('/upload', [FileController::class, 'upload'])->name('upload');

