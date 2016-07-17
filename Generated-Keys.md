# Generated Keys
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

**Note:** if composing fields with `addFields` see [this](composing-fields#generated-keys).