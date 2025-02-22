# ContextManager

### Type

Abstract Class

### About

`ContextManager` is the core abstract class for managing application data in Dreamstate. It should be extended
by your application store managers to encapsulate state, handle updates, emit signals, and process queries.

- To provide specific `ContextManager` classes into the React tree, see the [createProvider](./create_provider.md) method
- To consume data from a specific `ContextManager` in the React tree, see the [useManager](./use_manager.md) hook
- For detailed information on how shallow comparisons are performed during context updates, refer to utility
  methods like [createNested](../utils/create_nested.md), [createActions](../utils/create_actions.md), and others

### Class Signature

```typescript
abstract class ContextManager<
  T extends AnyObject,
  S extends AnyObject
> {
  public static get REACT_CONTEXT(): Context<T>;

  public static getDefaultContext(): AnyObject | null

  public IS_DISPOSED: boolean;

  public context: T;

  public constructor(initialState?: S);

  public onProvisionStarted(): void;

  public onProvisionEnded(): void;

  public getScope(): ScopeContext;

  public forceUpdate(): void;

  public setContext(next: Partial<T> | PartialTransformer<T>): void;

  public emitSignal<D = undefined>(baseSignal: BaseSignal<D>): void;

  public queryDataAsync<
    D extends AnyValue,
    T extends QueryType,
    Q extends OptionalQueryRequest<D, T>
  >(
    queryRequest: Q
  ): Promise<TQueryResponse<AnyValue>>;

  public queryDataSync<
    D extends AnyValue,
    T extends QueryType,
    Q extends OptionalQueryRequest<D, T>>(
    queryRequest: Q
  ): TQueryResponse<any>;
}
```

## Notes

- Constructor best practices:
  - Avoid heavy logic or subscriptions in the constructor; use it primarily for synchronous initialization of class fields
- Lifecycle management:
  - Context managers should begin operations on the provision start event and perform cleanup on the provision end event
- State organization:
  - Avoid creating deeply nested objects in stores; flatter state structures are easier to maintain
- Hot module replacement:
  - Managers should be modular. Clean all data on provision end and avoid reliance on the current environment for easier
    HMR updates during development

## Static methods and properties

### getDefaultContext

- **Description:**  
  Returns the default React context value. This method provides a placeholder value for context consumers when no
  specific manager is provided.
- **Return type:** `AnyObject | null`
- **Notes:** 
  Defaults to `null` if no custom default is specified.

### REACT_CONTEXT

- **Description:**  
  Retrieves the associated React context for the current `ContextManager`. This context is lazily initialized and is
  useful for manual rendering or testing.
- **Return Type:** `Context<AnyValue>`
- **Notes:**  
  Throws an error if accessed directly on the base `ContextManager` class, as direct references to
  ContextManager statics are forbidden.

## Instance fields

### IS_DISPOSED

- **Description:**  
  A flag that indicates whether the manager is still active or has been disposed.  
  Once disposed, the manager cannot be reused, and scope-related methods (signals, queries)
  will throw exceptions if used.
- **Type:** `boolean`
- **Default Value:** `false`

### context

- **Description:**  
  Stores the current state of the manager. This field is synchronized with React providers via `setContext`.  
  It should always be an object, and while nested field mutations are possible, they are not recommended.
- **Type:** `T`
- **Notes:**  
  Meta fields (e.g., created by `createActions` or `createNested`) may use a different comparison mechanism than the standard shallow check.

## Constructor

### constructor(initialState?: S)

- **Description:**  
  Initializes a new instance of the `ContextManager` with an optional initial state.  
  This initial state may be used for synchronous initialization or provided from SSR.
- **Parameters:**
  - `initialState` (optional): The initial state used to set up the manager.
- **Notes:**  
  Calls `processComputed` on the context to ensure that computed fields are calculated before the manager is provisioned.

## Instance Methods

### onProvisionStarted

- **Description:**  
  Lifecycle method invoked when the first provider is injected into the React tree.  
  It is analogous to `componentWillMount` in class-based components.
- **Usage:**  
  Used for data initialization and setting up subscriptions.
- **Return Type:** `void`

### onProvisionEnded

- **Description:**  
  Lifecycle method called when the last provider is removed from the React tree.  
  It functions similarly to `componentWillUnmount`.
- **Usage:**  
  Ideal for cleanup tasks such as disposing of data and unsubscribing from events, especially during
  hot module replacement (HMR) or dynamic managers provision.
- **Return Type:** `void`

### getScope

- **Description:**  
  Retrieves the current manager scope, which contains methods for managing context and retrieving manager instances.
- **Return Type:** `ScopeContext`
- **Throws:**  
  Throws an error if the manager is out of scope.

### forceUpdate

- **Description:**  
  Forces an update and re-render of subscribed components by creating a new shallow copy of the current context.  
  This ensures that updates are propagated to the provider.
- **Usage:**  
  Useful for ensuring that all subscribers are updated when necessary.
- **Return Type:** `void`
- **Notes:**  
  Only forces an update of the provider; components using `useManager` selectors will not be forced to re-render.

### setContext

- **Description:**  
  Updates the current context using a partial state object or a functional selector.  
  It performs a shallow comparison with the existing context and updates the provider only if changes are detected.
- **Parameters:**
  - `next`: A partial context object or a function that returns a partial context.
- **Return Type:** `void`

### emitSignal

- **Description:**  
  Emits a signal to other managers and subscribers within the current scope.  
  A signal includes a type and optional data.
- **Parameters:**
  - `baseSignal`: An object containing a signal type and optional data.
- **Return Type:** `SignalEvent<D>`
- **Throws:**  
  Throws an error if the manager is out of scope.

### queryDataAsync

- **Description:**  
  Sends an asynchronous query to retrieve data from query handler methods.  
  Particularly useful for async providers.
- **Parameters:**
  - `queryRequest`: An object containing the query type and optional data.
- **Return Type:** `Promise<QueryResponse<AnyValue>>`
- **Usage:**  
  Resolves with a query response object if a valid handler is found, or `null` if not.

### queryDataSync

- **Description:**  
  Sends a synchronous query to retrieve data from query handler methods.  
  Ideal for synchronous providers; async providers may still return a promise within the data field.
- **Parameters:**
  - `queryRequest`: An object containing the query type and optional data.
- **Return Type:** `QueryResponse<AnyValue> | null`
- **Usage:**  
  Returns a query response if a valid handler is found, or `null` if no handler exists in the current scope.


## Usage Example

Below is an example of creating an authentication manager that stores user information and provides
methods for state mutations:

```typescript
import { ContextManager } from "dreamstate";

export interface IAuthContext {
  authActions: {
    randomizeUser(): void;
    changeAuthenticationStatus(): void;
  };
  isAuthenticated: boolean;
  user: string;
}

export class AuthManager extends ContextManager<IAuthContext> {
  public readonly context: IAuthContext = {
    authActions: createActions({
      changeAuthenticationStatus: () => this.changeAuthenticationStatus(),
      randomizeUser: () => this.randomizeUser()
    }),
    isAuthenticated: true,
    user: "anonymous"
  };

  public changeAuthenticationStatus(): void {
    this.setContext(({ isAuthenticated }) => ({ isAuthenticated: !isAuthenticated }));
  }

  public randomizeUser(): void {
    this.setContext({ user: "user-" + Math.random() });
  }
}
```

To integrate this manager with other context managers, provide it at the application level using Dreamstate's scope:

```tsx
import { ScopeProvider, createProvider } from "dreamstate";
// ...

const ApplicationProvider = createProvider([ CustomService, AuthManager ]);

export function ApplicationRoot(): ReactElement {
  return (
    <ScopeProvider>
      <ApplicationProvider>
        <ApplicationRouter/>
      </ApplicationProvider>
    </ScopeProvider>
  );
}
```

For consuming the context in your components, use the useManager hook:

```tsx
import { useManager } from "dreamstate";
// ...

export function SomeNestedComponent(): ReactElement {
  const { user } = useManager(AuthManager);

  return (
    <div>
      Current user is: { user }
    </div>
  );
}
```
