# Module Reference (`ModuleRef`)

### Purpose

-   Obtain a reference to any `Controller`/`Injectable` using its injection token as a lookup key
-   Instantiate static and scoped providers

### Usage

#### Inject

```ts
constructor(private moduleRef: ModuleRef) {}
```

#### Retrieve

###### DEFAULT scope with `get`

```ts
moduleRef.get(Service);
// Global
moduleRef.get(Service, { strict: false });
```

###### TRANSIENT/REQUEST scope with `resolve`

`resolve()` returns a **_unique_** instance of the provider, from its own DI container sub-tree

```ts
// unique provider
await moduleRef.resolve(TransientService);
```

To retrieve the **_same instance_**, pass the same **_context id_** of the DI container sub-tree

```ts
const contextId = ContextIdFactory.create();
await moduleRef.resolve(TransientService, contextId),
```

Get the context id via `REQUEST`

```ts
constructor(
    @Inject(REQUEST) private request: Record<string, unknown>,
) {}

const contextId = ContextIdFactory.getByRequest(this.request);
```

#### Create

Dynamically instantiate a class that wasn't previously registered as a provider

```ts
this.factory = await this.moduleRef.create(Factory);
```
