# Flexible Content
Flexible content is like a repeater but with the ability to choose from different sub field groups, or as ACF calls them, layouts. [Check out ACFs Flexible Content documentation](https://www.advancedcustomfields.com/resources/flexible-content/). In ACF Builder, use the `addFlexibleContent` method to create a Flexible Content field, and then call `addLayout` for each layout you want to create.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$content = new FieldsBuilder('page_content');
$content
    ->addFlexibleContent('sections')
        ->addLayout('banner')
            ->addText('title')
            ->addWysiwyg('content')
        ->addLayout('content_columns')
            ->addRepeater('columns', ['min' => 1, 'max' => 2])
                ->addWysiwyg('content');
```
Flexible Contents `"Add Row"` button can be customized, by passing in a `button_label` key/val argument in the second parameter. By default, ACF Builder will singularize the Flexible Content name. So in the above example, the button `"Add Section"` would automatically be generated if a `button` argument wasn't manually passed in.

Adding a layout is essentially the same as adding an entire FieldGroup, but you can pass some additional arguments such as `min`, `max` and `display`, like a repeater. `display` will default to `"block"`. 

Internally ACF Builder creates a new FieldsBuilder to manage each layout. To encourage reuse, `addLayout` also accepts a FieldsBuilder object.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner');
$banner
    ->addText('title')
    ->addWysiwyg('content');

$columns = new FieldsBuilder('content_columns');
$columns
    ->addRepeater('columns', ['min' => 1, 'max' => 2])
        ->addWysiwyg('content');

$content = new FieldsBuilder('page_content');
$content
    ->addFlexibleContent('sections')
        ->addLayout($banner)
        ->addLayout($columns);
```
This generates the same exact config as the first example, but you have the ability to reuse `$banner` and `$columns` else where in other FlexibleContent fields or as stand alone field groups. See the documentation on [Composing Fields](composing-fields) for me information on the subject.

## FlexibleContent in FlexibleContent
[Just like repeaters](repeater#repeater-in-a-repeater), you can embed FlexibleContent field within each other. And just like repeaters, there is an `endFlexibleContent` method to stop adding fields to the chain, and effectively traverse back up the tree of fields.

```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$content = new FieldsBuilder('page_content');
$content
    ->addFlexibleContent('sections')
        ->addLayout('content_columns')
            ->addRepeater('columns', ['min' => 1, 'max' => 2])
                ->addFlexibleContent('column_type', ['min' => 1, 'max' => 1])
                    ->addLayout('copy')
                        ->addWysiwyg('content')
                    ->addLayout('image')
                        ->addImage('image')
                    ->endFlexibleContent()
                ->endRepeater()
        ->addLayout('banner')
            ->addText('title')
            ->addWysiwyg('content')
```
