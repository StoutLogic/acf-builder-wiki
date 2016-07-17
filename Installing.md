# Installing

### Requirements
PHP 5.4 or later are required

### Composer
Easiest way to install ACF Builder is to use composer. Read more about [composer](https://getcomposer.org/doc/00-intro.md) and how to [utilize it with WordPress](http://composer.rarst.net).

```
composer require stoutlogic/acf-builder
```

### Manual
You can manually install ACF Builder, if you checkout or copy it to your theme or plugin. Be sure to require autoload.php in your functions.php file so that the classes will be autoloaded.
```php
require_once('path/to/acf-builder/autoload.php');
```