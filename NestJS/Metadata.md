# `Metadata` (Decorator)

### `Reflector`

#### Declare

```typescript
import { Reflector } from "@nestjs/core";

// @Roles(string[])
// Ex: @Roles(['admin'])
export const Roles = Reflector.createDecorator<string[]>();
```

#### Get

```typescript
/* Guard */

// INJECT a Reflector
constructor(private reflector: Reflector) {}

// GET (handler)
const roles = this.reflector.get(Roles, context.getHandler());

// GET (controller)
const roles = this.reflector.get(Roles, context.getClass());

// Using get<T> for better practice

// OVERRIDE when applying for both controller and handler
const roles = this.reflector.getAllAndOverride(Roles, [context.getHandler(), context.getClass()]);
/* Question: is the position in the array important? */

// MERGE when applying for both controller and handler
const roles = this.reflector.getAllAndMerge(Roles, [context.getHandler(), context.getClass()]);
```

### `createParamDecorator`

#### Example: `@Auth()` from request as Param Decorator

```typescript
// No param
export const Auth = createParamDecorator(
    (data: unknown, ctx: ExecutionContext) => {
        const request = ctx.switchToHttp().getRequest();
        return request.auth;
    }
);
```

```typescript
// Parama is based on the `data`
export const Auth = createParamDecorator(
    (data: string, ctx: ExecutionContext) => {
        const request = ctx.switchToHttp().getRequest();
        return data ? request.auth?[data] : sth;
    }
);
```

```typescript
findOne(@Auth() authInfo: AuthInfo);
findOne(@User('firstName') firstName: string);
```

### `SetMetadata(key, value)`

```ts
export const Roles = (...roles: Role[]) => SetMetadata(ROLE_KEY, roles);
```
