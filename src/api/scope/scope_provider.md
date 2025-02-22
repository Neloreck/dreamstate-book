# ScopeProvider

### Type

React component.

### About

The `ScopeProvider` component is responsible for isolating the Dreamstate [scope](./scope.md).  
It ensures that signaling, context managers, and queries operate within a specific provider instance.  
When a `ScopeProvider` unmounts, all associated managers stop working, freeing up memory.

This component provides access to the current state of managers, as well as signaling and query methods.

### Method Signature

```typescript
export interface ScopeProviderProps {
  children?: ReactNode;
}

function ScopeProvider(
  props: ScopeProviderProps
): ReactElement;
```

### Props

- **props.children**: React nodes that will be wrapped by the ScopeProvider

### Notes

- Provides a dedicated Dreamstate scope for context isolation
- Signals and queries function strictly within the given scope
- Nested ScopeProvider instances create isolated sub-scopes, preventing cross-scope interference
- ScopeProvider should always be rendered above any context manager providers.

## Usage:

To enable Dreamstate's functionality, wrap your application's providers and consumers with ScopeProvider.
Typically, a single ScopeProvider is used at the root level of the application.

```typescript
import { createProvider, ScopeProvider } from "dreamstate";
// ... other imports ...

const StoreProvider = createProvider([SampleManager]);

export function ApplicationProvider({
  children
}: PropsWithChildren<Record<string, unknown>>): ReactElement {
  return (
    <ScopeProvider>
      <StoreProvider>{children}</StoreProvider>
    </ScopeProvider>
  );
}
```
