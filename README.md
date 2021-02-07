# Opinionated PHP and Laravel guidelines

> This project is an opinionated guidelines for PHP and Laravel.

You can find PHP guideline and more specific Laravel guidelines.  
These guidelines contain **my** PHP best practices and also **my** [Laravel](https://laravel.com/) best practices.  

**PHP Version >= 7.4**

Table of Contents
=================

* [Opinionated PHP and Laravel guidelines](#opinionated-php-and-laravel-guidelines)
* [Table of Contents](#table-of-contents)
    * [PHP Guidelines](#php-guidelines)
        * [Class organization](#class-organization)
        * [Methods/classes length](#methodsclasses-length)
        * [Variables naming](#variables-naming)
        * [Type hinting](#type-hinting)
        * [Void return](#void-return)
        * [PHPDoc](#phpdoc)
        * [Traits use](#traits-use)
        * [If statement](#if-statement)
        * [String concatenation](#string-concatenation)
        * [Function path, happy end](#function-path-happy-end)
    * [Laravel Guidelines](#laravel-guidelines)
    * [License](#license)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)

## PHP Guidelines

Use [PSR-1](https://www.php-fig.org/psr/psr-1/) and [PSR-2](https://www.php-fig.org/psr/psr-2/) for code style.

### Class organization

```php
namespace My\Class\Namespace;

use ClassInterface;

class MyClass extends ParentClass implements ClassInterface, Serializable
{
    public const PUBLIC_CONSTANTS = 'publicConstants';
    protected const PROTECTED_CONSTANTS = 'protectedConstants';
    private const PRIVATE_CONSTANTS = 'privateConstants';
    
    use SomeTrait;
    use AnotherTrait;
    
    public $publicProperties;
    protected $protectedProperties;
    private $privateProperties;
    
    public function __constructor() {}
    public function __destruct() {}
    
    public function publicMethods() {}
    protected function protectedMethods() {}
    private function privateMethods() {}
}
```

### Methods/classes length
Classes and methods SHOULD be as small as possible.  
Methods MUST have only one responsibility, 20 lines is probably a good soft limit.  

Example for single responsibility:
```php
// User.php class

// Good ✅
public function getSummaryDescription(): string 
{
    return sprintf(
        '%s, %d years old, %s',
        $this->getFullName(),
        $this->getAge(),
        $this->getFullAddress()
    );
}

public function getFullName(): string 
{
    //return ...;
}

public function getAge(): int 
{
    //return ...;
}

public function getFullAddress(): string 
{
    //return ...;
}

// Bad ❌
public function getSummaryDescription(): string 
{
    return sprintf(
        '%s %s %s, %d years old, %s %s %s',
        $this->civility,
        ucfirst($this->firstName),
        strtoupper($this->lastName),
        intval($this->age),
        $this->address,
        $this->postalCode,
        $this->city,
    );
}
```

### Variables naming
Variables name NEED to explain your code, code is auto-documented with naming.
You HAVE TO avoid generic names.  

```php
// This code is just an illustration for variables naming.  
// do not judge the usefulness and effectiveness of this code 🙏
// Good ✅
public function getPrettyUsersList(array $users): string 
{
    $prettyUsersList = '';
    
    foreach ($users as $user) {
        $prettyUsersList .= ', ' . $user->getFullName();
    }
    
    return $prettyUsersList;
}

// Bad ❌
// getUsersList name is not description the method
public function getUsersList(array $users): string 
{
    // Variable name is not understandable
    $list = '';
    
    // $item is too generic
    foreach ($users as $item) {
        $list .= ', ' . $item->getFullName();
    }
    
    return $list;
}
```

### Type hinting

You SHOULD use type hinting for parameters and return.

```php
// Good ✅
class MyClass 
{
    public function myMethod(string $param): void 
    {
        //
    }
}

// Bad ❌
class MyClass 
{
    public function myMethod($param) 
    {
        //
    }
}
```

### Void return

Use `void` as return type whenever possible to avoid mistakes.

```php
// Good ✅
class MyClass 
{
    public function myMethod(): void 
    {
        //
    }
}

// Bad ❌
class MyClass 
{
    public function myMethod() 
    {
        //
    }
}
```

### PHPDoc

PhpDoc stands for documentation purposes. Consider to use type hinting insteadof `@param` or `@return`

```php
// Good ✅
class MyClass 
{
    /**
     * Method description if method name can't be understandable
     */
    public function myMethod(string $param): void 
    {
        //
    }
}

// Bad ❌
class MyClass 
{
    /** 
     * @param string $param 
     * @return void 
     */
    public function myMethod(string $param): void 
    {
        //
    }
}
```

### Traits use

If you use PHP traits (and you have to use it!), for better readability and more understandable diffs, each trait SHOULD be on its own line.

```php
// Good ✅
class MyClass 
{
    use FirstTrait;
    use SecondTrait;
    use SecondTrait {
        SecondTrait::traitMethod as duplicateTraitMethod;
    }
}

// Bad ❌
class MyClass 
{
    use FirstTrait, SecondTrait {
        SecondTrait::traitMethod as duplicateTraitMethod;
    }
}
```

### If statement

You MUST use curly braces for if statements even if there is only one line of code inside.  

```php
// Good ✅
if ($someCondition === true) {
    $this->work();
}

// Very bad ❌
if ($someCondition === true) {$this->work();}
```

### String concatenation

For concatenate strings prefer In-string variables.  
You can also use `sprintf()` for more complex use cases.

```php
// Very good ✅
$finalString = "My name is {$name}!";

// Good ✅
$finalString = sprintf(
    'My name is %s!',
    $name
);

// Bad ❌
$finalString = 'My name is ' .$name . '!';

// If you want to update spring
$finalString = "My name is {$name}! I'm {$age} years old";

// Good ✅
$finalString = sprintf(
    'My name is %s!I\'m %d years old',
    $name,
    $age
);
// Bad ❌
$finalString = 'My name is ' .$name . '! I\'m ' . $age . ' years old';
```

### Function path, happy end

In function, you SHOULD put error check (return, exception) first, this last instruction must be a happy end!

```php
// Good ✅
private function good(): ?int 
{
    if (!$someCondition) {
        return null;
    }
    
    $this->work();
    
    if (!$anotherCondition) {
        return null;
    }
    
    
    return $this->secondWork();
}

// Bad ❌
private function bad(): ?int 
{
    if ($someCondition) {
        $this->work();
    
        if ($anotherCondition) {
            return $this->secondWork();
        }
    }
    
    return null;
}
```
This rule keep code readable and remove a lot of nested methods.

## Laravel Guidelines

// WIP

## License

[MIT](LICENSE)