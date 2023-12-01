# Table of content

1. [Basic](#basic)
2. [Define type and resolver](#define-type-and-resolver)

# Basic

-   Nexus + Apollo Server
-   Code-first: define schemas and resolvers together

## Install

```bash
npm init apollo-server nexus graphql
```

## Start an app

#### Schema

```typescript
import { makeSchema } from "nexus";
import { join } from "path";
// define schema using code-first
import * as types from "./graphql";

const schema = makeSchema({
    types,
    outputs: {
        typegen: join(__dirname, "..", "nexus-typegen.ts"),
        schema: join(__dirname, "..", "schema.graphql"),
    },
});
```

#### Server

```typescript
import { ApolloServer } from "apollo-server";

const server = new ApolloServer({ schema, context });
const { url } = await server.listen();
```

# Define type and resolver

-   Type: `objectType`, `extendType`, `interfaceType`, `enumType`, `list`, `nonNull`...
-   Arg: `arg`, `booleanArg`, `intArg`, `idArg`, `stringArg`, `floatArg`

### `enumtype`

```typescript
const RoleEnum = enumType({
    name: "Role",
    members: [Role.Admin, Role.User],
});
```

### `objectType`

```typescript
t.scalar("field");
t.field("field", {
    type: ... // Nexus type
    resolve() {} // like schema-first
});
```

```typescript
const User = objectType({
    name: "User",
    definition(t) {
        // Scalar field
        t.id("id"); // using semi
        t.string("username");
        t.string("password");
        t.field("roles", { type: list(RoleEnum) });
        t.field("articles", {
            type: list(Article),
            async resolve(parent) {},
        });
    },
});
```

### `extendType`

-   Use for `Query` and `Mutation`

```typescript
const UserQuery = extendType({
    // Remember the type name
    type: "Query",
    // or
    type: "Mutation",

    definition(t) {
        // Scalar field
        t.string("greet") {
            resolve() { return  "Hello" }
        };

        // Object field
        t.field("users", {
            type: nonNull(list(User)),
            async resolve() { ... },
        });

        // With args
        t.string("login", {
            args: {
                username: nonNull(stringArg()),
                password: nonNull(stringArg()),
            },
            // take `username` and `password` from args
            async resolve(_, { username, password }) { ... }
        });

```
