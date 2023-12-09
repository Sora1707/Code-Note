# `Config`

### Install

```bash
npm i @nestjs/config
```

### Usage

-   Different environments
-   Using `.env`

#### Load to Module

```ts
import { ConfigModule } from "@nestjs/config";
imports: [
    ConfigModule.forRoot({
        isGlobal: true,
        envFilePath: [""], // using .env path or
        load: [configuration], // a custom config
    }),
];

// Custom Config
configuration = function () {
    const config = {
        port: parseInt(process.env.PORT, 10) || 3000,
        database: {
            host: process.env.DATABASE_HOST,
            port: parseInt(process.env.DATABASE_PORT, 10) || 5432,
        },
    };
    return config;
};
```

#### Get the config via `ConfigService` injection

```ts
const dbHost = this.configService.get<string>("DATABASE_HOST"); // .env
const dbHost = this.configService.get<string>("database.host"); // custom
```
