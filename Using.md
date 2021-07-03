# Using ACF Builder

The quickest way to use ACF Builder is to require the autoload.php file (Composer's auto generated vendor/autoload.php or ACF Builder's custom autoload.php file) in your functions.php code. If your theme is set up to use composer, no require is needed.

## Creating Field Groups
Create new FieldsBuilder objects to create ACF groups, and `setLocation` to where you want them to show up in the admin.

## Register Field Groups
Use ACF's built in actions to register the field group config with ACF.
```php
$banner = new StoutLogic\AcfBuilder\FieldsBuilder('banner');
$banner
    ->addText('title')
    ->addWysiwyg('content')
    ->addImage('background_image')
    ->setLocation('post_type', '==', 'page')
        ->or('post_type', '==', 'post');

add_action('acf/init', function() use ($banner) {
   acf_add_local_field_group($banner->build());
});
```

## Saving to Admin Panel
Adding fields via the `acf_add_local_field_group` will mean they show up where you specified in `setLocation`, you cannot see them on the Custom Fields panel within the WordPress back-end. 

A workaround for this is simply by creating a /acf-json/ directory in the root of your theme ([more here](https://www.advancedcustomfields.com/resources/local-json/)) and using `acf_write_json_field_group` instead. This will save the new fields to a .json file that you can then sync from the Custom Fields panel.