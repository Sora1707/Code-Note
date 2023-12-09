# Execution `Context`

### `ArgumentsHost`

-   Retrieve the arguments being passed to a handler
-   Context via `host.getType<>()`: http, rpc (microservice), graphql...

```ts
const [req, res, next] = host.getArgs();
const context = host.switchToHttp();
```

### `ExecutionContext` extends `ArgumentsHost`

```ts
/*
UsersController
@Post()
create()
*/
getClass<T>(): Type<T>; // UsersController
getHandler(): Function; // create
```
