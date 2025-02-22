### Type

Utility function.

### About

The `createLoadable` function is a utility for handling simple asynchronous data. It encapsulates the state of
an asynchronous operation by maintaining the current value along with its loading and error states. Each mutation
(such as setting loading, ready, or error states) creates a new shallow copy of the loadable object, which helps
to manage state updates in a predictable way while reducing boilerplate code.

### Call Signature

```typescript
function createLoadable<T, E>(
  value?: T | null,
  isLoading?: boolean,
  error?: E | null
): Loadable<T, E>;
```

```typescript
interface Loadable<T, E = Error> {
  readonly value: T | null;
  readonly isLoading: boolean;
  readonly error: E | null;
  asReady(value?: T): Loadable<T, E>;
  asUpdated(value: T, isLoading?: boolean, error?: E | null): Loadable<T, E>;
  asLoading(value?: T): Loadable<T, E>;
  asFailed(error: E, value?: T): Loadable<T, E>;
}
```

### Notes

- **Immutable operations**: do not assign mutable values manually. Instead, use the provided methods
  (asLoading, asReady, asFailed, and asUpdated) to update the loadable state
- **Method integrity**: do not reassign loadable methods manually
- **Extension**: the Loadable type is not a generic object; it extends from NestedStore
- **Performance**: Dreamstate applies shallow comparison to NestedStore objects, so invoking the same method
  multiple times with identical data will not trigger unnecessary re-renders

### Usage

The createLoadable function is particularly useful when you need to manage asynchronous data within a manager class.
For example, consider a scenario where you need to load user data from an API:

```typescript
export interface IAuthContext {
  user: Loadable<string>;
}

export class AuthManager extends ContextManager<IAuthContext> {
  public readonly context: IAuthContext = {
    user: createLoadable("anonymous")
  };

  public async randomizeUser(): Promise<void> {
    // Initially: { isLoading: false, error: null, value: "anonymous" }
    const { user } = this.context;

    // As loading: { isLoading: true, error: null, value: "anonymous" }
    this.setContext({ user: user.asLoading() });

    await this.forMillis(1000);

    // As ready: { isLoading: false, error: null, value: "loadedUser" }
    this.setContext({ user: user.asReady("loadedUser") });
  }

  private forMillis(millis: number): Promise<void> {
    return new Promise((resolve) => setTimeout(resolve, millis));
  }
}
```

In this example, createLoadable is used to initialize the user state.
The manager then updates the state by calling asLoading before starting the asynchronous operation and asReady once
the new data is available. This approach handles the loading state (and any potential errors) without additional
boilerplate code.
