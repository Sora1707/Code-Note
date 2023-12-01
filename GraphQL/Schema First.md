# Table of content

1.  [Basic](#basic)
2.  [Schema](#schema)
3.  [Resolver](#resolver)
4.  [Context](#schema)
5.  [More](#more)
    -   [Directives](#directives)
    -   [Custom Scalar](#custom-scalar)
    -   [GraphQLError](#graphqlerror)

# Basic

-   Using `Graphql` and `Apollo Server`

## Install

```bash
npm i
    @apollo/server
    @graphql-tools/schema
    @graphql-tools/utils
    graphql
```

## Start a server

```typescript
import { ApolloServer } from "@apollo/server";
import { startStandaloneServer } from "@apollo/server/standalone";
import { makeExecutableSchema } from "@graphql-tools/schema";

let schema = makeExecutableSchema({
    typeDefs: [], // schemas
    resolvers: [],
});

const server = new ApolloServer({ schema });
const { url } = await startStandaloneServer(server, {
    listen: { port: 4000 },
    context,
});
```

# Schema

-   define like `Object` but using **string literal**

    ```typescript
    // # graphql marking
    const schemaDef = `#graphql
        type User {
            username: String!,
            articles: [Article!]
        }
        type Article {
            author: User
            content: String!
        }
    `;
    ```

## Basic types

-   Scalar: `Int`, `Float`, `String`, `Boolean`, `ID` (as `String`)
-   List: `[]`
-   Not null: `!`

## Object

-   `__typename`: the object/class name

## Query & Mutation

#### Query: (`READ`)

#### Mutation (`CREATE`, `DELETE`, `EDIT`)

-   **_Notice_**: we can define `Query` and `Mutation` many times

-   Syntax

    ```graphql
        function_name: return type
        function_name(args1: type, args2: type...): return type
    ```

-   Example

    ```graphql
    type Query {
        users: [User]
        userByName(username: String!): User
    }
    type Mutation {
        addUser(username: String!, password: String!): User
    }
    ```

-   Since Mutation easily exposed to error (modifying data), we should define a `MutationResponse` [interface](#interface)
    ```graphql
    interface MutationResponse {
        code: String!
        message: String!
    }
    ```
-   Each Mutation Response should implements this interface

## Enum

```graphql
enum Color {
    RED
    GREEN
    BLUE
}
```

## Union (`Object` only)

```graphql
union SearchItem = User | Article
```

-   Seperate each type using `Inline Fragment`

```graphql
__typename
... on User {}
... on Article {}
```

## Interface

-   When define an `Object` implements an `Interface`, remember to include all fields in the `Interface`

## Input

-   Provide hierarchical data as **arguments** to fields (like a **Data Transfer Object**)
-   Take `scalar`, `enum`, or `input` (no `object`)

# Resolver

-   `Resolver` is an Typescript `Object` with the field is the type we need to resolve

    ```typescript
    const resolvers = {
        Query: {
            users: () => users,
        },
    };
    ```

-   All resolver has the same syntax:

    ```typescript
    function_name: function (parent, args, contextValue, info);
    ```

    -   `parent`: the parent of the current field
    -   `args`: argument passed thourgh the query
    -   `contextValue`: object shared across all resolvers
    -   `info`: Information about the operation's execution state (field name, the path to the field from the root...)

-   Example
    ```typescript
    const typeResolver = {
        User: {
            async articles(parent, args) {
                // current field: articles of User
                const userId: String = parent.id;
                const articles = Article.find({ authorId: userId });
                return articles;
            },
        },
        Article: {
            async author(parent, args) {
                // current field: author of Article
                const userId: string = parent.authorId;
                const user = User.findById(userId);
                return user;
            },
        },
    };
    ```

## Enum

-   Specific Value for each field

```typescript
AllowedColor: {
    RED: '#f00',
    GREEN: '#0f0',
    BLUE: '#00f',
}
```

## Union & Interface

-   Use `__resolveType` to fully resolve types

```typescript
SearchItem: {
    __resolveType(obj, contextValue, info) {
        if(obj.username) {
            return 'User';
        }
        if(obj.content) {
            return 'Article';
        }
        return null; // GraphQLError is thrown
    },
}
```

# Context Function

-   pass `context` to the `startStandaloneServer` function
-   `context` is passed via `contextValue` argument in resolver
-   should be `asynchronous` and return an `object`

```typescript
{
    context: async ({ req, res }) => ({
        authScope: getScope(req.headers.authorization),
    });
}
```

-   The resolver should **_never_** modify the `contextValue` argument

# More

## Directives

-   Work like `@decorator`

```graphql
directive @uppercase(isSet: Boolean) on FIELD_DEFINITION
directive @auth(roles: [Role!]) on FIELD_DEFINITION
```

### Valid Locations

1. SCHEMA: entire schema
2. SCALAR
3. OBJECT
4. FIELD_DEFINITION: fields within an object
5. ARGUMENT_DEFINITION
6. INTERFACE
7. UNION
8. ENUM
9. ENUM_VALUE
10. INPUT_OBJECT
11. INPUT_FIELD_DEFINITION

### Custom Directives

-   Use `@graphql-tools`

```typescript
import { MapperKind, getDirective, mapSchema } from "@graphql-tools/utils";

function upperDirectiveTransform(schema, directiveName) {
    return mapSchema(schema, {
        // Executes once for each object field in the schema
        [MapperKind.OBJECT_FIELD]: fieldConfig => {
            // Check whether this field has the specified directive
            const upperDirective = getDirective(
                schema,
                fieldConfig,
                directiveName
            )?.[0];

            if (upperDirective) {
                // Get this field's original resolver
                const { resolve } = fieldConfig;

                // get the args from the Directive
                const { isSet } = upperDirective;

                // Replace the original resolver with a function that *first* calls
                // the original resolver, then converts its result to upper case
                return {
                    ...fieldConfig,
                    async resolve(source, args, context, info) {
                        // Get the original value
                        const result = await resolve(
                            source,
                            args,
                            context,
                            info
                        );
                        if (typeof result === "string" && isSet) {
                            return result.toUpperCase();
                        }
                        return result;
                    },
                };
            }
        },
    });
}
// directive @uppercase(isSet: Boolean) on FIELD_DEFINITION
schema = upperDirectiveTransform(shema, "uppercase");
```

## Custom Scalar

## GraphQLError

```typescript
throw new GraphQLError("User is not authenticated", {
    extensions: {
        code: "UNAUTHENTICATED",
        http: { status: 401 },
    },
});
```
