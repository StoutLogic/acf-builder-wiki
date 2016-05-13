# Tabs
Tabs are a great way to create a concise and usable admin UI. Creating a tab with ACF Builder is as easy as adding any other type of field.
```php
use StoutLogic\AcfBuilder\FieldsBuilder;

$banner = new FieldsBuilder('banner');
$banner
    ->addTab('Content')
    ->addText('title')
    ->addWysiwyg('content')
    
    ->addTab('Background Settings')
    ->addImage('background_image')
    ->addColorPicker('background_color');
```
`addTab('Content')` will generate:
```php
[
    'key' => 'field_content_tab',
    'name' => 'content_tab',
    'label' => 'Content',
    'type' => 'tab'
]
```
`addTab('Background Settings')` will generate:
```php
[
    'key' => 'field_background_settings_tab',
    'name' => 'background_settings_tab',
    'label' => 'Background Settings',
    'type' => 'tab'
]
```
The `name` and `key` will have `'_tab'` appended their values.

ACF provides a [placement](https://www.advancedcustomfields.com/resources/tab/#settings) setting for 'top' (default) or 'left'. Note 'left' will be ignored if the field group is configured with a table layout.

ACF also provides an `endpoint` setting to denote that a tab is the start of a new row or group of tabs. If the field group has a lot of tabs this may be helpful. It can either be passed in as an option like placement, or the `endPoint()` function can be called after `addTab()`.
```php
$banner
   ->addTab('Content')
   ...
   ->addTab('Settings')
       ->endPoint()
   ...
```