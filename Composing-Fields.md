# Composing Fields
A powerful ability that ACF Builder has is the ability to compose field groups from other field groups. This isn't something that is achievable using ACF's admin UI or even php or json exports. It is crucial for maintaining consistent admin UIs with the least amount of configuration as possible. Field groups can be built like blocks.

## Write once use everywhere 
A common use case for composing fields is to create a FieldsBuilder that never gets used by itself, but can be used in other FieldsBuilder. Background Settings is a good example of this. Pages can consist of banners, sliders, sections, columns, etc. Each of these potentially could have a configurable background.

The `addFields` method will take a `FieldsBuilder` as an argument, and add the fields from that `FieldsBuilder` to another `FieldsBuilder`.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$backgroundSettings = new FieldsBuilder('background_settings');
$backgroundSettings
    ->addImage('background_image')
    ->addTrueFalse('background_image_fixed')
    ->addColorPicker('background_color');

/**
 * Columns fields have multiple columns with individual backgrounds, 
 * and then the entire columns section can have a background.
 */
$columns = new FieldsBuilder('columns');
$columns
    ->addTab('Columns')
        ->addRepeater('columns', ['min' => 1, 'max' => 3, 'layout' => 'block'])
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
    ->addRepeater('slides', ['min' => 1, 'layout' => 'block'])
        ->addFields($columns)
    ->setLocation('post_type', '==', 'page');

/**
 * Sections go below a banner, additional banners can be added as a section.
 * Columns can also be added as a section. These layouts can be used on the
 * default template.
 */
$sections = new FieldsBuilder('sections')
$sections
    ->addFlexibleContent('sections')
       ->addLayout($banner)
       ->addLayout($columns)
    ->setLocation('page_template', '==', 'default');

/**
 * About template has a special Team Members sections only for the about page. 
 * It will list all the team members. It has an intro section and has a
 * configurable background.
 */
$aboutSections = new FieldsBuilder('about_sections')
$aboutSections
    ->addFlexibleContent('sections')
       ->addLayout($banner)
       ->addLayout($columns)
       ->addLayout('team_members')
           ->addTab('Intro')
               ->addText('title')
               ->addWysiwyg('intro')
           ->addTab('Background')
               ->addFields($background_settings)
    ->setLocation('page_template', '==', 'about');
```

## Mutability
One thing to keep in mind is that the FieldsBuilder is mutable. Meaning that when addField or setConfig is called, it changes the object in place. Calling `build` on a FieldsBuilder will not _lock in_ the fields or config. `build` only outputs a config array for the FieldsBuilder at that particular moment in time. So the array is _locked in_ but the FieldsBuilder is not.

When adding a FieldsBuilder to another FieldsBuilder with `addFields` or `addLayout`, that FieldsBuilder is cloned, which will _lock in_ the fields and configs. So updating those fields at a later time on the original FieldsBuilder will not have an effect on the cloned versions. These cloned copies embedded in other FieldsBuilder can still be updated separately, so they are still mutable.

## Generated Keys
In ACF, a Field's key needs to be unique. Keys are used in the admin forms, as the form field names. If two form field have the same name, only one field (the one appearing last) gets saved.

ACF solves this issue when using the UI by randomly generating the keys so they are unique. ACF Builder solves this by namespacing a field's key by its parent field groups.

For example:
```php
$builder = new FieldsBuilder('page_content');
$builder->addFlexibleContent('sections')
              ->addLayout('banner')
                  ->addText('title')
                  ->addWysiwyg('content')
              ->addLayout('content_columns')
                  ->addRepeater('columns', ['min' => 1, 'max' => 2])
                      ->addWysiwyg('content');
```

The group's key will become: `group_page_content`.
The flexible content areas `sections` key will become: `field_page_content_sections`
The layout banner key will become: `field_page_content_sections_banner`
And the title field's key in the banner will become: `field_page_content_sections_banner_title`
The last field in the above example will have the key: `field_page_content_sections_content_columns_columns_content`

This provides a logical semantic naming scheme for keys. They will be unique because the names of fields need to be unique. It mirrors how ACF stores fields name in the database.

A user of ACF Builder shouldn't have to worry about key names, but if the user ever needs to know what the key for a particular field is, they should be able to logically deduce it.