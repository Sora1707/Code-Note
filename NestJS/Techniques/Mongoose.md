# `Mongoose`

### Installation

```bash
npm i @nestjs/mongoose mongoose
```

### Define connection

```ts
@Module({
  imports: [MongooseModule.forRoot('mongodb://localhost/nest')],
})
```

### Schema

#### Define

```ts
import { Prop, Schema, SchemaFactory } from "@nestjs/mongoose";
import { HydratedDocument } from "mongoose";

export type UserDocument = HydratedDocument<User>;

@Schema()
export class User {
    @Prop({ required: true })
    username: string;

    // `User` relates to `Boss`
    @Prop(@Prop({ type: mongoose.Schema.Types.ObjectId, ref: 'Boss' }))
    boss: Boss;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

#### Inject to Module

```ts
imports: [MongooseModule.forFeature([{ name: User.name, schema: UserSchema }])];
```

#### Inject to Service

```ts
constructor(@InjectModel(User.name) private userModel: Model<User>)
```
