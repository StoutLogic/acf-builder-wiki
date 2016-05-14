# Composing Fields
A powerful ability that ACF Builder has is the ability to compose field groups from other field groups. This isn't something that is achievable using ACF's admin UI or even php or json exports. It is crucial for maintaining consistent admin UIs with the least amount of configuration as possible. Field groups can be built like blocks.


## Mutability
One thing to keep in mind is that the FieldsBuilder is mutable. Meaning that when addField or setConfig is called, it changes the object.