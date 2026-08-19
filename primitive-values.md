# Primitive values by MDN (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures)
All types except Object define immutable values represented directly at the lowest level of the language. We refer to values of these types as primitive values.

All primitive types, except null, can be tested by the typeof operator. typeof null returns "object", so one has to use === null to test for null.

All primitive types, except null and undefined, have their corresponding object wrapper types, which provide useful methods for working with the primitive values. 
For example, the Number object provides methods like toExponential(). When a property is accessed on a primitive value, 
JavaScript automatically wraps the value into the corresponding wrapper object and accesses the property on the object instead. 
However, accessing a property on null or undefined throws a TypeError exception, which necessitates the introduction of the optional chaining operator.

## Primitives vs. Objects
In JavaScript, any value is either a Primitive or an Object.
## Meaning of "Immutable Values"

* Cannot be changed: Once a primitive value is created, it cannot be altered.
* Overwritten, not modified: If you change a string variable from "cat" to "dog", you did not change the letters of "cat". You threw away "cat" and created a brand new string "dog".
* Objects are mutable: By contrast, you can change the properties of an existing object without creating a new one.

## Meaning of "Lowest Level of the Language"

* Hardcoded in memory: Primitives are the most basic building blocks of the language.
* No hidden structure: They are not made of other values. A number like 5 is just a raw binary number in your computer's memory.
* Objects are complex: Objects are structures that hold collections of these basic primitive values.

## The 7 Primitive Types
If a value is not an Object, it must be one of these 7 primitive types:

* Number: 42 or 3.14
* String: "Hello"
* Boolean: true or false
* Null: null (intentional emptiness)
* Undefined: undefined (unassigned variable)
* Symbol: Unique identifiers
* BigInt: Very large integers

## Summary Example

let name = "Alex"; 
name.toUpperCase(); 
console.log(name); // Still outputs "Alex"

The code above outputs "Alex" because strings are primitive. The .toUpperCase() method cannot mutate the original string; it can only return a brand new one.
To help clarify this concept, would you like to see a code comparison showing how changing a primitive behaves differently from changing an object?

## Quick Reference Comparison

| Concept | Definition | Examples |
| :--- | :--- | :--- |
| **Primitive Type** | The structural blueprint or classification label. | `Number`, `String`, `Boolean` |
| **Primitive Value** | The actual immutable raw data instance. | `5`, `"Alex"`, `true` |
| **Object** | A mutable collection of key-value properties. | `{ name: "Alex", age: 30 }` |

