### Type
Function.

### About
Utility for consumption of ContextManagers. <br/>

 - React hook
 - Same as React useContext (alias), but for ContextManager
 - Additionally allows providing of memo selector as second argument
 - Second parameter is optional function that returns array of required props from context

### Call Signature
```typescript
function useManager<S extends object, M extends IContextManagerConstructor<S>>(
  Manager: M,
  depsSelector?: (context: S) => Array<any>
): S;
```

### Parameters
  - Manager - class extending context manager reference to consume in current component
  - depsSelector - optional callback for diff checking before update, returns array of observed dependencies

### Returns
  - 'context' value of existing ContextManager class in current scope

### Throws
  - TypeError : a supplied parameter is not instance of ContextManager

### Parameters
  - Provide selector callback as second parameter for big contexts and rendering optimization
  - Consume manager in parameters as default value - it will be easier to test component later

### Usage
For example, we should load context for AuthContextManager:

```typescript
export function SomeComponent(): ReactElement {
  const { username } = useManager(AuthContextManager);

  ...
  ...
  ...
}
```

Same problem as with default context: every update of auth context will trigger rendering of our component. That's how useContext from react behaves. <br/>
If we want to observe only one field and ignore updates of other parameters, we should supply memo selector:

```typescript
export function SomeComponent({
}): ReactElement {
  const { username } = useManager(AuthContextManager, ({ username }: IAuthContext) => [ username ]);

  ...
  ...
  ...
}
```

Default props usage for hook declaration makes component easier to test - you will not need to create Context.Provider before rendering of your component because it can be mocked with props parameter.
