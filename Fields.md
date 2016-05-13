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
    ->addTrueFalse('fixed', [
        'instructions' => "Check to add a parallax effect where the background image doesn't move when scrolling"
      ])
    ->addColorPicker('background_color', ['default' => '#ffffff']);
```