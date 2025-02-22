### Type

Abstract class.

### About

Core abstract class for application data management. <br/>
Should be extended by application store managers. <br/>
* To provide specific ContextManager classes in react tree, see 'createProvider' method.
* To consume specific ContextManager data in react tree, see 'useManager' method.
* To get more details about shallow check on context updates, see 'createNested', 'createActions' and other utility methods.

### Class signature
```typescript
declare abstract class ContextManager<T extends TAnyObject, S extends TAnyObject> {

    /**
     * React context getter.
     * Method allows to get related React.Context for manual renders or testing.
     * Lazy initialization, even for static resolving before anything from ContextManager is used.
     */
    public static get REACT_CONTEXT(): Context<T>;

    /**
     * Flag indicating whether current manager is still working or disposed.
     * Once manager is disposed, it cannot be reused or continue working.
     * Scope related methods (signals, queries) will not be visible, usage of scope related methods will throw exceptions.
     */
    public IS_DISPOSED: boolean = false;

    /**
     * Manager instance context.
     * Generic field that will be synchronized with react providers when 'setContext' method is called.
     * Should be object value.
     *
     * Manual nested values fields mutations are allowed, but not desired.
     * After calling setContext it will be shallow compared with already provided context before react tree syncing.
     * Meta fields created by dreamstate utils (createActions, createNested etc)
     *   may have different comparison instead of shallow check.
     * 
     * If you need more details about shallow check, see 'createNested', 'createActions' and other methods.
     */
    public context: T;

    /**
     * Generic context manager constructor.
     * Initial state can be used as some initialization value or SSR provided data.
     * Treating it as optional value can help to write more generic and re-usable code because manager can be
     * provided in a different place with different initial state.
     *
     * @param initialState - initial state received from dreamstate Provider component properties.
     */
    public constructor(initialState?: S);

    /**
     * Forces update and render of subscribed components.
     * Just in case when you need forced update to keep everything in sync with your context.
     *
     * Side effect: after successful update all subscribed components will be updated accordingly to their subscription.
     *
     * Note: it will only force update of provider, components that use useManager selectors will not be forced to render.
     * Note: creates new shallow copy of 'this.context' reference after each call.
     * Note: if manager is out of scope, it will simply replace 'this.context'.
     */
    public forceUpdate(): void;


    /**
     * Update current context from partially supplied state or functional selector.
     * Updates react providers tree only if 'shouldObserversUpdate' check has passed and anything has changed in store.
     * Has same philosophy as 'setState' of react class components.
     *
     * Side effect: after successful update all subscribed components will be updated accordingly to their subscription.
     *
     * Note: partial context is needed or callback that returns partial context.
     * Note: it will only update provider, components that use useManager selectors will not be forced to render.
     * Note: if manager is out of scope, it just rewrites 'this.context' object without side effects.
     *
     * @param {Object|Function} next - part of context that should be updated or context transformer function.
     *   In case of functional callback, it will be executed immediately with 'currentContext' parameter.
     */
    public setContext(next: Partial<T> | TPartialTransformer<T>): void;

    /**
     * Emit signal for other managers and subscribers in current scope.
     * Valid signal types: string, number, symbol.
     *
     * @throws {Error} - manager is out of scope.
     *
     * @param {Object} baseSignal - signal base that contains basic descriptor of emitting signal.
     * @param {TSignalType} baseSignal.type - signal type.
     * @param {*=} baseSignal.data - optional signal data.
     * @returns {Promise} promise that will be resolved after signal listeners call.
     */
    public emitSignal<D = undefined>(baseSignal: IBaseSignal<D>): void;

    /**
     * Send context query to retrieve data from query handler methods.
     * Async method is useful when provider is async. Sync providers still will be handled correctly.
     * Will return query response packed object in case if valid handler is found in scope.
     * Will return null in case if no valid handler found in current scope.
     *
     * @param {Object} queryRequest - query request base.
     * @param {TQueryType} queryRequest.type - query type.
     * @param {*=} queryRequest.data - optional query data, some kind of getter params.
     * @return {Promise<null | TQueryResponse<*>>} result of query search or null, if no providers in current scope.
     */
    public queryDataAsync<D extends any, T extends TQueryType, Q extends IOptionalQueryRequest<D, T>>(queryRequest: Q): Promise<TQueryResponse<any>>;

    /**
     * Send context query to retrieve data from query handler methods.
     * Sync method is useful when provider is sync. Async providers will return promise in a data field.
     * Will return query response packed object in case if valid handler is found in scope.
     * Will return null in case if no valid handler found in current scope.
     *
     * @param {Object} queryRequest - query request base.
     * @param {TQueryType} queryRequest.type - query type.
     * @param {*=} queryRequest.data - optional query data, some kind of getter params.
     * @return {null | TQueryResponse<*>} result of query search or null, if no providers in current scope.
     */
    public queryDataSync<D extends any, T extends TQueryType, Q extends IOptionalQueryRequest<D, T>>(queryRequest: Q): TQueryResponse<any>;

    /**
     * Lifecycle method.
     * First provider was injected into react tree.
     * Same philosophy as 'componentWillMount' for class-based components.
     *
     * Useful for data initialization and subscriptions creation.
     */
    protected onProvisionStarted(): void;

    /**
     * Lifecycle method.
     * Last provider was removed from react tree.
     * Same philosophy as 'componentWillUnmount' for class-based components.
     *
     * Useful for data disposal when context is being ejected/when HMR happens.
     */
    protected onProvisionEnded(): void;

}

```

### Parameters
  - Avoid heavy constructor logic/subscriptions, use it for sync initialization of class fields
  - Context managers should start working on provision start event and should cleanup everything on provision end event
  - Avoid creation of deeply nested object in stores, flatter fields are easier to maintain
  - It is easier to maintain HMR updates while developing if managers are modular, cleaning all the data on provision end and do not rely on current environment

### Usage
For example, if you need to create auth manager that stores information about current user and provides few methods for mutations:

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

Then you should provide it with other context managers.
Do not forget about application-level scope provision.

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

For consumption, we should call useManager hook:

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
