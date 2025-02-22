# mockScope

### Type

Function.

### About

Function to mock scope object in testing context.
Mocked scope can be used to emit fake signals and query stored data.

### Call Signature

```typescript
interface IMockScopeConfig {
  isLifecycleDisabled?: boolean;
  applyInitialContexts?: Array<[TAnyContextManagerConstructor, TAnyObject]>;
}

function mockScope(
  mockConfig: IMockScopeConfig = {},
  registry: IRegistry = createRegistry()
): IScopeContext;
```

### Parameters

- mockConfig - configuration object for mocked scope
- mockConfig.applyInitialContexts - array of mocked contexts to be applied for picked manager classes
- mockConfig.isLifecycleDisabled - whether lifecycle in scope is enabled
- registry - mocked object if internal registry should be replaced

### Returns

Mocked ScopeContext object for testing.

### Usage

Mocking util can be used to verify some signals or scope-level functionality with queries and managers interaction.

```typescript
describe("Some manager functionality", () => {
  it("Should correctly handle some signals", () => {
    const scope: IScopeContext = mockScope();
    const manager: TestManager = mockManager(TestManager, {}, scope);

    expect(manager.context.field).toBe(1);
    scope.emitSignal({ type: "SET_FIELD", data: 2 });
    expect(manager.context.field).toBe(2);
  });
});
```
