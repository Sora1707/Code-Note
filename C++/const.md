# `const` keyword

### Pointer `const DataType* p`

-   Can be written as `DataType const* p`

```cpp
const int MAX = 10;
int* p = &MAX; // Error:
const int* p = &MAX; // OK
```

-   p is a pointer to a const DataType
-   p could not be used to change DataType value/object
-   p can be NULL or point to somewhere else
-   p can only call const function if it is poiting to a class

#### Note: Read from right to left

-   `int* const p`: p is a constant pointer, pointing to an int
-   `const int* p`: p is a pointer, pointing to an int, which is a constant
-   `int const* p`: p is a pointer, pointing to a constant int
-   `const int* const p`: lock p from changing the target data and p itself

### Reference

Reference is already a constant

### Member function

```cpp
data User::getData() const;
```

-   can be called by const object only (no object modification)
-   When returning a reference to a member, remember to put a const (or else this member is exposed to outside)

```cpp
const string& getName() const; // Ok
string& getName() const; // Bad
```

### `mutatble` keyword for property

-   Changeable even inside const member function
