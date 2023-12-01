# Symbol

-   Unique value (Common)
-   Global symbols (equal): `Symbol.for(string)`
-   Identifiers for properties of an Object
-   Hidden property not appear in `Object.getOwnPropertyNames()`
-   Get the symbol through `Object.getOwnPropertySymbols()`
-   May look the same using `console.log()`

# Object

### `prototype` and `__proto__`

-   `class Person` and `person = Person()`: `person.__proto__ === Person.prototype`
-   When you try to access a property on an object, JavaScript will **first look for the property on the object itself**.
-   If the property is not found on the object itself, JavaScript will **then look for the property on the object's prototype**.
-   This process will continue until the property is found or until the top of the prototype chain is reached.

### `assign(src, ...value)`

-   Override & add property
-   Only copies enumerable and own properties from a source object to a target object

### `create(proto, propertiesObject)`

-   creates a new object, using an existing object as the prototype

### `defineProperty(obj, prop, descriptor)`

-   `Prop`: defined/modified property
-   `Descriptor` must be one of these two
    -   **Data descriptor**: property with a value that may or may not be writable
    -   **Accessor descriptor**: property described by a getter-setter pair of functions
    -   Options: `configurable`, `enumerable`, `value`, `writable`, `get`, `set`
    -   `enumerable`: cannot be accesed through `for...in` or `Object.keys()` or **spread** operator
    -   `configurable`: deletable? + change value through `defineProperty()`
-   This operation prevents javascript to look for the property in `prototype`
-   `defineProperty(Class.prototype)`: the property will be inherited by all instances
-   `defineProperty(Class)`: only in the constructor, no inherit

```javascript
function MyClass() {}
Object.defineProperty(MyClass.prototype, "x", {
    get() {
        return this.storedX;
    },
    set(x) {
        this.storedX = x;
    },
});
const a = new MyClass();
const b = new MyClass();
a.x = 1;
b.x; // undefined
```

### `preventExtensions(obj)`

-   Allowed to modify only existing properties
-   No new property

### `freeze(obj)`

-   Prevent extensions
-   Makes existing properties non-writable and non-configurable (no convert data from accessor props to data props)

### `seal(obj)`

-   Prevent extensions
-   Makes existing properties non-configurable
