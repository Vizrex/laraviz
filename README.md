> # READ THIS FIRST
> It is highly recommended for all contributors to update this file whenever there's a major update in source code. Use [this tool](https://stackedit.io/app#) for easy editing or [visit this page](https://help.github.com/articles/basic-writing-and-formatting-syntax/) for comprehensive guide on markdown syntax.

# Introduction
This is base package with various helpers and abstract classes, acting as a foundation and a standard for other packages. It is highly recommended to build future packages and projects on top of this package.

# Included Items
## Base Classes
### BaseCommand
A base class for Console commands of Laravel. It contains three new methods:
- `debug()`
This method calls `info()` of base class and outputs the given string in `VERY_VERBOSE` mode i.e. the given string will be displayed on console only when `-vvvv` flag will be passed to a command.
- `setNamespace()`
This is an abstract method. It should set `$this->namespace` property of command. Ideally its value should be taken from static `getNamespace()` method of Service Provider class as implemented in `BaseServiceProvider` and its child classes.
- `setDefaultTranslationFile()`
This method sets the `$defaultTranslationFile` property of command. It is the name of file from which the localized strings should be loaded by default.
- `str()`
This is a helper method that calls `__()` or `trans()` method to load localized strings based upon the values of `$namespace` and `$defaultTranslationFile` property of command. However, it is overloaded to accept a translations file name other than `$defaultTranslationFile`.

### BaseException
### BaseController
### BaseServiceProvider
It has a static method `getNamespace()` that returns the namespace of this service provider in a specific format.
### BaseRepository
It has the implementation of basic methods related to a resource:
- `index()`
- `find()`
- `store()`
- `update`
- `destroy()`

### BaseMigration
This class can be extended by Laravel migration classes. Child class would need to implement following three methods:
- `__construct()`
It should call `parent::__construct("my_table")` where `my_table` should be replaced with appropriate table name. It is highly recommended to get this string by calling `getTableName()` of a model for which this migration is intended.

- `doChanges()`
This method will get an object of `Blueprint`. Developer can write the schema related statements straightforwardly without worrying about existence and non-existence of table. Use it as substitute of traditional `up()` method. However `up()` method can also be overridden if required.
- `undoChanges()`
This function gets the table name as parameter. Use it as substitute of traditional `down()` method. However the later can still be overridden when required.

### BaseSeeder
### BaseTrait: LaravizModel
Use this trait in Eloquent Models and override `findByIdentifier()` method. There can be more than one identifiers of a model e.g. a user can be identified by its `id` as well as by `email` attribute. So instead of writing `orWhere` clauses, `findByIdentifier()` method can be called neatly.

### Base Resource
This class has following two additional methods as compared to default `Resource` of Laravel:
- `toCompactArray`
- `toFullArray`

There're two properties of this class:
#### Static: `$defaultResponseType`
Default response type can be set with this property (default is `ResponseType::FULL`)

#### Non-static: `$responseType`
This property overrides the `$defaultResponseType` and can be set only via constructor.


# Default Routes
Following open routes will be added to the application using this package:
### `/time`
It returns current datetime with timezone details as JSON string.

### `/info`
It returns the output of `phpinfo()`

These routes can be customized using config/laraviz.php file. Execute the following command to publish this config file:

``` bash
php artisan vendor:publish --tag=Vizrex_Laraviz
```

# Added Helpers
## Eloquent Helpers
### `get_fillables()`
It returns the fillable attributes as an associative array having `key` and `value`.

## Html Helpers
### `public_url()`
Returns base URL of application.

### `css($absolutePathOrArray)`
Adds the HTML tag for including CSS. You can pass either an string, an array or an associative array where the key should contain the URL of CSS and the value should contain an expression such that CSS would be included only when the expression yields to true.
``` php
css("http://sample-cdn.com/example.css");
css(["http://abc.com/first.css", "http://abc.com/second.css"]);
css(["http://abc.com/first.css", ["http://abc.com/second.css" => 1 > 2], "http://www.example.com/main.css"]);
```
###  `public_css($relativePath)`
Adds the HTML tag for including css relative to `public/css` folder.
### `js($absolutePathOrArray)`
Adds the HTML tag for including Javascript file. See the description of `css()` method. This method works exactly like that.
### `public_js($relativePath)`
Adds the HTML tag for including js relative to `public/js` 
## Path Helpers
### `merge_url()`
Currently it simply concatenates two string and adds a forward slash `/` between them. It can be enhanced to handle more complex situations.

## String Helpers
### `str_concat()`
This function accepts variable number of parameters and concatenates.

### `s()`
This helper method is a wrapper on `trans()` and `__()` methods of Laravel. Using this method will eliminate the need of specifying language/translations file name every time. Simply specify a default language file in `config/laraviz.php` file and only pass the `key` to `s()` method. It will simply concatenate the configured file name to the given key and pass it to `trans()` method.

## System Helpers
Following methods have very basic implementation. They're sort of thin wrappers. However there's a lot of room for enhancements
- `v_echo()`
- `v_info()`
- `v_debug`
- `v_error()`

# To-dos
Following are the approved items:
- Improve `merge_url()` to handle complex situations e.g. concatenate relative urls to create an absolute one.
- Improve `SystemHelpers`. Ideally there should be a separate package for logging and `SystemHelpers` should only be thin wrappers on top of our custom logger.

# Wishlist
Add the suggestions in this wishlist. Only approved wishlist items can be moved to To-dos list:
- Item-1