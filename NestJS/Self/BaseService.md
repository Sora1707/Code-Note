## Base Service using Moongose

#### BaseService.ts

```ts
import { Injectable } from "@nestjs/common";
import { Model } from "mongoose";

@Injectable()
export class BaseStorageService<T> {
    constructor(private model: Model<T>) {}

    async findAll(): Promise<T[]> {
        return await this.model.find();
    }
    async findById(id: string) {
        return await this.model.findById(id);
    }
    async findManyByQuery(query: object) {
        return await this.model.find(query);
    }
    async findOneByQuery(query: object) {
        return await this.model.findOne(query);
    }
    async deleteById(id: string) {
        const deletedItem = await this.model.findByIdAndDelete(id);
        return deletedItem;
    }
    async deleteManyByQuery(query: object) {
        const deletedItems = await this.model.find(query);
        await this.model.deleteMany(query);
        return deletedItems;
    }
    async deleteOneByQuery(query: object) {
        const deletedItem = await this.model.findOneAndDelete(query);
        return deletedItem;
    }
    async create<CreateDTO>(args: CreateDTO) {
        const newItem = new this.model(args);
        return await newItem.save();
    }
    async update<UpdateDTO>(query: object, updatedValue: UpdateDTO) {
        const item = await this.model.findOneAndUpdate(query, updatedValue);
        return item;
    }
}
```

#### User Service

```ts
@Injectable()
export class UsersService extends BaseStorageService<User> {
    constructor(@InjectModel(User.name) private userModel: Model<User>) {
        super(userModel);
    }
}
```
