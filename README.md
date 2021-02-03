# Opinionated PHP and Laravel guideline

> This project is an opinionated guideline for PHP and Laravel.

**PHP Version >= 7.4**

## PHP Guideline

Use [PSR-1](https://www.php-fig.org/psr/psr-1/) and [PSR-2](https://www.php-fig.org/psr/psr-2/) for code style.

## Class organization

```php
<?php

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

## Type hinted

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

## Void return

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

## PHPDoc

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

## Traits use order

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

## If statement

You need to always use curly brackets for if statement even if single instruction.

```php
// Good 
if ($someCondition === true) {
    $this->work();
}

// Very bad
if ($someCondition === true) {$this->work();}
```

## Function path, happy end

In function, you have to put error check (return, exception) first, this last instruction must be a happy end!

```php
// Good 
if (!$someCondition) {
    return null;
}

$this->work();

if (!$anotherCondition) {
    return null;
}

return $this->secondWork();

// Bad
if ($someCondition) {
    $this->work();

    if ($anotherCondition) {
        return $this->secondWork();
    }
}

return null;
```

## Laravel Guideline

// WIP

## License

[MIT](LICENSE)