#### Type
Method decorator.

#### About
Method decorator that lazily binds methods to class instances. <br/>
Bound methods can be used without .bind calls or arrow functions usage. <br/>

#### Call Signature
```typescript
@Bind();
```

#### Throws
- Error : attempt to re-assign decorator method at runtime

#### Notes
- Intended to work with ContextManagers
- Do not use with static methods

#### Usage
After decorating, all manager method references will be bound to class instance that owns callback reference.
```typescript
class Service {
 
  public numberValue: number = 42;
  public stringValue: string = "justForExample";

  @Bind()
  public getValue(): number {
    return this.value
  }

}
```
Even afte losing "this" context, method still is bound to it.
```typescript
const service: Service = new Service();
const getValue: number = service.getValue // .bind(service) isn't needed.

getValue() // returns 42
```

#### Reference
All credits to 'https://www.npmjs.com/package/autobind-decorator'. <br/>
