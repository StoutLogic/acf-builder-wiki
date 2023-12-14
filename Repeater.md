# Repeater Fields
One of ACF's more powerful features is a repeater field. A repeater allows an admin to create 1 to N number of entities comprised of a sub-group of fields. [Read more about repeaters](https://www.advancedcustomfields.com/resources/repeater/) on ACF's website. Repeaters generate a lot of ACF config code, and using ACF Builder can reduce the amount of config written dramatically.

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
        'key' => 'field_slider_slides',
        'name' => 'slides',
        'label' => 'Slides',
        'type' => 'repeater',
        'sub_fields' => [
            [
                'key' => 'field_slider_slides_title',
                'name' => 'title',
                'type' => 'text',
            ],
            [
                'key' => 'field_slider_slides_content',
                'name' => 'content',
                'type' => 'wysiwyg',
            ],
        ],
    ],
],
...

```
## Repeater Field Settings
Repeater fields have a few settings that it might be worth using in order to provide a better admin experience.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$slider = new FieldsBuilder('slider');
$slider
    ->addText('title')
    ->addRepeater('slides', [
            'min' => 1,
            'max' => 7,
            'button_label' => 'Add Slide',
            'layout' => 'block',
            'collapsed' => 'title',
          ])
        ->addText('title')
        ->addWysiwyg('content');
```
This will set a minimum number of slides to 1, (which will automatically create the first slide for you, so the user doesn't have to click the 'Add Slide' button) A maximum number of 7 slides. By default, the add button for a repeater is 'Add Row', but here it is set to 'Add Slide'. This is useful if you have multiple repeaters or buttons in the admin, allowing the admin to more easily keep track of where they are. There is also a `layout` setting that can be set to `table`, `block` or `row`. `table` is selected by default. If you have a lot of sub fields, changing it from table would be advised as it try to fit all the fields on a single table row.

## Repeater in a Repeater
ACF allows repeaters to be embedded inside of repeaters for infinite levels. Probably not a good idea to go more than 2 or 3, if absolutely necessary, for usability sake. To embed a repeater in a repeater, it works just as one might imagine.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$slider = new FieldsBuilder('slider');
$slider
    ->addText('title')
    ->addRepeater('slides', ['min' => 1, 'button_label' => 'Add Slide'])
        ->addText('title')
        ->addRepeater('columns', ['min' => 1, 'max' => 2, 'button_label' => 'Add Column'])
            ->addWysiwyg('content')
            ->endRepeater()
        ->addImage('background_image');
```
Here there is the ability to add 1 or 2 columns of content within each slide. Notice the use of `endRepeater()` which will tell the builder to stop adding fields to the repeater when chaining methods together. Essentially allowing the chain to traverse back up a level.
