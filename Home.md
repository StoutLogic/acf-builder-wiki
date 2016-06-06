# ACF Builder Documentation
[ACF Pro](https://www.advancedcustomfields.com/pro/) is an indispensable tool for creating powerful admin interfaces for WordPress. Best $100 AUD one could ever spend. It even has a great admin interface for creating the custom fields. While the admin interface is great for beginners, as you start to add more developers, source control, and more and more fields, it starts to become a little cumbersome. ACF has the ability to [register fields directly in your php code](https://www.advancedcustomfields.com/resources/register-fields-via-php/). It also describes all the default value for each field. The main problem is the array syntax used can be very verbose making it hard to get a birds eye view, and can be error prone. This is what the ACF Builder hopes to solve, along with making fields groups composable of other field groups.

## Sample
Say we wanted to create a banner field group with a title, content editor and background image. And we wanted to make this field group available on all pages and posts. We could do the following:
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
`$banner->build();` will return:
```php
[
    'key' => 'group_banner',
    'title' => 'Banner',
    'fields' => [
        [
            'key' => 'field_title',
            'name' => 'title',
            'label' => 'Title',
            'type' => 'text'
        ],
        [
            'key' => 'field_content',
            'name' => 'content',
            'label' => 'Content',
            'type' => 'wysiwyg'
        ],
        [
            'key' => 'field_background_image',
            'name' => 'background_image',
            'label' => 'Background Image',
            'type' => 'image'
        ],
    ],
    'location' => [
        [
            [
                'param' => 'post_type',
                'operator' => '==',
                'value' => 'page'
            ]
        ],
        [
            [
                'param' => 'post_type',
                'operator' => '==',
                'value' => 'post'
            ]
        ]
    ]
]
```
That's almost a 75% reduction in characters. Here are a few things to note:
* `->build()` will generate the array config on the fly
* At any time you can add to the field group and call `->build()` again to regenerate the array config with your updates
* `->addField()` makes some convienient yet opinionated assumptions on what to put for some config values
* You can chain `->addField()` function calls for convenience, since they return an instance of the builder, indent for your own readability