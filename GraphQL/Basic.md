### Aliases

Change the query's name to another string

```graphql
{
    sora: user(username: "Sora") {}
    legendavrs: user(username: "Legendavrs") {}
}
```

### Fragment

#### Reuseable code

```graphql
fragment basicInfo on User {
    username
    friend(username: $username) {
        username
    }
    articles {
        content
    }
}
```

```graphql
query getUser($username: String = "Quantical") {
    sora: user(username: "Sora") {
        ...basicInfo
    }
    legendavrs: user(username: "Legendavrs") {
        ...basicInfo
    }
}
```

[NOTE] `getUser` is using variable with default value `"Quantical"`

#### Cannot be used on itself (loop)

```graphql
fragment basicInfo on Character {
    username
    friends {
        ...basicInfo
    }
}
```

#### Type indentification

```graphql
{
    user {
        username
        ...AdminFields
    }
}

fragment AdminFields on Admin {
    adminId
}
```

or using shorthand

```graphql
{
    user {
        username
        ... on Admin {
            adminId
        }
    }
}
```

### Inline Fragment

Get the types from Union/Interface

```graphql
query searchItem($keyword: String!) {
    items(keyword: $keyword) {
        createdDate
        ... on User {
            username
        }
        ... on Article {
            content
        }
    }
}
```

### Thinking in GraphQL

-   **_"How"_** rather than **_"What"_** when building schema (how client use it, not what the database shows)
-   Delegate authorization to the **_business logic layer_**

### Pagination

#### Problems

-   Request k fields multiple times: [1..k], [k+1, 2k], [2k+1, 3k]...
