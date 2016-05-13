# Conditions
ACF allows fields to be visible or hidden based on the values of other fields, but only based on a field that has choices: checkbox, radio, select, true_false. These conditions work exactly like the [field group locations](location) work, being comprised by and / or logic, where `or` takes precedence to `and`.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$colors = new FieldsBuilder('colors');
$colors
    ->addRadio('color')
        ->addChoices('red', 'blue', 'green', 'other')
    ->addText('other_value')
        ->conditional('color', '==', 'other');
```
The above will show a text box if 'other' is selected from color, otherwise it will not be displayed in the admin.

## Complex Logic
Fields can be made visible or hidden based on more complex logic. The same rules apply as for [field group locations](location#complex-logic)
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner');
$banner
    ->addImage('background_image')
    ->addTrueFalse('add_title')
    ->addText('title')
        ->condition('add_title', '==', '1')
    ->addRadio('title_color')
        ->addChoices('black', 'white')
        ->condition('add_title', '==', '1')
    ->addRadio('title_background_color')
        ->addChoices(['transparent' => 'none'], 'black')
        ->condition('add_title', '==', '1')
            ->and('title_color', '==', 'white');
```
This will display the Title Background Options only if a title is available and the title text color is white.  The logic probably won't be as complex as field group locations logic, but it is there if needed.