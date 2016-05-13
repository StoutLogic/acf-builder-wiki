# Field Groups
A field group is represented by a `FieldsBuilder`. When you are instantiating a new `FieldsBuilder` you can pass to it any group configuration options as an array to the second optional parameter, to overwrite the defaults that the ACF Builder library imposes, or ACF itself imposes.

## ACF Builder Field Group defaults
When you create a new `FieldsBuilder`
```php
$banner = new StoutLogic\AcfBuilder\FieldsBuilder('banner');
```
here are the defaults that get generated:
```php
[
    'key' => 'group_banner',
    'title' => 'Banner',
]
```
By passing `banner` to the constructor, the key prepends `group_` to it, and the title gets title cased. If there were underscores in the passed name like `new FieldsBuilder('content_sections')` the title would be `Content Sections` and the key would be `group_content_sections`.

`key` and `title` are the only default values ACF Builder creates. Please see the [ACF Register fields via PHP](https://www.advancedcustomfields.com/resources/register-fields-via-php/#group-settings) page for the defaults that ACF itself assumes. 

## Overwriting Default Values
You can overwrite the ACF Builder default values, or ACF's own default values by passing an array of values as the second parameter in the constructor.
```php
$banner = new StoutLogic\AcfBuilder\FieldsBuilder('banner', [
    'key' => 'group_header_banner', 
    'style' => 'seamless'
]);
```
Generates:
```php
[
    'key' => 'group_header_banner',
    'title' => 'Banner',
    'style' => 'seamless',
]
```
You can also modify the group configuration after instantiating the object using the `->setGroupConfig($key, $value)` method.
```php
$banner->setGroupConfig('position', 'acf_after_title');
```
Changes the config to:

```php
[
    'key' => 'group_header_banner',
    'title' => 'Banner',
    'style' => 'seamless',
    'position' => 'acf_after_title',
]
```
The `setGroupConfig` method returns the `FieldsBuilder` instance just like the `addField` methods do, so you can use it to chain method calls together.