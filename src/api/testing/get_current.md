### Type

Function

### About

Test utility to get currently provided service instance. <br/>
Returns null if parameter class is not service or is not created.

### Call Signature

```typescript
function getCurrent<T extends TAnyContextManagerConstructor>(
  ManagerClass: T,
  scope: IScopeContext
): InstanceType<T> | null;
```

### Parameters

- Helps to test context services in most cases
- Return value is optional - null if provision of requested service was not started

### Usage

For example, I want to get auth manager instance in scope and test some method:

```typescript
const scope: ScopeContext = mockScope();
const map: ManagerInstanceMap = mockManagers([ TestManager ], null, scope);

expect(map.get(TestManager)).toBeInstanceOf(TestManager);
expect(map.get(ExtendingManager)).toBeUndefined();

expect(getCurrent(TestManager, scope)).toBeInstanceOf(TestManager);
expect(getCurrent(ExtendingManager, scope)).toBeNull();
```
