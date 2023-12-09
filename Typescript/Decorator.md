# Metadata

-   Data that provides information about other data
-   A label that describes a piece of data,

# Decorator

### Reference

-   [Typescript decorator](https://stackoverflow.com/questions/29775830/how-to-implement-a-typescript-decorator)

### General

-   Decorators are called when the **_class_** is declared — **_not_** when an **_object_** is instantiated.
-   Decorators are not allowed on **_constructors_**.
-   **Decorator Factory** is simply a function that returns the expression (function) that will be called by the decorator at runtime.
-   multiple decorators &#8594; composition in math

    -   `@f @g x`: f(g(x))

-   Syntax:

    ```typescript
    function decorator(
        target: Object,
        propertyKey: string,
        descriptor: TypedPropertyDescriptor<any>
    ) {}
    ```

### Class Decorator

-   Applied to the `constructor` and its parameters
-   Observer, modify or replace
-   If the class decorator returns a value, it will replace the class declaration with the provided constructor function.

```typescript
function test(constructor: Function) {
    // no need to return
}
```

#### Example

##### Helper

```typescript
function Helper(...helpers: Function[]) {
    return function (constructor: Function) {
        for (const helper of helpers) {
            constructor.prototype[helper.name] = helper;
        }
    };
}

function sayHello() {
    console.log("Hello");
}

@Helper(sayHello)
class User {}

const a = new User();
a["sayHello"]();
```

#### Singleton

### Method Decorator

-   return `descriptor`

#### Without arguments

-   Assign the method to a variable
-   Use the `descriptor` to override the `value` to a new method
-   In this method, add some code before and after the call of the old method using `apply(this, args)`
-   `this`: the class through `obj.method(args)`
-   `Modify` > `Overwrite` &#8594; multiple decorators that edit the descriptor without overwriting what another decorator did

```typescript
function log(
    target: Object,
    propertyKey: string,
    descriptor: TypedPropertyDescriptor<any>
) {
    const originalMethod = descriptor.value;
    // Modify the old method with the new one
    descriptor.value = function (...args: any[]) {
        console.log("Before");
        const result = originalMethod.apply(this, args);
        console.log("After");
        return result;
    };

    return descriptor;
}
```

#### With arguments (Decorator Factory)

-   Put the args in the decorator function
-   Return the common function

```typescript
function Roles(roles: Role[]) {
    return function (
        target: Object,
        propertyKey: string,
        descriptor: TypedPropertyDescriptor<any>
    ) {
        // Do something
        return descriptor;
    };
}
```

### Static Method Decorator

-   Its `target` parameter is the constructor function and not the prototype.
-   The `descriptor` is defined on the constructor function and not the prototype.

### Class

-   `target`: The class the decorator is declared on (`TFunction extends Function`).

### Property Decorator

-   `target`: The `prototype` of the class (Object).
-   `propertyKey`: The name of the property (`string` | `symbol`).

```typescript
function prefix(prefix: string) {
    return function (target: Object, key: string | symbol) {
        let value = target[key];
        Object.defineProperty(target, key, {
            configurable: true,
            enumerable: true,
            set(newValue) {
                value = newValue;
            },
            get() {
                return `${prefix} ${value}`;
            },
        });
    };
}
```

### Parameter Decorator

-   `target`: The prototype of the class
-   `propertyKey`: The name of the method (`string` | `symbol`).
-   `parameterIndex`: The index of parameter in the list of the function's parameters (`number`).
