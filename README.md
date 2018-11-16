# AnyClone

A CSharp library that can deep clone any object using only reflection.

No requirements for [Serializable] attributes, supports standard ignore attributes.

## Description

I built this library as almost all others I tried on complex objects either didn't work at all, or failed to account for common scenarios. Serialization required too much boiler plate (BinarySerialization, Protobuf, or Json.Net) and fails to account for various designs. Implementing IClonable was too much of a chore and should be unnecessary. Various projects that use expression trees also failed to work.

## Usage

```
var originalObject = new SomeComplexTypeWithDeepStructure();
var myClonedObject = originalObject.Clone();
```

### Capture Errors
```
var originalObject = new SomeComplexTypeWithDeepStructure();
// capture errors found with your object where impossible situations occur, and add [IgnoreDataMember] to those properties/fields.
var myClonedObject = originalObject.Clone((ex, path, property, obj) => {
  Console.WriteLine($"Cloning error: {path} {ex.Message}");
  Assert.Fail();
});
```
There are unfortunately some situations that can't be resolved, such as cloning delegates, events etc. Fortunately, and it most scenarios you don't want these anyways so you can use any of the standard supported attributes to ignore these properties: `[IgnoreDataMember]`, `[JsonIgnore]`, and `[NonSerialized]` (fields only, just use `[IgnoreDataMember]` and save yourself the hassle).