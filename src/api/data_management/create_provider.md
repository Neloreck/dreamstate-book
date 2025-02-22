# createProvider

### Type

Function.

### About

`createProvider` is a factory function for combining context managers and creating a single provider component.
It enables components within the provider to consume the related context seamlessly.

### Call Signature

```typescript
interface ProviderProps<T> {
  initialState?: T;
  children?: ReactNode;
}

export interface CreateProviderProps {
  /**
   * A flag that determines whether to observe the context changes in one large React node
   *   or as smaller scoped nodes for better performance.
   */
  isCombined?: boolean;
}

function createProvider(
  sources: Array<ContextManagerConstructor>,
  config?: CreateProviderProps,
): FunctionComponent<ProviderProps<T>>;
```

### Parameters

- **sources**: an array of context manager classes to be provided
- **config**: configuration object for adjusting created provider

### Returns

A React component that provides the specified context managers within a component tree. The returned component
supports an initialState property for initializing context.

### Notes:

- Do not create providers inside render functions
- Providers should be created once per combination as a constant reference, globally or in some cached storage
- It is preferable to have a few provision points rather than providing ContextManagers everywhere
- `initialState` property can be passed on provider on rendering, it will be supplied for managers on construction
- `isCombined` config value can be passed on provider creation to adjust how managers are provisioned

### Throws

- **TypeError**: thrown if the parameter is not an array
- **TypeError**: thrown if any member of the array does not extend ContextManager

### Usage

For example, to provide context services with a pre-calculated initial state:

```typescript
// Create the provider as a static value, should not be inside a render function.
const RootProvider: FunctionComponent = createProvider([
  AuthContextManager,
  MediaContextManager,
  SomeService,
]);

const initialState = { a: 1, b: 2 };

function SampleComponent(): ReactElement {
  return (
    <RootProvider initialState={initialState}>
      <ApplicationRouter/>
    </RootProvider>
  );
}
```
