#### Type
Function.

#### About
Factory for combining of context service and creating single provider component. <br/>
Allows components under provider component to consume related context. <br/>
<br/>

#### Call Signature
```typescript
interface IProviderProps<T> {
  initialState?: T;
  partialHotReplacement?: boolean;
  children?: ReactNode;
}
```

```typescript
function createProvider(sources: Array<IContextManagerConstructor>): FunctionComponent<IProviderProps<T>>;
```

#### Parameters
- sources - array of context manager extending classes for provision

#### Returns
- React component providing source context managers in a tree. Returned component supports initialState property

#### Throws
- TypeError : decorator call parameter is not array type
- TypeError : array member is not extending ContextManager

#### Notes
- Do not create in render functions
- Should be created once for every combination as constant reference
- Do not provide ContextManagers everywhere, few provision points are preferred

#### Usage
For example, if you want to provide context services somewhere with pre-calculated initial state:
```tsx
// Create it as static value. Not inside rendering function.
const RootProvider: FunctionComponent = createProvider([ AuthContextManager, MediaContextManager, SomeService ]);

const initialState = { a: 1, b: 2 };

function SampleComponent(): ReactElement {
  return (
    <RootProvider initialState={initialState}>
      <ApplicationRouter/>
    </RootProvider>
  );
}

```
