### Type
React component.

### About
Dreamstate scope provider component. <br/>
Makes sure signaling and context managers are isolated under this specific provider. <br/>
Once scope unmounts, managers under it should stop working and free RAM. <br/>
<br/>
Scope gives access to current managers states, signaling and queries context and methods.

### Method signature
```typescript
function ScopeProvider(props: =ScopeProviderProps): ReactElement;
```

```typescript
export interface ScopeProviderProps {
  children?: ReactNode;
}
```

### Parameters
- Simple scope provider for dreamstate context isolation
- All signals and queries are working strictly under the scope
- Another scope below another scope takes priority and isolates everything in tree below
- Make sure ScopeProvider is rendering above managers providers

### Usage
Application providers and consumers should be wrapped with ScopeProvider in order to work correctly. <br/>
Typically one provider component exists that wraps application with scope and managers providers. <br/>
As an example:
```typescript
import { createProvider, ScopeProvider } from "dreamstate";

/**
 * Application managers provider.
 */
const StoreProvider = createProvider([ SampleManager ]);

export function ApplicationProvider({ children }: PropsWithChildren<Record<string, unknown>>): ReactElement {
  return (
    <ScopeProvider>
      <StoreProvider>{ children }</StoreProvider>
    </ScopeProvider>
  );
}
```
