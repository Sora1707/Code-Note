# `Middleware`

### Purpose

-   Access to `request` and `response`

### Usage

#### Class

```typescript
import { Injectable, NestMiddleware } from "@nestjs/common";
import { Request, Response, NextFunction } from "express";

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
    use(req: Request, res: Response, next: NextFunction) {
        next();
    }
}
```

#### Function

```typescript
export function logger(req: Request, res: Response, next: NextFunction) {
    next();
}
```

### Apply Middleware

```typescript
import { NestModule, MiddlewareConsumer } from "@nestjs/common";
@Module({
    imports: [UsersModule],
})
export class AppModule implements NestModule {
    configure(consumer: MiddlewareConsumer) {
        // using class
        consumer.apply(LoggerMiddleware).forRoutes("users");
        // using function
        consumer
            .apply(logger)
            .forRoutes({ path: "cats", method: RequestMethod.GET })
            .exclude({ path: "cats", method: RequestMethod.GET });
    }
}
```

-   `apply()`: Middleware class(es)/function(s)
-   `forRoutes()`: `string(s)`, `RouteInfo`, Controller class(es)

### Global middleware

```typescript
app.use(logger, LoggerMiddleware);
```

-   **NOTE**: Global middleware cannot read DI container
