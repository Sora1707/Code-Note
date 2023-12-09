# Circular Dependency

A depends B and B depends A

### `forwardRef()`

#### At provider

```ts
class AService {
    constructor(
        @Inject(forwardRef(() => BService))
        private aService: AService
    ) {}
}

// Do one for B
```

#### At module

```ts
@Module({
    imports: [forwardRef(() => BModule)],
})
class AModule {}
```
