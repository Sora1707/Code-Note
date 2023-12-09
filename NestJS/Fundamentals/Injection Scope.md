# `Injection` Scope

```ts
@Injectable({ scope: Scope.SCOPE })
@Controller({ scope: Scope.SCOPE })
```

-   `DEFAULT`: singleton
-   `REQUEST`: created in each request
-   `TRANSIENT`: new injection new instance

### Scope hierachy

-   A class has a `request`-scoped dependency is also `request`-scoped
    -   Example: Controller ← Service (request)

### Durable Providers

-   1 Request-scoped provider can make thr controller request-scoped

#### Multi-tenant

-   Customers with dedicated data source
-   Declare a request-scoped "data source" provider based on the request object
