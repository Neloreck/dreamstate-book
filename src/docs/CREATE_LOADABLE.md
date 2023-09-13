#### Type
Function.

#### About
Utility for handling of simple async data.
 - Includes loading and error state for reducing of possible boilerplate code. <br/>
 - Each mutation creates new shallow copy of loadable object.

#### Call Signature
```typescript
function createLoadable<T, E>(value?: T | null, isLoading?: boolean, error?: E | null): Loadable<T, E>;
```

```typescript
interface Loadable<T, E = Error> {
  readonly error: E | null;
  readonly isLoading: boolean;
  readonly value: T | null;
  asFailed(error: E, value?: T): Loadable<T, E>;
  asLoading(value?: T): Loadable<T, E>;
  asReady(value?: T): Loadable<T, E>;
  asUpdated(value: T, isLoading?: boolean, error?: E | null): Loadable<T, E>;
}
```

#### Notes
- Do not assign mutable values manually, use asLoading, asReady, asFailed and asUpdated methods.
- Do not re-assign loadable methods manually.
- Loadable is not generic object, it extends NestedStore.
- Dreamstate applies shallow comparison to NestedStore objects (methods called multiple times with same data will not trigger odd rendering).

#### Usage
For example, if we should load something from api for manager class:

```typescript
export interface IAuthContext {
  user: Loadable<string>;
}

export class AuthManager extends ContextManager<IAuthContext> {

  public readonly context: IAuthContext = {
    user: createLoadable("anonymous")
  };

  public async randomizeUser(): Promise<void> {
    const { user } = this.context;
    // user: { isLoading: false, error: null, value: "anonymous" }.
    this.setContext({ user: user.asLoading() });
    // user: { isLoading: true, error: null, value: "anonymous" }.
    await this.forMillis(1000);
    this.setContext({ user: user.asReady("loadedUser") });
    // user: { isLoading: false, error: null, value: "loadedUser" }.
  }

  private forMillis(millis: number): Promise<void> {
    return new Promise((resolve) => setTimeout(resolve, millis));
  }

}
```

In that way we handled loading state (and possible error) without additional boilerplate code.
