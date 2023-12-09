# Life Cycle

The Life cycle of a nestjs program runs in the order:

-   `onModuleInit()`:
-   `onApplicationBootstrap()`: all modules initialized, before listening
-   `onModuleDestroy()`: after terminal signal
-   beforeApplicationShutdown(): all `onModuleDestroy` resolved
-   `onApplicationShutdown()`: after connection close
