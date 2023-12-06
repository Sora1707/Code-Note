# `Exception` filters

### Purpose

-   Self-defined exception filter for user-friendly response
-   Use a different JSON schema based on some dynamic factors

### Usage

#### Default: `HttpException`

```typescript
catch (error) {
    throw new HttpException(
        "Forbidden Resource"
        HttpStatus.FORBIDDEN,
        { cause: error }
    );
}
```

#### Exception Filter

```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
    catch(exception: HttpException, host: ArgumentsHost) {
        const ctx = host.switchToHttp();
        const response = ctx.getResponse<Response>();
        const request = ctx.getRequest<Request>();
        const status = exception.getStatus();

        response.status(status).json({
            statusCode: status,
            timestamp: new Date().toISOString(),
            path: request.url,
        });
    }
}
```

#### Binding `@UseFilter(FilterClass)`

-   affect routes, controllers
