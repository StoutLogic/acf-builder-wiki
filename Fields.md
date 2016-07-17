# Fields
Fields are very easy to add to a field group. There is very general 
```php
$builder->addField($name, $config = []);
```
method that will accept a name (to be in lowercase and underscores) and an array of field configurations that can be found on the [ACF register fields via PHP page](https://www.advancedcustomfields.com/resources/register-fields-via-php/#field-settings). If you don't specify a type option, ACF will default to a normal text field.

Luckily the ACF Builder library provides a number of self descriptive shortcut functions for adding different field types. The best way to see all the available field type functions in the [FieldsBuilder.php](https://github.com/StoutLogic/acf-builder/blob/master/src/FieldsBuilder.php#L142) class file, and their options correlate to those [defined by ACF](https://www.advancedcustomfields.com/resources/register-fields-via-php/#field-settings)

If you're using a 3rd party ACF plugin that adds additional field types, you can just use the generic `addField` function and pass the field type to the `$args` array.

## Sample
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$background = new FieldsBuilder('background');
$background
    ->addImage('background_image', ['preview_size' => 'medium'])
    ->addTrueFalse('background_fixed', [
        'label' => 'Fixed',
        'instructions' => "Check to add a parallax effect where the background image doesn't move when scrolling"
      ])
    ->addColorPicker('background_color', ['default_value' => '#ffffff']);
```
`$background->build();` Will generate:
```php
[
    'key' => 'group_background',
    'title' => 'Background',
    'fields' => [
        [
            'key' => 'field_background_background_image',
            'name' => 'background_image',
            'label' => 'Background Image',
            'type' => 'image',
            'preview_size' => 'medium'
        ],
        [
            'key' => 'field_background_background_fixed',
            'name' => 'background_fixed',
            'label' => 'Fixed',
            'type' => 'true_false',
            'instructions' => "Check to add a parallax effect where the background image doesn't move when scrolling"
        ],
        [
            'key' => 'field_background_background_color',
            'name' => 'background_color',
            'label' => 'Background Color',
            'type' => 'color_picker',
            'default_value' => '#ffffff'
        ],
    ]
]
```
Notice how the `key`, `name` and `label` values are automatically populated based on the name passed into the field. Much like the [FieldGroups](../Field-Groups#acf-builder-field-group-defaults) generates defaults based on the name. `key` will prepend `key_` and the field group's name 'background_', `name` will be the same, and `label` will get title cased and the underscores replaced with spaces.

The `type` setting is automatically generated based on the `addField` method used.

We also passed in some optional configuration arguments for each field. Including overwriting the default label for the `background_fixed` field.

Some of these configurations are very often used, so there are some declarative shortcut functions for them. They are:
* `defaultValue($value)`
* `required($value = true)` true by default, pass in false to unrequire it
* `instructions($value)`

The above can be rewritten to be:
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$background = new FieldsBuilder('background');
$background
    ->addImage('background_image', ['preview_size' => 'medium'])
    ->addTrueFalse('background_fixed', ['label' => 'Fixed'])
        ->instructions("Check to add a parallax effect where the background image doesn't move when scrolling")
    ->addColorPicker('background_color')
        ->defaultValue('#ffffff');
```
Any field configuration in this style using `setConfig($key, $value)` This is useful if the field was already declared and you wanted to change the configuration.