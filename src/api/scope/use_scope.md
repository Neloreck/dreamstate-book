# useScope

### Type

Function.

### About

The `useScope` hook provides access to the current Dreamstate scope.  
It returns a [`ScopeContext`](./scope_context.md) object or `null` if no scope is available.

Scope enables interaction with context managers, signals, and queries without triggering unnecessary re-renders.

### Call Signature

```typescript
function useScope(): ScopeContext;
```

### Returns

ScopeContext – The current Dreamstate scope, if available.
The returned scope is a constant reference and does not update.
It can be regenerated with Hot Module Replacement updates.

### Usage

Using `useScope` allows retrieving manager values without triggering effects on every update:

```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  // Manager updates will not trigger effect every time, but we still have access to latest context values:
  useEffect(() => {
    return scope.subscribeToSignals(() => {
      const someContext: ISomeContext = scope.getContextOf(SomeContextManager);
      // Handle the retrieved context...
    });
  }, [scope]);

  return <div>content</div>;
}
```

Signals can be dispatched to notify subscribers in the current scope:

```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  const onClick = useCallback(() => {
    scope?.emitSignal({ type: "SOME_SIGNAL" });
  }, [scope]);

  return <button onClick={onClick}>Emit Signal</button>;
}
```

A component can register itself as a provider for specific queries:

```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  useEffect(() => {
    return scope.registerQueryProvider("SOME_QUERY_TYPE", () => "some_query_response");
  }, [scope]);

  return <div>Query provider active</div>;
}
```

Data queries can be executed synchronously within the current scope:

```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();
  const [state, setState] = useState(0);

  const onClick = useCallback(() => {
    const response: QueryResponse<number> | null = scope.queryDataSync({ type: "SOME_NUMBER" });

    if (response) {
      setState(response.data);
    }
  }, [scope]);

  return <button onClick={onClick}>Query data</button>;
}
```
