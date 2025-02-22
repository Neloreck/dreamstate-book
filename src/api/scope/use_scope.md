### Type
Function.

### About
Utility for consumption of current dreamstate scope. <br/>
Returns [scope context](https://github.com/Neloreck/dreamstate/wiki/ScopeContext) object or null, if no scope provided. <br/>

### Call Signature
```typescript
function useScope(): ScopeContext;
```

### Returns
- 'ScopeContext' - current dreamstate scope, if it is provided. Scope never updates and is constant reference.
- Scope can be regenerated with HMR code updates.

### Usage
Scope context can be used to get current manager value without effect updating every time:
```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  // Manager updates will not trigger effect every time, but we still have access to latest context values:
  useEffect(() => {
    return scope.subscribeToSignals(() => {
      const someContext: ISomeContext = scope.getContextOf(SomeContextManager);
      // ...
    });
  }, [scope]);

  ...
  ...
  ...
}
```

Scope context can be used to emit signals:
```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  const onClick = useCallback(() => scope.emitSignal({ type: "SOME_SIGNAL" }), []);

  ...
  ...
  ...
}
```

Scope can be used to register custom query providers:
```typescript
export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  // Manager updates will not trigger effect every time, but we still have access to latest context values:
  useEffect(() => {
    return scope.registerQueryProvider("SOME_QUERY_TYPE", () => "some_query_response");
  }, [scope]);

  ...
  ...
  ...
}
```

Scope can be used to query custom data from scope:
```typescript
export function SomeComponent(): ReactElement {
    const scope: ScopeContext = useScope();
    const [state, setState] = useState(0);

    const onClick = useCallback(() => {
      const response: QueryResponse<number> = scope.queryDataSync({ type: "SOME_NUMBER" });

      setState(response.data);
    }, [scope]);

...
...
...
}
```
