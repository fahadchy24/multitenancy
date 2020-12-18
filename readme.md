## Laravel Multi-Tenancy Trait

The whole "magic" is in this trait: __app/Traits/Multitenantable.php__:

```
namespace App\Traits;

use Illuminate\Database\Eloquent\Builder;

trait Multitenantable {

    public static function bootMultitenantable() {
        if (auth()->check()) {
            static::creating(function ($model) {
                $model->created_by_user_id = auth()->id();
            });
            static::addGlobalScope('created_by_user_id', function (Builder $builder) {
                if (auth()->check()) {
                    return $builder->where('created_by_user_id', auth()->id());
                }
            });
        }
    }

}
```

## How to use

- Clone the repository with __git clone__
- Copy __.env.example__ file to __.env__ and edit database credentials there
- Run __composer install__
- Run __php artisan key:generate__
- Run __php artisan migrate --seed__ (it has some seeded data for your testing)
- That's it: launch the main URL and login with default credentials __admin@admin.com__ - __password__

---

## License

Basically, feel free to use and re-use any way you want.

---