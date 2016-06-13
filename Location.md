# Location
Field groups won't do any good if they don't belong to any locations. If you don't set a location, the field group won't appear anywhere.

To change this default configuration, use the `FieldsBuilders->setLocation($param, $operation, $value)` method.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner')
$banner
    ->addText('title')
    ->addWysiwyg('content')
    ->setLocation('post_type', '==', 'page')
```
This will make the banner field group appear on all pages.

## Complex Logic
We can do some more complex logic, if we have multiple conditions of where a field group should appear. To add more rules, use the `and` and `or` methods. `or` is higher in the order of operations than `and` is, so the logic should be structured as: 
```
(condition 1 and condition 2) or (condition 3 and condition 4)
```
In order to express:
```
(condition 2) and (condition 1 or condition 3)
``` 
The common condition must be distributed and be expressed as:
```
(condition 1 and condition 2) or (condition 3 and condition 2)
```

This is due to [the way ACF stores conditions](https://www.advancedcustomfields.com/resources/register-fields-via-php/#group-settings) in a 3 dimensional array.

```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner')

$banner
    ->addText('title')
    ->addWysiwyg('content')
    ->setLocation('post_type', '==', 'page')
        ->or('post_type', '==', 'post')
        ->and('post', '!=', '10')
    ->addText('subtitle');
```
This logic says that the banner should appear on a page or a post, and that post can't have a post id of 10. Also notice that `setLocation` doesn't have to be added last.
`$banner->build` will generate:
```php
...,
'fields' => [
    [
        'name' => 'title',
        ...
    ],
    [
        'name' => 'content',
        ...
    ],
    [
        'name' => 'subtitle',
        ...
    ],
],
'location' => [
    [
        [
            'param' => 'post_type',
            'operator'  =>  '==',
            'value' => 'page',
        ],
    ],
    [
        [
            'param' => 'post_type',
            'operator'  =>  '==',
            'value' => 'post',
        ],
        [
            'param' => 'post',
            'operator'  =>  '!=',
            'value' => '10',
        ],
    ],
]
...
```

## Modify Locations
If you have a builder already constructed with a location set and you need to add another or condition, use the `getLocation` method
```php
$banner
    ->getLocation()
    ->or('post_type', '==', 'team_member');
```
This will add the banner to all team_member post types as well.
