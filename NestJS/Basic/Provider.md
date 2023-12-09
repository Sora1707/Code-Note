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

### Custom Provider

```ts
const provider = {
    provide: UsersService,
    provide: 'CONFIG', // get via @Inject(...)

    useValue: someValue,
    useClass: UsersService,

    useFactory: (arg1, arg2,...) => {
        return provider;
    }
    inject: [arg1, arg2], // list of arguments to pass into Factory
},
```

#### Export

```ts
exports: ["CONFIG"]; // string
exports: [UserService]; // class name
exports: [provider]; // whole object
```
