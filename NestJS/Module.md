# `Module` (Organizer)

### Usage `@Module`

Module is **_singleton_** by default and every module is a **_shared_** module

-   `providers`: shared across this module (encapsulation)
-   `controllers`
-   `imports`: providers exported by other modules (used in this module)
-   `exports`: providers for other modules

-   `@Global`: global modules

-   Module classes cannot be injected as providers due to **_circular dependency_**.

### Dynamic/Cusomizable `Module`

-   Register and Configure providers dynamically
