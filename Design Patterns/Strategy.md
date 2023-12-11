# Strategy Pattern (Task oriented)

**EXTENSION**

### Composition > Inheritance

-   Composition: **HAS-A** relationship
-   Inheritance (**IS-A**) may lead to **_redundant_** methods/properties when sharing across generations
-   Event if we're using **interface**, each subclass still has to implement its own version

### Solution

-   Separate the parts of your code that will **_change the most_** from the rest of your application and try to make them as **_freestanding_** as possible
-   The implementation of the votalile ones are called `algorithms` or `strategies`

### Algorithms

-   One algorithm, one purpose

#### Base Interface: Family of algorithms

```cpp
class RunAlgorithm {
public:
    virtual void do() = 0;
}
```

#### Multiple algorithms

```cpp
class RunAlgorithm01: public RunAlgorithm {
public:
    void do() {}
}
```

#### Use algorithms

```cpp
class Object {
private:
    RunAlgorithm runAlgorithm;
public:
    void setAlgorithm(Algorithm algorithm);
    run() {
        runAlgorithm.run();
    }
}
```

### Pros

-   Change at **_runtime_**
-   Algorithms vary independently from clients that use it
