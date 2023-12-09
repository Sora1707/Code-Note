# `Guard` (Auth)

### Purpose

-   Whether to **_handle_** a request by a route handler

### Usage `@UseGuards()`

#### Auth

```typescript
import { Observable } from "rxjs";

@Injectable()
export class AuthGuard implements CanActivate {
    canActivate(
        context: ExecutionContext
    ): boolean | Promise<boolean> | Observable<boolean> {
        const httpContext = context.switchToHttp();
        const request = httpContext.getRequest();
        // validateRequest is written by us
        return validateRequest(request);
    }
}
```

-   `ExecutionContext` extends `ArgumentHost`
