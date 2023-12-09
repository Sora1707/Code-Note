# Authentication

### JWT Token

#### Install

```ts
npm install --save @nestjs/jwt
```

#### Usage

###### Register

```ts
imports: [
    JwtModule.register({
      global: true,
      secret: 'SECRET_KEY'
      signOptions: { expiresIn: '60s' },
    }),
  ],
```

###### Inject

```ts
import { JwtService } from '@nestjs/jwt';
constructor(
    private jwtService: JwtService
) {}
```

###### Get token

```ts
const payload = { sub: user.userId, username: user.username };
return {
    access_token: await this.jwtService.signAsync(payload),
};
```

###### Retrieve token

```ts
const payload = await this.jwtService.verifyAsync(token, {
    secret: jwtConstants.secret,
});

// Pass payload to request
request["user"] = payload;
```
