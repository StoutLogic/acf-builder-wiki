# Composing Fields
A powerful ability that ACF Builder has is the ability to compose field groups from other field groups. This isn't something that is achievable using ACF's admin UI or even php or json exports. It is crucial for maintaining consistent admin UIs with the least amount of configuration as possible. Field groups can be built like blocks.


## Mutability
One thing to keep in mind is that the FieldsBuilder is mutable. Meaning that when addField or setConfig is called, it changes the object in place. Calling `build` on a FieldsBuilder will not _lock in_ the fields or config. `build` only outputs a config array for the FieldsBuilder at that particular moment in time. So the array is _locked in_ but the FieldsBuilder is not.

When adding a FieldsBuilder to another FieldsBuilder with `addFields` or `addLayout`, that FieldsBuilder is cloned, which will _lock in_ the fields and configs. So updating those fields at a later time on the original FieldsBuilder will not have an effect on the cloned versions. These cloned copies embedded in other FieldsBuilder can still be updated separately, so they are still mutable.