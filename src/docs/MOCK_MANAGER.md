#### Type
Utility function.

#### About
Function to mock manager instance in testing context.

#### Call Signature
```typescript
function mockManager<T extends TAnyObject, S extends TAnyObject, M extends IContextManagerConstructor<T, S>>(
    ManagerClass: M,
    initialState?: S | null,
    scope: IScopeContext = initializeScopeContext()
): InstanceType<M>
```

#### Parameters
- ManagerClass - class definition reference to mock instance
- initialState - optional initial state used in constructor
- scope - optional scope for manager mocking

#### Returns
Mocked manager instance for testing in provided scope.

#### Usage
Used for testing of managers functionality.

```typesctpt
describe("Some manager functionality", () => {
  it("Should correctly handle some updates", () => {
    const manager: ComputedManager = mockManager(ComputedManager);

    expect(manager.context.numbers).toHaveLength(6);
    expect(manager.context.computed.greaterThanFive).toHaveLength(3);

    manager.setContext({ numbers: [ 1, 2, 10 ] });

    expect(manager.context.numbers).toHaveLength(3);
    expect(manager.context.computed.greaterThanFive).toHaveLength(1);
  });
});
```
