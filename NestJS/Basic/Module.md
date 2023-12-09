# `Module` (Organizer)

### Usage `@Module`

Module is **_singleton_** by default and every module is a **_shared_** module

-   `providers`: shared across this module (encapsulation)
-   `controllers`
-   `imports`: providers exported by other modules (used in this module)
-   `exports`: providers for other modules

-   `@Global`: global modules

-   Module classes cannot be injected as providers due to **_circular dependency_**.

### Dynamic/Cusomizable `Module`

-   Register and Configure providers dynamically at run time

```ts
@Module({})
export class ConfigModule {
    static register(options: Record<string, any>): DynamicModule {
        return {
            module: ConfigModule, // must have
            providers: [
                {
                    provide: `CONFIG_OPTIONS`,
                    useValue: options,
                },
                ConfigService,
            ],
            exports: [ConfigService],
        };
    }
}
```
