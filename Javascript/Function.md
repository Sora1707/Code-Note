# Function

### get & set

-   binds an object property to a function

```javascript
class ClassWithGetSet {
    #msg = "hello world";
    get msg() {
        return this.#msg;
    }
    set msg(x) {
        this.#msg = `hello ${x}`;
    }
}

const instance = new ClassWithGetSet();
console.log(instance.msg);

instance.msg = "cake";
console.log(instance.msg);
```
