# Bind

### Type

Class method decorator.

### About

The `@Bind` decorator automatically binds class methods to their instance.\
This ensures that the `this` keyword
within the method always refers to the instance that owns the method, eliminating the need
for manual binding (e.g., using `.bind()` in the constructor or arrow functions).

This decorator is especially useful in scenarios where methods are passed as callbacks or used
in event handlers, as it preserves the correct context.

### Call Signature

```typescript
@Bind();
```

### Throws

- **Error**: thrown if an attempt is made to reassign a method decorated with @Bind at runtime

### Notes

- **Intended use**: designed to work seamlessly with ContextManagers
- **Restrictions**: should not be used with static methods

### Usage

When you decorate a method with @Bind, all references to that method are bound to the instance that defines it.
This means that even if the method is detached from its original context, it will still operate correctly.

```typescript
class Manager extends ContextManager {
  public numberValue: number = 42;
  public stringValue: string = "justForExample";

  @Bind()
  public getValue(): number {
    return this.value
  }
}

// Even after losing `this` context, method still is bound to it.
const manager: Manager = new Manager();
const getValue: number = manager.getValue;

console.log(getValue()); // Still returns 42
```

### Reference

All credits to 'https://www.npmjs.com/package/autobind-decorator'. <br/>
