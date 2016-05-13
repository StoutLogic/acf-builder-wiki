# Repeater Fields
One of ACF's more powerful features is a repeater field. A repeater allows an admin to create 1 to N number of entities comprised of a sub-group of fields. [Read more about repeaters](https://www.advancedcustomfields.com/resources/repeater/) on ACF's website.

```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$slider = new FieldsBuilder('slider');
$slider
    ->addText('title')
    ->addRepeater('slides')
        ->addText('title')
        ->addWysiwyg('content');
```
Here is an example of a simple slider. The slider itself has a title, and then there can be 1 to N number of slides that contain a title and content. 
`$slider->build()` returns the following:
```php
...
'fields' => [
    [
        ...,
        'name' => 'title',
        'type' => 'text',
    ],
    [
        'key' => 'field_slides',
        'name' => 'slides',
        'label' => 'Slides',
        'type' => 'repeater',
        'sub_fields' => [
            [
                ...,
                'name' => 'title',
                'type' => 'text',
            ],
            [
                ...,
                'name' => 'content',
                'type' => 'wysiwyg',
            ],
        ],
    ],
],
...

```
