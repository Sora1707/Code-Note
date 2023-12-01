# Class

-   can be defined in two ways: a class expression or a class declaration.
-   `class User {}` and `const User = class {}`

-   `Class.prototype.value`: for instance
-   `Class.value`: static members

### Static members

-   Only accessible using `Class name`
-   static methods and accessors are installed on the class itself.

### Constructor

-   A class has **_no more than one_** constructor method.
-   special method for creating and initializing an object created with a class.
-   Property of `prototype`

```javascript
const o = []; // Array/Object/Number
Object.hasOwn(o, "constructor"); // false
Object.hasOwn(Object.getPrototypeOf(o), "constructor"); // true
```
