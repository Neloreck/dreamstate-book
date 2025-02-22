# useManager

### Type

Function.

### About

`useManager` is a hook for consuming ContextManagers. It serves as a specialized alias for
React's `useContext`, tailored for use with ContextManagers. Additionally, it accepts an optional memo selector
function that returns an array of observed dependencies, allowing optimized re-rendering
based on specific context properties.

### Call Signature

```typescript
function useManager<
  S extends AnyObject,
  M extends ContextManagerConstructor<S>
>(
  Manager: M,
  depsSelector?: (context: S) => Array<any>
): S;
```

### Parameters

- **Manager**: The class reference of the ContextManager to consume within the current component scope
- **depsSelector** (optional): A callback function that extracts an array of dependencies from the context.
  This selector is used for diff checking to optimize re-rendering by triggering updates only
  when the specified dependencies change

### Returns

The current context value provided by the specified ContextManager in the active scope or value declared in static
gedDefaultContext method. If static method was not overrode in manager class, returns `null`.

### Throws

- **TypeError**: Thrown if the supplied parameter is not an instance of a ContextManager

### Notes

- Using a selector callback as the second parameter can help optimize rendering in components that consume large contexts
  by focusing updates on only the necessary fields
- Supplying the manager as a default parameter makes the component easier to test, as it allows you to mock the context
  without wrapping the component in a Context.Provider. Also it can help to visually distinguish internal component
  hooks and subscription to managers

### Usage

For example, to consume context provided by AuthManager:

```typescript
export function SomeComponent(): ReactElement {
  const { username } = useManager(AuthManager);
  
  // ... component logic and rendring
}
```

By default, every update to the auth context will trigger a re-render of the component (as is typical with
React's useContext). To observe only specific fields and avoid unnecessary re-renders, supply a memo selector:

```typescript
export function SomeComponent(): ReactElement {
  const { username } = useManager(
    AuthManager,
    ({ username }: IAuthContext) => [username]
  );
  
  // ... component logic
}
```

Variant with props:

```typescript
export function SomeComponent({
  authContext: { username } = useManager(AuthManager)
}): ReactElement {
  // ... component logic and rendring
}
```
