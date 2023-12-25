# `Exception` Handling

### `throw`

```cpp
throw Exception; // anything

throw "Error";
throw 401;
throw Error(); // a class
```

### `catch(Error)`

-   `Error` and `Exception` must have the **_same type_** or
-   `Error` is the **_base class_** of `Exception`

#### Default catch

```cpp
catch(...); // it really is `...`
```
