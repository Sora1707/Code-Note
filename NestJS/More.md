# Note

-   `Middleware` is **_unaware_** of the **_execution context_**

-   Flow: Middleware → Guard → Interceptor → Pipe → Controller → Interceptor

-   Dependencies are resolved **_bottom up_**: A depends on B → resolve B then A

-   `Record<T, K>` = `{[x: T]: K}`
