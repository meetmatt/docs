# Filter Object
The filter object used to perform complex data validation and filtration using PSR-7 or any other input. You 
can create filter manually or using scaffolder `php app.php create:filter my`:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA    = [  
    ];
    
    protected const VALIDATES = [
    ];
    
    protected const SETTERS   = [       
    ];
}
```

Use `Spiral\Filter\FilterProviderInterface`->`createFilter` or simply request filter dependency to create an instance:

```php
use App\Filter\MyFilter;

class HomeController
{
    public function index(MyFilter $filter)
    {
        dump($filter);
    }
}
```

## Filter Schema
The core of any filter object is `SCHEMA`, this constant defines mapping between fields and values provided by input.
Every key pair is defined as `field` => `source:origin` or `field` => `source`. The **source** is the subset of data from
user input, in HTTP scope the sources can be `cookie`, `data`, `query`, `input` (`data`+`query`), `header`, `file`,
`server`. The **origin** is the name of external field, dot notation is supported.

> You can use any input bag from [InputManager](/http/request-response.md) as source.

For example we can tell our filter to point field `login` to the QUERY param `username`:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'login' => 'query:username'
    ];
}
```

You can combine multiple source inside the filter:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'redirectTo'   => 'query:redirectURL',
        'memberCookie' => 'cookie:memberCookie',
        'username'     => 'data:username',
        'password'     => 'data:password',
        'rememberMe'   => 'data:rememberMe'
    ];
}
```

> The most common source is `data` (points to PSR-7 - parsed body), you can use this data to fetch values from incoming
> json payloads. 

### Dot Notation
The data **origin** can be specified using dot notation pointing to some nested structure. For example:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'firstName' => 'data:name.first'
    ];

    protected const VALIDATES = [
        'firstName' => [
            ['notEmpty']
        ]
    ];

    protected const SETTERS = [
        'firstName' => 'strval'
    ];
}
```

We can accept and validate the following data structure:

```json
{
  "names": {
    "first": "Antony" 
  }
}
```

> The error messages will be properly mounted into original location. You can also use composite filters for more complex
> use-cases.

### Other Sources
By design you can use any method of `[InputManager](/http/request-response.md)` as source where origin is passed
parameter. Following sources are available:

Source | Description
--- | ---
uri | Current page Uri in a form of `Psr\Http\Message\UriInterface`
path | Current page path
method | Http method (GET|POST|...)
isSecure | If https used.
isAjax | If `X-Requested-With` set as `xmlhttprequest`
isJsonExpected | When client expects `application/json`
remoteAddress | User ip address

> Read more about InputManager [here](/v1.0.0/httphttp/input.md).

For example to check if request is made over https:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'httpsRequest' => 'isSecure'
    ];

    protected const VALIDATES = [
        'httpsRequest' => [
            ['notEmpty', 'error' => 'Connection is not secure.']
        ]
    ];
}
```

> Read more about validation below. 

### Setters
Use setters to typecast the incoming value before passing it to validator. The filter will assign null to the value
in case of typecast error:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'number' => 'query:number'
    ];

    protected const VALIDATES = [
        'number' => [
            ['notEmpty'],
            ['number::higher', 5]
        ]
    ];

    protected const SETTERS = [
        'number' => 'intval'
    ];
}
```

> You can use any of the default PHP functions like `intval`, `strval` and etc.

```php
namespace App\Controller;

use App\Filter\MyFilter;

class HomeController
{
    public function index(MyFilter $filter)
    {
        dump($filter->number); // always int
    }
}
```

## Validation
The validation rules can be defined using same approach as in [validation](/security/validation.md) component.

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'name' => 'data:name'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty']
        ]
    ];
}
```

You can use all the checkers, conditions and rules.

### Custom Errors
You can specify custom error message to any of the rule similar way as in validation component.

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'name' => 'data:name'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty', 'error' => 'Name must not be empty']
        ]
    ];
}
```

If you plan to later localize error message to another language wrap the text in `[[]]` to automatically index and
replace the translation:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'name' => 'data:name'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty', 'error' => '[[Name must not be empty]]']
        ]
    ];
}
```

## Usage
Once the filter is configured you can access it's fields (filtered data), check if the data valid and return the set
of errors in case of failure.

> Use [Domain Core Interceptors](/cookbook/domain-core.md) to validate your filters before they will arrive to the 
> controller.

### Get Fields
To get filtered list of fields use methods `getField` and `getFields`. For the filter like that:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'name'  => 'data:name',
        'email' => 'data:email'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty']
        ]
    ];

    protected const SETTERS = [

    ];
}
```

Following fields are available:

```php
public function index(MyFilter $filter)
{
    dump($filter->getFields()); // {name: ..., email: ...}

    dump($filter->getField('name'));

    // same as above
    dump($filter->email);
}
```

### Get Errors
To check if filter is valid use `isValid`, list of field errors is available via `getErrors`:

```php
public function index(MyFilter $filter)
{
    if (!$filter->isValid()) {
        dump($filter->getErrors());
    }
}
```

The errors will be automatically mapped to origin property name, for example:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class MyFilter extends Filter
{
    protected const SCHEMA = [
        'name' => 'data:names.name.0'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty']
        ]
    ];

    protected const SETTERS = [

    ];
}
```

Will produce the following error if field `name` is invalid:

```json
{
  "status": 400,
  "errors": {
    "names": {
      "name": "This value is required."
    } 
  } 
}
```

> The error format is identical to one described in [validation](/security/validation.md).

## Inheritance 
You can extend one filter from another, the schema, validation and setters will be inherited:

```php
namespace App\Filter;

class MyFilter extends BaseFilter
{
    protected const SCHEMA = [
        'name' => 'data:name'
    ];

    protected const VALIDATES = [
        'name' => [
            ['notEmpty']
        ]
    ];
}
```

Where `BaseFilter` is:

```php
namespace App\Filter;

use Spiral\Filters\Filter;

class BaseFilter extends Filter
{
    protected const SCHEMA = [
        'token' => 'data:token'
    ];

    protected const VALIDATES = [
        'token' => [
            ['notEmpty']
        ]
    ];
}
```

Now filter `MyFilter` will require `token` value as well:

```json
{
  "status": 400,
  "errors": {
    "token": "This value is required.",
    "name": "This value is required."
  }
}
```