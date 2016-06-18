# Modifying Fields
When composing fields from other fields, some tweaks might need to be made to fit the use case. That might mean modifying the configuration of a field, adding additional fields, or even removing a field.

## Modifying a field's configuration
The simplest way to modify a field's configuration is to call `modifyField`, and passing in the name of the field to be modified, and the modified field config as an array. The following example will change the label of a title field:
```php
$fieldsBuilder
    ->modifyField('title', ['label' => 'Headline']);
```
If the field 'title' doesn't exist, a `FieldNotFoundException` will be thrown.

## Insert Fields After a Particular Field
A more powerful way to modify a field is to pass in a closure instead of an config array to `modifyField`. The following example will change the label of the title field and add a sub title field after the title field, but before the `content` field:
```php
$builder = new FieldsBuilder('Banner');
    $builder
        ->addText('title')
        ->addWysiwyg('content');

    $builder
        ->modifyField('title', function($fieldsBuilder) {
            return $fieldsBuilder
                ->setConfig('label', 'Banner Title')
                ->addText('sub_title');
        })
        ->addTextarea('footnotes');
```
The closure must except a new `FieldsBuilder` object which will have the field 'title' already initialized as the only field. From their, the field's configuration can be modified, as well as fields added after it. This function also must return a `FieldsBuilder`. The original `title` field will have its contents replaced with these built fields. 

`modifyField` will return an instance to the original builder, so additional fields can be added after `content`, in this case a `footnote`s textarea.

If the closure passed to modifyField doesn't return a `FieldsBuilder`, a `ModifyFieldReturnTypeException` will be thrown.

## Removing Fields
Simply call `removeField` and pass in that field's name.
```php
$builder
    ->removeField('title')
    ->addText('headline');
```
The previous code will remove the `title` field and then add a new `headline` field.