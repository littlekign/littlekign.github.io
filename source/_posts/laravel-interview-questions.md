---
title: laravel-interview-questions
date: 2019-11-19 10:23:38
tags: laravel
---

[TOC]

## Q1: What is the Laravel?

**Laravel** is a free, open-source PHP web framework, created by Taylor Otwell and intended for the development of web applications following the model–view–controller (MVC) architectural pattern.

## Q2: What are some benefits of Laravel over other Php frameworks?

- Setup and customisation process is easy and fast as compared to others.
  Inbuilt Authentication System
- Supports multiple file systems
- Pre-loaded packages like Laravel Socialite, Laravel cashier, Laravel elixir, Passport, Laravel Scout
- Eloquent ORM (Object Relation Mapping) with PHP active record implementation
- Built in command line tool “Artisan” for creating a code skeleton ,database structure and build their migration

## Q3: Explain Migrations in Laravel

**Laravel Migrations** are like version control for the database, allowing a team to easily modify and share the application’s database schema. Migrations are typically paired with Laravel’s schema builder to easily build the application’s database schema.

## Q4: What is the Facade Pattern used for?

**Facades** provide a static interface to classes that are available in the application's service container. Laravel facades serve as static proxies to underlying classes in the service container, providing the benefit of a terse, expressive syntax while maintaining more testability and flexibility than traditional static methods.

All of Laravel's facades are defined in the `Illuminate\Support\Facades` namespace. Consider:

```php
use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});
```

## Q5: What is Service Container?

The Laravel **service container** is a tool for managing class dependencies and performing dependency injection.

## Q6: What is Eloquent Models?

The Eloquent ORM included with Laravel provides a beautiful, simple ActiveRecord implementation for working with your database. Each database table has a corresponding `Model` which is used to interact with that table. Models allow you to query for data in your tables, as well as insert new records into the table.

## Q7: What are Laravel events?

Laravel event provides a simple `observer pattern` implementation, that allow to subscribe and listen for events in the application. An event is an incident or occurrence detected and handled by the program.

Below are some events examples in Laravel:

- A new user has registered
- A new comment is posted
- User login/logout
- New product is added.

## Q8: What do you know about query builder in Laravel?

Laravel's database query builder provides a convenient, fluent interface to creating and running database queries. It can be used to perform most database operations in your application and works on all supported database systems.

The Laravel query builder uses PDO parameter binding to protect your application against SQL injection attacks. There is no need to clean strings being passed as bindings.

Some QB features:

- Chunking
- Aggregates
- Selects
- Raw Expressions
- Joins
- Unions
- Where
- Ordering, Grouping, Limit, & Offset

## Q9: How do you generate migrations?

**Migrations** are like version control for your database, allowing your team to easily modify and share the application's database schema. Migrations are typically paired with Laravel's schema builder to easily build your application's database schema.

To create a migration, use the `make:migration` Artisan command:

```php
php artisan make:migration create_users_table
```

The new migration will be placed in your `database/migrations` directory. Each migration file name contains a timestamp which allows Laravel to determine the order of the migrations.

## Q10: How do you mock a static facade methods?

Facades provide a "static" interface to classes that are available in the application's service container. Unlike traditional static method calls, facades may be mocked. We can mock the call to the static facade method by using the `shouldReceive` method, which will return an instance of a `Mockery` mock.

```php
// actual code
$value = Cache::get('key');

// testing
Cache::shouldReceive('get')
                    ->once()
                    ->with('key')
                    ->andReturn('value');
```

## Q11: What is the benefit of eager loading, when do you use it?

When accessing Eloquent relationships as properties, the relationship data is "lazy loaded". This means the relationship data is not actually loaded until you first access the property. However, Eloquent can "eager load" relationships at the time you query the parent model.

Eager loading alleviates the `N + 1` query problem when we have nested objects (like books -> author). We can use eager loading to reduce this operation to just 2 queries.

## Q12: How do you do soft deletes?

Scopes allow you to easily re-use query logic in your models. To define a scope, simply prefix a model method with scope:

```php
class User extends Model {
    public function scopePopular($query)
    {
        return $query->where('votes', '>', 100);
    }

    public function scopeWomen($query)
    {
        return $query->whereGender('W');
    }
}
```

Usage:

```php
$users = User::popular()->women()->orderBy('created_at')->get();
```

Sometimes you may wish to define a scope that accepts parameters. Dynamic scopes accept query parameters:

```php
class User extends Model {
    public function scopeOfType($query, $type)
    {
        return $query->whereType($type);
    }
}
```

Usage:

```php
$users = User::ofType('member')->get();
```

## Q13: What are named routes in Laravel?

Named routes allow referring to routes when generating redirects or Url’s more comfortably. You can specify named routes by chaining the name method onto the route definition:

```php
Route::get('user/profile', function () {
    //
})->name('profile');
```

You can specify route names for controller actions:

```php
Route::get('user/profile', 'UserController@showProfile')->name('profile');
```

Once you have assigned a name to your routes, you may use the route's name when generating URLs or redirects via the global route function:

```php
// Generating URLs...
$url = route('profile');

// Generating Redirects...
return redirect()->route('profile');
```

## Q14: What is Closure in Laravel?

A Closure is an anonymous function. Closures are often used as callback methods and can be used as a parameter in a function.

```php
function handle(Closure $closure) {
    $closure('Hello World!');
}

handle(function($value){
    echo $value;
});
```

## Q15: List some Aggregates methods provided by query builder in Laravel ?

Aggregate function is a function where the values of multiple rows are grouped together as input on certain criteria to form a single value of more significant meaning or measurements such as a set, a bag or a list.

Below is list of some Aggregates methods provided by Laravel query builder:

- count()

```php
$products = DB::table(‘products’)->count();
```

- max()

```php
$price = DB::table(‘orders’)->max(‘price’);
```

- min()

```php
$price = DB::table(‘orders’)->min(‘price’);
```

- avg()

```php
*$price = DB::table(‘orders’)->avg(‘price’);
```

- sum()

```php
$price = DB::table(‘orders’)->sum(‘price’);
```

## Q16: What is reverse routing in Laravel?

In Laravel reverse routing is generating URL’s based on route declarations.Reverse routing makes your application so much more flexible. For example the below route declaration tells Laravel to execute the action “login” in the users controller when the request’s URI is ‘login’.

[http://mysite.com/login](http://mysite.com/login)

```php
Route::get(‘login’, ‘users@login’);
```

Using reverse routing we can create a link to it and pass in any parameters that we have defined. Optional parameters, if not supplied, are removed from the generated link.

```php
{{ HTML::link_to_action('users@login') }}
```

It will create a link like [http://mysite.com/login](http://mysite.com/login) in view.

## Q17: Why do we need Traits in Laravel?

**Traits** have been added to PHP for a very simple reason: PHP does not support multiple inheritance. Simply put, a class cannot extends more than on class at a time. This becomes laborious when you need functionality declared in two different classes that are used by other classes as well, and the result is that you would have to repeat code in order to get the job done without tangling yourself up in a mist of cobwebs.

Enter traits. These allow us to declare a type of class that contains methods that can be reused. Better still, their methods can be directly injected into any class you use, and you can use multiple traits in the same class. Let's look at a simple Hello World example.

```php
trait SayHello
{
    private function hello()
    {
        return "Hello ";
    }

    private function world()
    {
        return "World";
    }
}

trait Talk
{
    private function speak()
    {
        echo $this->hello() . $this->world();
    }
}

class HelloWorld
{
    use SayHello;
    use Talk;

    public function __construct()
    {
        $this->speak();
    }
}

$message = new HelloWorld(); // returns "Hello World";
```

## Q18: What is Autoloader in PHP?

Autoloaders define ways to automatically include PHP classes in your code without having to use statements like require and include.

- **PSR-4** would allow for simpler folder structures, but would prevent us from knowing the exact path of a class just by looking at the fully qualified name.
- **PSR-0** on the other hand is chaotic on the hard drive, but supports developers who are stuck in the past (the underscore-in-class-name users) and helps us discern the location of a class just by looking at its name.

## Q19: Let's create Enumerations for PHP. Prove some code examples.

And what if our code require more validation of enumeration constants and values?

**Answer:**
Depending upon use case, I would normally use something simple like the following:

```php
abstract class DaysOfWeek
{
    const Sunday = 0;
    const Monday = 1;
    // etc.
}

$today = DaysOfWeek::Sunday;
```

Here's an expanded example which may better serve a much wider range of cases:

```php
abstract class BasicEnum {
    private static $constCacheArray = NULL;

    private static function getConstants() {
        if (self::$constCacheArray == NULL) {
            self::$constCacheArray = [];
        }
        $calledClass = get_called_class();
        if (!array_key_exists($calledClass, self::$constCacheArray)) {
            $reflect = new ReflectionClass($calledClass);
            self::$constCacheArray[$calledClass] = $reflect - > getConstants();
        }
        return self::$constCacheArray[$calledClass];
    }

    public static function isValidName($name, $strict = false) {
        $constants = self::getConstants();

        if ($strict) {
            return array_key_exists($name, $constants);
        }

        $keys = array_map('strtolower', array_keys($constants));
        return in_array(strtolower($name), $keys);
    }

    public static function isValidValue($value, $strict = true) {
        $values = array_values(self::getConstants());
        return in_array($value, $values, $strict);
    }
}
```

And we could use it as:

```php
abstract class DaysOfWeek extends BasicEnum {
    const Sunday = 0;
    const Monday = 1;
    const Tuesday = 2;
    const Wednesday = 3;
    const Thursday = 4;
    const Friday = 5;
    const Saturday = 6;
}

DaysOfWeek::isValidName('Humpday');                  // false
DaysOfWeek::isValidName('Monday');                   // true
DaysOfWeek::isValidName('monday');                   // true
DaysOfWeek::isValidName('monday', $strict = true);   // false
DaysOfWeek::isValidName(0);                          // false

DaysOfWeek::isValidValue(0);                         // true
DaysOfWeek::isValidValue(5);                         // true
DaysOfWeek::isValidValue(7);                         // false
DaysOfWeek::isValidValue('Friday');                  // false
```

## Q20: What is autoloading classes in PHP?

With autoloaders, PHP allows the last chance to load the class or interface before it fails with an error.

The `spl_autoload_register()` function in PHP can register any number of autoloaders, enable classes and interfaces to autoload even if they are undefined.

```php
spl_autoload_register(function ($classname) {
    include  $classname . '.php';
});
$object  = new Class1();
$object2 = new Class2();
```

In the above example we do not need to include Class1.php and Class2.php. The `spl_autoload_register()` function will automatically load Class1.php and Class2.php.

## Q21: Does PHP support method overloading?

**Method overloading** is the phenomenon of using same method name with different signature. You cannot overload PHP functions. Function signatures are based only on their names and do not include argument lists, so you cannot have two functions with the same name.

You can, however, declare a **variadic function** that takes in a variable number of arguments. You would use func_num_args() and func_get_arg() to get the arguments passed, and use them normally.

```php
function myFunc() {
    for ($i = 0; $i < func_num_args(); $i++) {
        printf("Argument %d: %s\n", $i, func_get_arg($i));
    }
}

/*
Argument 0: a
Argument 1: 2
Argument 2: 3.5
*/
myFunc('a', 2, 3.5);
```

## Q22: What does yield mean in PHP?

Explain this code and what the yield does:

```php
function a($items) {
    foreach ($items as $item) {
        yield $item + 1;
    }
}
```

**Answer:**
The `yield` keyword returns data from a generator function. A generator function is effectively a more compact and efficient way to write an Iterator. It allows you to define a function that will calculate and return values while you are looping over it.

So the function in the question is almost the same as this one without:

```php
function b($items) {
    $result = [];
    foreach ($items as $item) {
        $result[] = $item + 1;
    }
    return $result;
}
```

With only one difference that `a()` returns a `generator` and`b()` just a simple `array`. You can iterate on both.

The generator version of the function does not allocate a full array and is therefore less memory-demanding. Generators can be used to work around memory limits. Because generators compute their yielded values only on demand, they are useful for representing sequences that would be expensive or impossible to compute at once.

## Q23: What does a \$\$\$ mean in PHP?

A syntax such as `$$variable` is called Variable Variable. Let's give the `$$$` a try:

```php
$real_variable = 'test';
$name = 'real_variable'; // variable variable for real variable
$name_of_name = 'name'; // variable variable for variable variable

echo $name_of_name . '<br />';
echo $$name_of_name . '<br />';
echo $$$name_of_name . '<br />';
```

And here's the output :

name
real_variable
test
