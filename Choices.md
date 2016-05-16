# Choices
There are a group of fields that ACF allows to have choices: checkbox, radio and select. [Check out the plethora of settings](https://www.advancedcustomfields.com/resources/register-fields-via-php/#field-type%20settings) ACF allows for these types of fields. To make managing the choices easier, ACF Builder provides `addChoice` and `addChoices` methods.

## Add Choice
After adding the checkbox, radio or select field, you can use `addChoice` to clearly setup the choice options.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner');
$banner
    ->addText('title')
    ->addWysiwyg('content')
    ->addRadio('background_color')
        ->addChoice('red')
        ->addChoice('blue')
        ->addChoice('green')
        ->defaultValue('blue');
```
`$banner->build();` will generate:
```php
...
 'fields' => [
    [
        ...
        'name' => 'background_color',
        'type' => 'radio',
        'choices' => [
            'red' => 'red',
            'blue' => 'blue',
            'green' => 'green',
        ],
        'default_value' => 'blue',
    ],
],
...
```
`addChoice` can take a second parameter for the label of the radio button. By default it will be the same as the value.
```php
$banner
    ->addRadio('background_color')
    ->addChoice('yellow', 'Yellow');
```
## Shorthand for adding multiple choices
Another option is to use the `addChoices` method to quickly add 1 or more choices. Just pass in multiple arguments for each choice. Or pass in a [key => value] array if you want to specify a label.
```php
$banner
    ->addRadio('background_color')
    ->addChoices('green', 'orange', ['rose' => 'pink'], 'white');
```