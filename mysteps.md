#Steps


1. Type on your terminal to create project like this `composer create-project laravel/laravel laravel_intermediate --prefer-dist`
2. Modif your `.env` file, database configuration.
3. Create migration `php artisan make:migration create_tasks_table --create=tasks`
4. Modif your created migration file at `database/migration/2016_01_18_081913_create_tasks_table.php`
5. Add like this:
```
public function up()
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->increments('id');
            $table->integer('user_id')->index();
            $table->string('name');
            $table->timestamps();
        });
    }

```
6. After that, run migration `php artisan migrate`
7. Create model Task, run `php artisan make:model Task`
8. Modif the Task model like this
```
namespace App;

use App\User;
use Illuminate\Database\Eloquent\Model;

class Task extends Model
{
    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['name'];

    /**
     * Get the user that owns the task.
     */
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```
9. Create auth, execute `php artisan make:auth --views`
10. Then we create Task Controller `php artisan make:controller TaskController`
11. Create folder `app/Repositories` and create a file `TaskRepository.php` inside.
12. Modif content of `TaskRepository.php` as below
```
<?php

namespace App\Repositories;

use App\User;
use App\Task;

class TaskRepository
{
    /**
     * Get all of the tasks for a given user.
     *
     * @param  User  $user
     * @return Collection
     */
    public function forUser(User $user)
    {
        return Task::where('user_id', $user->id)
                    ->orderBy('created_at', 'asc')
                    ->get();
    }
}
```
