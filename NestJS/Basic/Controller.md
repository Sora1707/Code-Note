# `Controller` (Routing)

### Usage `@Controller`

-   `@[Method]`: Method is` http method`
-   `@HttpCode(StatusCode)`
-   Express objects: `@Res`, `@Req`, `@Next` as parameter decorator
    ```typescript
    import { Request, Response, NextFunction } from "express";
    function findAll(
        @Req req: Request,
        @Res() res: Response,
        @Next next: NextFunction
    );
    ```
    -   `@Res(key)`is the same as `req[key]`;
    -   Special `req` object: `@Body`, `@Query`, `@Param`
-   Two way to response:

    -   Standard: using `return`, automatically changed to `JSON`
    -   Response object (Express): `@Res` and `res.send(...)`
    -   If we use Express object, it is considered we are using the **_second way_**

-   Request payload using `DTO interface`(define how data be transferred)
