## Laravel cheat sheet
## Project commands
```
// New project
$ laravel new projectName

// Launch server/project
$ php artisan serve

// commands list
$ php artisan list

// command help
$ php artisan help migrate

// Laravel console
$ php artisan tinker

// Route list
$ php artisan route:list
```
## Create and update data tables (with migrations)
```
// Create data table 
$ php artisan make:migration create_products_table

// Create table(migration exemple)
Schema::create('products', function (Blueprint $table) {
    // auto-increment primary key
    $table->id();
    // created_at and updated_at
    $table->timestamps();
    // Unique constraint
    $table->string('modelNo')->unique();
    // Not required
    $table->text('description')->nullable();
    // Default value
    $table->boolean('isActive')->default(true); 
    // Index
    $table->index(['account_id', 'created_at']);
    // Foreign Key relation
    $table->foreignId('user_id')->constrained('users')->onDelete('cascade');
});

// Update table (migration exemple)
$ php artisan make:migration add_comment_to_products_table

// up()
Schema::table('products', function (Blueprint $table) {
    $table->text('comment');
});

// down()
Schema::table('products', function (Blueprint $table) {
    $table->dropColumn('comment');
});
```
## Commons commands
```
// Database migration
$ php artisan migrate

// Data seed
$ php artisan db:seed

// Create table migration
$ php artisan make:migration create_products_table

// Create from model with migration, factory, resource controller
$ php artisan make:model Product -all

// Create a controller
$ php artisan make:controller ProductsController

// Update table migration
$ php artisan make:migration add_date_to_blogposts_table

// Rollback latest migration
php artisan migrate:rollback

// Rollback all migrations
php artisan migrate:reset

// Rollback all and re-migrate
php artisan migrate:refresh

// Rollback all, re-migrate and seed
php artisan migrate:refresh --seed
```
## Models
```
// Model mass assignment list exclude attributes
protected $guarded = []; // empty == All

// Or list included attributes
protected $fillable = ['name', 'email', 'password',];


// One to Many relationship (posts with many comments)
public function comments() 
{
    return $this->hasMany(Comment:class); 
}

// One to Many relationship (comments with one post) 
public function post() 
{                            
    return $this->belongTo(Post::class); 
}

// One to one relationship (Author with one profile)
public function profile() 
{
    return $this->hasOne(Profile::class); 
}

// One to one relationship (Profile with one Author) 
public function author() 
{                            
    return $this->belongTo(Author::class); 
}

// Many to Many relationship
// 3 tables (posts, tags and post_tag)
// post_tag (post_id, tag_id)

// in Tag model...
public function posts()
    {
        return $this->belongsToMany(Post::class);
    }

// in Post model...
public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
```
## Eloquent ORM
```
// New 
$flight = new Flight;
$flight->name = $request->name;
$flight->save();

// Update 
$flight = Flight::find(1);
$flight->name = 'New Flight Name';
$flight->save();

// Create
$user = User::create(['first_name' => 'Taylor','last_name' => 'Otwell']); 

// Update All:  
Flight::where('active', 1)->update(['delayed' => 1]);

// Delete 
$current_user = User::Find(1)
$current_user.delete(); 

// Delete by id:  
User::destroy(1);

// Delete all
$deletedRows = Flight::where('active', 0)->delete();

// Get All 
$items = Item::all(). 

// Find one by primary key
$flight = Flight::find(1);

// display 404 if not found
$model = Flight::findOrFail(1); 

// Get last entry
$items = Item::latest()->get()

// Chain 
$flights = App\Flight::where('active', 1)->orderBy('name', 'desc')->take(10)->get();

// Where
Todo::where('id', $id)->firstOrFail()  

// Like 
Todos::where('name', 'like', '%' . $my . '%')->get()

// Or where
Todos::where('name', 'mike')->orWhere('title', '=', 'Admin')->get();

// Count
$count = Flight::where('active', 1)->count();

// Sum
$sum = Flight::where('active', 1)->sum('price');

// Contain?
if ($project->$users->contains('mike')) 
```
## Routes
```
// Basic route closure
Route::get('/greeting', function () {
    return 'Hello World';
});

// Route direct view shortcut
Route::view('/welcome', 'welcome');

// Route to controller class
use App\Http\Controllers\UserController;
Route::get('/user', [UserController::class, 'index']);

// Route only for specific HTTP verbs
Route::match(['get', 'post'], '/', function () {
    //
});

// Route for all verbs
Route::any('/', function () {
    //
});

// Route Redirect
Route::redirect('/clients', '/customers');


// Route Parameters
Route::get('/user/{id}', function ($id) {
    return 'User '.$id;
});

// Optional Parameter
Route::get('/user/{name?}', function ($name = 'John') {
    return $name;
});

// Named Route
Route::get(
    '/user/profile',
    [UserProfileController::class, 'show']
)->name('profile');

// Resource
Route::resource('photos', PhotoController::class);

GET /photos index   photos.index
GET /photos/create  create  photos.create
POST    /photos store   photos.store
GET /photos/{photo} show    photos.show
GET /photos/{photo}/edit    edit    photos.edit
PUT/PATCH   /photos/{photo} update  photos.update
DELETE  /photos/{photo} destroy photos.destroy

// Nested resource
Route::resource('photos.comments', PhotoCommentController::class);

// Partial resource
Route::resource('photos', PhotoController::class)->only([
    'index', 'show'
]);

Route::resource('photos', PhotoController::class)->except([
    'create', 'store', 'update', 'destroy'
]);

// URL with route name
$url = route('profile', ['id' => 1]);

// Generating Redirects...
return redirect()->route('profile');

// Route Prefix group
Route::prefix('admin')->group(function () {
    Route::get('/users', function () {
        // Matches The "/admin/users" URL
    });
});

// Route model binding
use App\Models\User;
Route::get('/users/{user}', function (User $user) {
    return $user->email;
});

// Route model binding (other than id)
use App\Models\User;
Route::get('/posts/{post:slug}', function (Post $post) {
    return view('post', ['post' => $post]);
});

// Route fallback
Route::fallback(function () {
    //
});

// Route Caching
php artisan route:cache
```
