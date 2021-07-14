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
