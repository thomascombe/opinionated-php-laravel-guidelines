# Opinionated PHP and Laravel guideline

> This project is an opinionated guideline for PHP and Laravel.

**PHP Version >= 7.4**

Table of Contents
=================

* [Opinionated PHP and Laravel guideline](#opinionated-php-and-laravel-guideline)
    * [PHP Guideline](#php-guideline)
        * [Class organization](#class-organization)
        * [Methods/classes length](#methodsclasses-length)
        * [Variables naming](#variables-naming)
        * [Type hinted](#type-hinted)
        * [Void return](#void-return)
        * [PHPDoc](#phpdoc)
        * [Traits use order](#traits-use-order)
        * [If statement](#if-statement)
        * [Function path, happy end](#function-path-happy-end)
    * [Laravel Guideline](#laravel-guideline)
    * [License](#license)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)

## PHP Guideline

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
Classes and methods should be as small as possible.  
Methods must have only one responsibility, 20 lines is probably a good soft limit.  

Example for responsibility:
```php
// User.php class

// Good
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

// Bad
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
Variables name need to explain you code, code is auto-documented with naming.
You have to avoid generic names.  

```php
// This code is just an illustration for variables naming.  
// do not judge the usefulness and effectiveness of this code 🙏
// Good
public function getPrettyUsersList(array $users): string 
{
    $prettyUsersList = '';
    
    foreach ($users as $user) {
        $prettyUsersList .= ', ' . $user->getFullName();
    }
    
    return $prettyUsersList;
}

// Bad
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

### Type hinted

Use type hinted for parameters and return.

```php
// Good
class MyClass 
{
    public function myMethod(string $param): void 
    {
        //
    }
}

// Bad
class MyClass 
{
    public function myMethod($param) 
    {
        //
    }
}
```

### Void return

Use `void` return type as soon is needed to remove mistake.

```php
// Good
class MyClass 
{
    public function myMethod(): void 
    {
        //
    }
}

// Bad
class MyClass 
{
    public function myMethod() 
    {
        //
    }
}
```

### PHPDoc

PHPDoc is for the documentation, so, use type hinted instead of `@param` or `@return`

```php
// Good
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

// Bad
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

### Traits use order

If you use PHP traits (and you have to use it!), each apply trait need to have his own line.  
It will be more readable, and when you add new apply trait the commit diff is more readable.

```php
// Good
class MyClass 
{
    use FirstTrait;
    use SecondTrait;
    use SecondTrait {
        SecondTrait::traitMethod as duplicateTraitMethod;
    }
}

// Bad
class MyClass 
{
    use FirstTrait, SecondTrait {
        SecondTrait::traitMethod as duplicateTraitMethod;
    }
}
```

### If statement

You need to always use curly brackets for if statement even if single instruction.

```php
// Good 
if ($someCondition === true) {
    $this->work();
}

// Very bad
if ($someCondition === true) {$this->work();}
```

### Function path, happy end

In function, you have to put error check (return, exception) first, this last instruction must be a happy end!

```php
// Good 
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

// Bad
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

## Laravel Guideline

// WIP

## License

[MIT](LICENSE)