# Composing Fields
A powerful ability that ACF Builder has is the ability to compose field groups from other field groups. This isn't something that is achievable using ACF's admin UI or even php or json exports. It is crucial for maintaining consistent admin UIs with the least amount of configuration as possible. Field groups can be built like blocks.

## Write once use everywhere 
A common use case for composing fields is to create a FieldsBuilder that never gets used by itself, but can be used in other FieldsBuilder. Background Settings is a good example of this. Pages can consist of banners, sliders, sections, columns, etc. Each of these potentially could have a configurable background.

```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$backgroundSettings = new FieldsBuilder('background_settings');
$backgroundSettings
    ->addImage('background_image')
    ->addTrueFalse('background_image_fixed')
    ->addColorPicker('background_color');

/**
 * Columns has multiple columns with individual backgrounds, 
 * and the entire columns section can have a background 
 */
$columns = new FieldsBuilder('columns');
$columns
    ->addTab('Columns')
    ->addRepeater(columns, ['min' => 1, 'max' => 3])
        ->addTab('Content')
            ->addWysiwyg('content')
        ->addTab('Background')
            ->addFields($backgroundSettings)
        ->endRepeater()

     ->addTab('Background')
         ->addFields($backgroundSettings);

/**
 * A Banner on top of every page 
 */
$banner = new FieldsBuilder('banner');
$banner
    ->addRepeater('slides', ['min' => 1])
        ->addFields($columns)
    ->setLocation('post_type', '==', 'page');

/**
 * Sections go below a banner, additional banners can be added as a section
 */
$sections = new FieldsBuilder('sections')
$sections
    ->addFlexibleContent('sections')
       ->addLayout($banner)
       ->addLayout($columns)
    ->setLocation('post_type', '==', 'page');
```

## Mutability
One thing to keep in mind is that the FieldsBuilder is mutable. Meaning that when addField or setConfig is called, it changes the object in place. Calling `build` on a FieldsBuilder will not _lock in_ the fields or config. `build` only outputs a config array for the FieldsBuilder at that particular moment in time. So the array is _locked in_ but the FieldsBuilder is not.

When adding a FieldsBuilder to another FieldsBuilder with `addFields` or `addLayout`, that FieldsBuilder is cloned, which will _lock in_ the fields and configs. So updating those fields at a later time on the original FieldsBuilder will not have an effect on the cloned versions. These cloned copies embedded in other FieldsBuilder can still be updated separately, so they are still mutable.