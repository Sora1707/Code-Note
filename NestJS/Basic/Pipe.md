# `Pipe` (Validator)

### Purposes

-   Transformation
-   Validation

### Usage

-   Operate on the arguments in controller route handler
-   When an **_exception_** is thrown in a Pipe, **_NO_** controller method is subsequently executed

#### Built-in

-   ValidationPipe
-   Parse \* Pipe
-   DefaultValuePipe(defaultValue)

#### Binding

```typescript
@Param('id', ParseIntPipe) id: number
@Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE }))
```

#### Custom

```typescript
@Injectable()
export class ValidationPipe implements PipeTransform<T, R> {
    transform(value: T, metadata: ArgumentMetadata): R {
        // logic here
        return value;
    }
}
```

#### `ArgumentMetadata`

```typescript
export interface ArgumentMetadata {
    // @Body, @Query...
    type: "body" | "query" | "param" | "custom";
    // Type of the argument: String, Number,...
    // `undefined` if no declaration
    metatype?: Type<unknown>;
    // The string put inside (), @Query('auth')
    data?: string;
}
```

#### Using `class-validator` and `class-transformer`

```typescript
import { IsString, IsInt } from "class-validator";

export class CreateUserDto {
    @IsString()
    username: string;

    @IsString()
    password: string;

    @IsInt()
    day: number;
}
```

```typescript
import { validate } from "class-validator";
import { plainToInstance } from "class-transformer";

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
    async transform(value: any, { metatype }: ArgumentMetadata) {
        if (!metatype || this.isDefaultType(metatype)) {
            return value;
        }
        const object = plainToInstance(metatype, value);
        const errors = await validate(object);
        if (errors.length > 0) {
            throw new BadRequestException("Validation failed");
        }
        return value;
    }

    private isDefaultType(metatype: Function): boolean {
        const types: Function[] = [String, Boolean, Number, Array, Object];
        return types.includes(metatype);
    }
}
```
