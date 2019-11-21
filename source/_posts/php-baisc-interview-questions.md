---
title: php-baisc-interview-questions
date: 2019-11-19 11:35:34
tags: php
---

[TOC]

## Q1: What is the difference between == and ===?

- The operator == casts between two different types if they are different
- The === operator performs a 'typesafe comparison'

That means that it will only return true if both operands have the same type and the same value.

```php
1 === 1: true
1 == 1: true
1 === "1": false // 1 is an integer, "1" is a string
1 == "1": true // "1" gets casted to an integer, which is 1
"foo" === "foo": true // both operands are strings and have the same value
```

## Q2: How can you pass a variable by reference?

To be able to pass a variable by **reference**, we use an ampersand in front of it, as follows:

```php
$var1 = &$var2
```

## Q3: What does \$GLOBALS mean?

`$GLOBALS` is associative array including references to all variables which are currently defined in the global scope of the script.

## Q4: What is the use of ini_set()?

PHP allows the user to modify some of its settings mentioned in php.ini using ini_set(). This function requires two string arguments. First one is the name of the setting to be modified and the second one is the new value to be assigned to it.

Given line of code will enable the display_error setting for the script if it’s disabled.

`ini_set('display_errors', '1');`

We need to put the above statement, at the top of the script so that, the setting remains enabled till the end. Also, the values set via ini_set() are applicable, only to the current script. Thereafter, PHP will start using the original values from php.ini.

## Q5: When should I use require vs. include?

The `require()` function is identical to `include()`, except that it handles errors differently. If an error occurs, the `include()` function generates a warning, but the script will continue execution. The `require()` generates a fatal error, and the script will stop.

My suggestion is to just use `require_once` 99.9% of the time.

Using `require` or `include` instead implies that your code is not **reusable** elsewhere, i.e. that the scripts you're pulling in actually execute code instead of making available a class or some function libraries.

## Q6: What is stdClass in PHP?

`stdClass` is just a generic 'empty' class that's used when casting other types to objects. `stdClass` is **not** the base class for objects in PHP. This can be demonstrated fairly easily:

```php
class Foo{}
$foo = new Foo();
echo ($foo instanceof stdClass)?'Y':'N'; // outputs 'N'
```

It is useful for anonymous objects, dynamic properties, etc.

An easy way to consider the `StdClass` is as an alternative to associative array. See this example below that shows how `json_decode()` allows to get an `StdClass` instance or an associative array. Also but not shown in this example, `SoapClient::__soapCall` returns an `StdClass` instance.

```php
//Example with StdClass
$json = '{ "foo": "bar", "number": 42 }';
$stdInstance = json_decode($json);

echo $stdInstance - > foo.PHP_EOL; //"bar"
echo $stdInstance - > number.PHP_EOL; //42

//Example with associative array
$array = json_decode($json, true);

echo $array['foo'].PHP_EOL; //"bar"
echo $array['number'].PHP_EOL; //42
```

## Q7: What are the differences between die() and exit() functions in PHP?

There's no difference - they are the same. The only advantage of choosing die() over exit(), might be the time you spare on typing an extra letter.

## Q8: What are the main differences between const vs define

The fundamental difference between `const` vs `define` is that `const` defines constants at compile time, whereas `define` defines them at run time.

```php
const FOO = 'BAR';
define('FOO', 'BAR');

// but
if (...) {
    const FOO = 'BAR';    // Invalid
}
if (...) {
    define('FOO', 'BAR'); // Valid
}
```

Also until PHP 5.3, `const` could not be used in the global scope. You could only use this from within a class. This should be used when you want to set some kind of constant option or setting that pertains to that class. Or maybe you want to create some kind of enum. An example of good `const` usage is to get rid of magic numbers.

`Define` can be used for the same purpose, but it can only be used in the global scope. It should only be used for global settings that affect the entire application.

Unless you need any type of conditional or expressional definition, use `consts` instead of `define()`- simply for the sake of readability!

## Q9: What's the difference between isset() and array_key_exists()?

- array_key_exists will tell you if a key exists in an array and complains when \$a does not exist.
- isset will only return true if the key/variable exists and is not null. isset doesn't complain when \$a does not exist.

Consider:

```php
$a = array('key1' => 'Foo Bar', 'key2' => null);

isset($a['key1']);             // true
array_key_exists('key1', $a);  // true

isset($a['key2']);             // false
array_key_exists('key2', $a);  // true
```

## Q10: What is the difference between var_dump() and print_r()?

The `var_dump` function displays structured information about variables/expressions including its type and value. Arrays are explored recursively with values indented to show structure. It also shows which array values and object properties are references.

The `print_r()` displays information about a variable in a way that's readable by humans. array values will be presented in a format that shows keys and elements. Similar notation is used for objects.

Consider:

```php
$obj = (object) array('qualitypoint', 'technologies', 'India');
```

`var_dump($obj)`will display below output in the screen:

```php
object(stdClass)#1 (3) {
[0]=> string(12) "qualitypoint"
[1]=> string(12) "technologies"
[2]=> string(5) "India"
}
```

And, print_r(\$obj) will display below output in the screen.

```php
stdClass Object (
[0] => qualitypoint
[1] => technologies
[2] => India
)
```

## Q11: Explain what the different PHP errors are

- A `notice` is a non-critical error saying something went wrong in execution, something minor like an undefined variable.

- A `warning` is given when a more critical error like if an include() command went to retrieve a non-existent file. In both this and the error above, the script would continue.

* A `fatal` error would terminate the code. Failure to satisfy a require() would generate this type of error, for example.

## Q12: How can you enable error reporting in PHP?

Check if “`display_errors`” is equal “on” in the php.ini or declare “`ini_set('display_errors', 1)`” in your script.

Then, include “`error_reporting(E_ALL)`” in your code to display all types of error messages during the script execution.

## Q13: Declare some function with default parameter

Consider:

```php
function showMessage($hello = false){
  echo ($hello) ? 'hello' : 'bye';
}
```

## Q14: Is multiple inheritance supported in PHP?

PHP supports only single inheritance; it means that a class can be extended from only one single class using the keyword 'extended'.

## Q15: In PHP, objects are they passed by value or by reference?

In PHP, objects passed by value.

## Q16: What is the differences between $a != $b and $a !== $b?

`!=` means inequality (TRUE if $a is not equal to $b) and `!==` means non-identity (TRUE if $a is not identical to $b).

## Q17: What is PDO in PHP?

**PDO** stands for PHP Data Object.

It is a set of PHP extensions that provide a core PDO class and database, specific drivers. It provides a vendor-neutral, lightweight, data-access abstraction layer. Thus, no matter what database we use, the function to issue queries and fetch data will be same. It focuses on data access abstraction rather than database abstraction.

## Q18: Explain how we handle exceptions in PHP?

When an exception is thrown, code following the statement will not be executed, and PHP will attempt to find the first matching catch block. If an exception is not caught, a PHP Fatal Error will be issued with an "Uncaught Exception". An exception can be thrown, and caught within PHP.

To handle exceptions, code may be surrounded in a `try` block. Each try must have at least one corresponding catch block. Multiple `catch` blocks can be used to catch different classes of exceptions. Exceptions can be thrown (or re-thrown) within a catch block.

Consider:

```php
try {
    print "this is our try block n";
    throw new Exception();
} catch (Exception $e) {
    print "something went wrong, caught yah! n";
} finally {
    print "this part is always executed n";
}
```

## Q19: Differentiate between echo and print()

`echo` and `print` are more or less the same. They are both used to output data to the screen.

The differences are:

- echo has no return value while print has a return value of 1 so it can be used in expressions.
- echo can take multiple parameters (although such usage is rare) while print can take one argument.
- echo is faster than print.

## Q20: When should I use require_once vs. require?

The `require_once()` statement is identical to `require()` except PHP will check if the file has already been included, and if so, not include (require) it again.

My suggestion is to just use `require_once` 99.9% of the time.

Using `require` or `include` instead implies that your code is not **reusable** elsewhere, i.e. that the scripts you're pulling in actually execute code instead of making available a class or some function libraries.

## Q21: Check if PHP array is associative

Consider:

```php
function has_string_keys(array $array) {
  return count(array_filter(array_keys($array), 'is_string')) > 0;
}
```

If there is at least one string key, `$array` will be regarded as an associative array.

## Q22: How do I pass variables and data from PHP to JavaScript?

There are actually several approaches to do this:

- Use AJAX to get the data you need from the server.
  Consider **get-data.php**:

```php
echo json_encode(42);
```

Consider **index.html:**

```php
<script>
  function reqListener () {
    console.log(this.responseText);
  }

  var oReq = new XMLHttpRequest(); // New request object
  oReq.onload = function() {
      // This is where you handle what to do with the response.
      // The actual data is found on this.responseText
      alert(this.responseText); // Will alert: 42
  };
  oReq.open("get", "get-data.php", true);
  //                               ^ Don't block the rest of the execution.
  //                                 Don't wait until the request finishes to
  //                                 continue.
  oReq.send();
</script>
```

- Echo the data into the page somewhere, and use JavaScript to get the information from the DOM.

```php
<div id="dom-target" style="display: none;">
  <?php
      $output = "42"; // Again, do some operation, get the output.
      echo htmlspecialchars($output); /* You have to escape because the result
                                         will not be valid HTML otherwise. */
  ?>
</div>
<script>
  var div = document.getElementById("dom-target");
  var myData = div.textContent;
</script>
```

- Echo the data directly to JavaScript.

```php
<script>
  var data = <?php echo json_encode("42", JSON_HEX_TAG); ?>; // Don't forget the extra semicolon!
</script>
```

## Q23: Is there a function to make a copy of a PHP array to another?

In PHP arrays are assigned by copy, while objects are assigned by reference so PHP will copy the array by default. References in PHP have to be explicit:

```php
$a = array(1,2);
$b = $a; // $b will be a different array
$c = &$a; // $c will be a reference to $a
```

## Q24: What will be returned by this code?

Consider the code:

```php
$a = new stdClass();
$a->foo = "bar";
$b = clone $a;
var_dump($a === $b);
```

What will be echoed to the console?

**Answer:**
Two instances of the same class with equivalent members do NOT match the `===` operator. So the answer is:

```php
bool(false)
```

## Q25: What will be returned by this code? Explain the result.

Consider the code. What will be returned as a result?

```php
$something = 0;
echo ('password123' == $something) ? 'true' : 'false';
```

**Answer:**
The answer is `true`. You should never use `==` for string comparison. Even if you are comparing strings to strings, PHP will implicitly cast them to floats and do a numerical comparison if they appear numerical. `===` is OK.

For example

```php
'1e3' == '1000' // true
```

also returns true.

## Q26: What exactly is the the difference between array_map, array_walk and array_filter?

- `array_walk` takes an array and a function F and modifies it by replacing every element x with F(x).
- `array_map` does the exact same thing **except** that instead of modifying in-place it will return a new array with the transformed elements.
- `array_filter` with function F, instead of transforming the elements, will remove any elements for which F(x) **is not true**

## Q27: Explain the difference between exec() vs system() vs passthru()?

- `exec()` is for calling a system command, and perhaps dealing with the output yourself.
- `system()` is for executing a system command and immediately displaying the output - presumably text.
- `passthru()` is for executing a system command which you wish the raw return from - presumably something binary.

## Q28: How would you create a Singleton class using PHP?

```php
/**
 * Singleton class
 *
 */
final class UserFactory {
    /**
     * Call this method to get singleton
     *
     * @return UserFactory
     */
    public static
    function Instance() {
        static $inst = null;
        if ($inst === null) {
            $inst = new UserFactory();
        }
        return $inst;
    }

    /**
     * Private ctor so nobody else can instantiate it
     *
     */
    private
    function __construct() {

    }
}
```

To use:

```php
$fact = UserFactory::Instance();
$fact2 = UserFactory::Instance();
```

But:

```php
$fact = new UserFactory()
```

Throws an error.

## Q29: What is the difference between PDO's query() vs execute()?

- `query` runs a standard SQL statement and requires you to properly escape all data to avoid SQL Injections and other issues.
- `execute` runs a prepared statement which allows you to bind parameters to avoid the need to escape or quote the parameters. execute will also perform better if you are repeating a query multiple times.

Best practice is to stick with prepared statements and execute for increased security. Aside from the escaping on the client-side that it provides, a prepared statement is compiled on the server-side once, and then can be passed different parameters at each execution.

## Q30: What is use of Null Coalesce Operator?

Null coalescing operator returns its first operand if it exists and is not NULL. Otherwise it returns its second operand.

Example:

```php
$name = $firstName ?? $username ?? $placeholder ?? "Guest";
```

## Q31: Differentiate between exception and error

- Recovering from `Error` is not possible. The only solution to errors is to terminate the execution. Where as you can recover from `Exception` by using either try-catch blocks or throwing exception back to caller.
- You will not be able to handle the `Errors` using try-catch blocks. Even if you handle them using try-catch blocks, your application will not recover if they happen. On the other hand, `Exceptions` can be handled using try-catch blocks and can make program flow normal if they happen.
- `Exceptions` are related to application where as `Errors` are related to environment in which application is running.

## Q32: What are the exception class functions?

There are following functions which can be used from `Exception` class.

- `getMessage()` − message of exception
- `getCode()` − code of exception
- `getFile()` − source filename
- `getLine()` − source line
- `getTrace()` − n array of the `backtrace()`
- `getTraceAsString()` − formated string of trace
- `Exception::__toString` gives the string representation of the exception.

## Q33: Differentiate between parameterised and non parameterised functions

- **Non parameterised functions** don't take any parameter at the time of calling.
- **Parameterised functions** take one or more arguments while calling. These are used at run time of the program when output depends on dynamic values given at run time There are two ways to access the parameterised function:
  1.call by value: (here we pass the value directly )
  2.call by reference: (here we pass the address location where the value is stored)

## Q34: Explain function call by reference

In case of call by reference, actual value is modified if it is modified inside the function. In such case, we need to use `&` symbol with formal arguments. The `&` represents reference of the variable.

Example

```php
function adder(&$str2) {
    $str2 .= 'Call By Reference';
}
$str = 'This is ';
adder($str);
```

Output:

```php
This is Call By Reference
```

## Q35: Why do we use extract()?

The `extract()` function imports variables into the local symbol table from an array. This function uses array keys as variable names and values as variable values. For each element it will create a variable in the current symbol table. This function returns the number of variables extracted on success.

Example:

```php
$a = "Original";
$my_array = array("a" => "Cat","b" => "Dog", "c" => "Horse");
extract($my_array);
echo "\$a = $a; \$b = $b; \$c = $c";
```

Output:

```php
$a = Cat; $b = Dog; $c = Horse
```

## Q36: explain what is a closure in PHP and why does it use the “use” identifier?

Consider this code:

```php
public function getTotal($tax)
{
    $total = 0.00;

    $callback =
        function ($quantity, $product) use ($tax, &$total)
        {
            $pricePerItem = constant(__CLASS__ . "::PRICE_" .
                strtoupper($product));
            $total += ($pricePerItem * $quantity) * ($tax + 1.0);
        };

    array_walk($this->products, $callback);
    return round($total, 2);
}
```

Could you explain why use it?

**Answer:**
This is how PHP expresses a **closure**. Basically what this means is that you are allowing the anonymous function to "capture" local variables (in this case, `$tax` and a reference to `$total`) outside of it scope and preserve their values (or in the case of $total the reference to $total itself) as state within the anonymous function itself.

A closure is a separate namespace, normally, you can not access variables defined outside of this namespace.

- `use` allows you to access (use) the succeeding variables inside the closure.
- `use` is early binding. That means the variable values are COPIED upon DEFINING the closure. So modifying \$tax inside the closure has no external effect, unless it is a pointer, like an object is.
- You can pass in variables as pointers like in case of `&$total`. This way, modifying the value of `$total` DOES HAVE an external effect, the original variable's value changes.

## Q37: What exactly are late static bindings in PHP?

Basically, it boils down to the fact that the `self` keyword does not follow the same rules of inheritance. `self` always resolves to the class in which it is used. This means that if you make a method in a parent class and call it from a child class, `self` will not reference the child as you might expect.

Late static binding introduces a new use for the `static` keyword, which addresses this particular shortcoming. When you use static, it represents the class where you first use it, ie. it 'binds' to the runtime class.

Consider:

```php
class Car {
    public static
    function run() {
        return static::getName();
    }

    private static
    function getName() {
        return 'Car';
    }
}

class Toyota extends Car {
    public static
    function getName() {
        return 'Toyota';
    }
}

echo Car::run(); // Output: Car
echo Toyota::run(); // Output: Toyota
```

## Q38: How to measure execution times of PHP scripts?

I want to know how many milliseconds a PHP while-loop takes to execute. Could you help me?

**Answer**:
You can use the `microtime` function for this.

Consider:

```php
$start = microtime(true);
while (...) {

}
$time_elapsed_secs = microtime(true) - $start;
```

## Q39: What is the best method to merge two PHP objects?

```php
//We have this:
$objectA->a;
$objectA->b;
$objectB->c;
$objectB->d;

//We want the easiest way to get:
$objectC->a;
$objectC->b;
$objectC->c;
$objectC->d;
```

**Answer**:
This works:

```php
$obj_merged = (object) array_merge((array) $obj1, (array) $obj2);
```

You may also use `array_merge_recursive` to have a deep copy behavior.

One more way to do that is:

```php
foreach($objectA as $k => $v) $objectB->$k = $v;
```

This is faster than the first answer in PHP versions < 7 (estimated 50% faster). But in PHP >= 7 the first answer is something like 400% faster.

## Q40: Compare mysqli or PDO - what are the pros and cons?

Let's name some:

- PDO is the standard, it's what most developers will expect to use.

- Moving an application from one database to another isn't very common, but sooner or later you may find yourself working on another project using a different RDBMS. If you're at home with PDO then there will at least be one thing less to learn at that point.

- A really nice thing with PDO is you can fetch the data, injecting it automatically in an object.

* PDO has some features that help agains SQL injection

* In sense of speed of execution MySQLi wins, but unless you have a good wrapper using MySQLi, its functions dealing with prepared statements are awful. inserts - almost equal, selects - mysqli is ~2.5% faster for non-prepared statements/~6.7% faster for prepared statements.

## Q41: What is use of Spaceship Operator?

This `<=>` operator will offer combined comparison in that it will:

- Return 0 if values on either side are equal
- Return 1 if value on the left is greater
- Return -1 if the value on the right is greater

Consider:

```php
//Comparing Integers
echo 1 <= > 1; //outputs 0
echo 3 <= > 4; //outputs -1
echo 4 <= > 3; //outputs 1

//String Comparison

echo "x" <= > "x"; // 0
echo "x" <= > "y"; //-1
echo "y" <= > "x"; //1
```

## Q42: Does PHP have threading?

Standard php does not provide any multithreading but there is an (experimental) extension that actually does - pthreads. The next best thing would be to simply have one script execute another via CLI, but that's a bit rudimentary. Depending on what you are trying to do and how complex it is, this may or may not be an option.

## Q43: Is PHP single or multi threaded?

PHP is not single threaded by nature. It is, however, the case that the most common installation of PHP on unix systems is a single threaded setup, as is the most common Apache installation, and nginx doesn't have a thread based architecture whatever. In the most common Windows setup and some more advanced unix setups, PHP can and does operate multiple interpreter threads in one process.

PHP as an interpreter had support for multi-threading since the year 2000.

## Q44: Provide some ways to mimic multiple constructors in PHP

It's known you can't put two `__construct` functions with unique argument signatures in a PHP class but I'd like to do something like this:

```php
class Student
{
   protected $id;
   protected $name;
   // etc.

   public function __construct($id){
       $this->id = $id;
      // other members are still uninitialised
   }

   public function __construct($row_from_database){
       $this->id = $row_from_database->id;
       $this->name = $row_from_database->name;
       // etc.
   }
}
```

What is the best way to achieve this in PHP?

**Answer**:
I'd probably do something like this:

```php
class Student
{
    public function __construct() {
        // allocate your stuff
    }

    public static function withID( $id ) {
        $instance = new self();
        $instance->loadByID( $id );
        return $instance;
    }

    public static function withRow( array $row ) {
        $instance = new self();
        $instance->fill( $row );
        return $instance;
    }

    protected function loadByID( $id ) {
        // do query
        $row = my_awesome_db_access_stuff( $id );
        $this->fill( $row );
    }

    protected function fill( array $row ) {
        // fill all properties from array
    }
}
```

Then if i want a Student where i know the ID:

```php
$student = Student::withID( $id );
```

Technically you're not building multiple constructors, just static helper methods, but you get to avoid a lot of spaghetti code in the constructor this way.

Another way is to use **the mix of factory and fluent style:**

```php
class Student
{
    protected $firstName;
    protected $lastName;
    // etc.

    /**
     * Constructor
     */
    public function __construct() {
        // allocate your stuff
    }

    /**
     * Static constructor / factory
     */
    public static function create() {
        $instance = new self();
        return $instance;
    }

    /**
     * FirstName setter - fluent style
     */
    public function setFirstName( $firstName) {
        $this->firstName = $firstName;
        return $this;
    }

    /**
     * LastName setter - fluent style
     */
    public function setLastName( $lastName) {
        $this->lastName = $lastName;
        return $this;
    }
}

// create instance
$student= Student::create()->setFirstName("John")->setLastName("Doe");
```

Q45: How could we implement method overloading in PHP?
You cannot overload PHP functions. Function signatures are based only on their names and do not include argument lists, so you cannot have two functions with the same name.

You can, however, declare a `variadic function` that takes in a variable number of arguments. You would use `func_num_args()` and `func_get_arg()` to get the arguments passed, and use them normally.

Consider:

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
