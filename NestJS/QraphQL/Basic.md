# Install

```bash
npm i @nestjs/graphql @nestjs/apollo @apollo/server graphql
```

# Initialize in `App Module`

```ts
import { GraphQLModule } from "@nestjs/graphql";
import { ApolloDriver, ApolloDriverConfig } from "@nestjs/apollo";
imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
        driver: ApolloDriver,
        // Generate a graphql Schema
        autoSchemaFile: join(process.cwd(), "src/schema.gql"),
    }),
];
```

# Resolvers

### Define a `User Model`

```ts
import { Field, ObjectType } from "@nestjs/graphql";
import { Article } from "./article";
import { Role } from "src/global/roles.enum";

@ObjectType()
export class User {
    @Field(type => String, { nullable: false })
    id: string;

    @Field({ nullable: false })
    username: string;

    @Field({ nullable: false })
    password: string;

    @Field(type => [Role], { nullable: "itemsAndList" })
    roles: Role[];

    // type resolvers [Item]
    // type define Item[]
    @Field(type => [Article], { nullable: "items" })
    articles: Article[];
}
```

### Register `Enum` type

```ts
registerEnumType(Role, { name: "Role" });
```

### Define a `User Resolver`

```ts
@Resolver(of => User) // Define the parent type
export class UsersResolver extends BaseResolver(User) {
    constructor(
        private usersService: UsersService,
        private articlesService: ArticlesService
    ) {
        super();
    }

    @Query(returns => [User], { name: "users" })
    async getUsers() {}

    @Query(returns => User, { name: "user" })
    async getUser(
        @Args("username", {
            type: () => String,
            nullable: false,
            defaultValue: "Sora",
        })
        username: string
    ) {}

    @Mutation(returns => User)
    // Shaping args with an interface
    async createUser(@Args() args: UserPostArgs) {}

    @ResolveField("articles", returns => [Article])
    async articles(@Parent() user: User) {
        const { id } = user;
        return await this.articlesService.findByAuthorId(id);
    }
}
```

`UserPostsArgs.ts`

```ts
import { ArgsType, Field } from "@nestjs/graphql";
import { Role } from "src/global/roles.enum";

@ArgsType()
export class UserPostArgs {
    @Field({ nullable: false })
    username: string;

    @Field({ nullable: false })
    password: string;

    @Field(type => [Role], { nullable: true, defaultValue: [Role.User] })
    roles: Role[];
}
```

### Inject into `Module` for use

```ts
providers: [UsersResolver];
```

### `Base Resolver` using typescript inheritance

```ts
function BaseResolver<T extends Type<unknown>>(classRef: T): any {
    @Resolver({ isAbstract: true })
    abstract class BaseResolverHost {
        @Query(type => [classRef], { name: `findAll${classRef.name}` })
        async findAll(): Promise<T[]> {
            return [];
        }
    }
    return BaseResolverHost;
}
```

# Custom Scalar

### Example: `Date` with `CustomScalar`

```ts
import { Scalar, CustomScalar } from "@nestjs/graphql";
import { Kind, ValueNode } from "graphql";

@Scalar("Date", type => Date)
export class DateScalar implements CustomScalar<number, Date> {
    description = "Date custom scalar type";

    parseValue(value: number): Date {
        return new Date(value); // value from the client
    }

    serialize(value: Date): number {
        return value.getTime(); // value sent to the client
    }

    parseLiteral(ast: ValueNode): Date {
        if (ast.kind === Kind.INT) {
            return new Date(ast.value);
        }
        return null;
    }
}
```

### Example: `UUID` with `GraphQLScalarType`

```ts
import { GraphQLScalarType } from "graphql";
export const CustomUuidScalar = new GraphQLScalarType({
    name: "UUID",
    description: "A simple UUID parser",
    serialize: value => validate(value),
    parseValue: value => validate(value),
    parseLiteral: ast => validate(ast.value),
});
```

### Inject via providers

```ts
providers: [DateScalar],
```

### Inject via imports in `AppModule`

```ts
imports: [
    GraphQLModule.forRoot({
        resolvers: { UUID: CustomUuidScalar },
    }),
];
```
