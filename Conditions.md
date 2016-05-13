# Conditions
ACF allows fields to be visible or hidden based on the values of other fields. These conditions work exactly like the [field group locations](location) work, being comprised by and / or logic, where `or` takes precedence to `and`.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$builder = new FieldsBuilder('fields');
$builder
    ->addRadio('color')
        ->addChoices('red', 'blue', 'green', 'other')
    ->addText('other_value')
        ->conditional('color', '==', 'other');
```
The above will show a text box if 'other' is selected from color, otherwise it will not be displayed in the admin.