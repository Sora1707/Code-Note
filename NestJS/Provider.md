# `Provider` (Injected Dependency)

### Purpose

-   Services
-   Respositories
-   Factories
-   Helpers

### Usage: `@Injectable()`

-   Define a service as a class that have some functionality (accessing database...)
-   Inject into `@Controller`
    ```typescript
    constructor(private service: Service) {}
    ```

#### Property-based injection

-   Class **_inherits_** other classes may need some providers but passing through `super()` is **_tedious_**
-   `@Inject()` at **_property level_**
