# ScopeContext

### Type

Object interface.

### About

The `ScopeContext` interface describes the methods and data available within the current Dreamstate scope.
All active providers, managers, signals, and queries operate within this scope and do not function outside of it.
Accessing the scope allows you to work with specialized methods for signal handling, query processing,
and context management.

### Interface signature

```typescript
export interface ScopeContext {
  INTERNAL: ScopeContextInternals;

  getContextOf<
    T extends AnyObject,
    D extends ContextManagerConstructor<T>
  >(
    manager: D
  ): T;

  emitSignal<
    D = undefined
  >(
    base: BaseSignal<D>,
    emitter?: AnyContextManagerConstructor | null
  ): SignalEvent<D>;

  subscribeToSignals<
    D = undefined
  >(
    listener: SignalListener<D>
  ): TCallable;

  unsubscribeFromSignals<
    D = undefined
  >(
    listener: SignalListener<D>
  ): void;

  registerQueryProvider<
    T extends QueryType
  >(
    queryType: T,
    listener: QueryListener<T, AnyValue>
  ): TCallable;

  unRegisterQueryProvider<
    T extends QueryType
  >(
    queryType: T,
    listener: QueryListener<T, AnyValue>
  ): void;

  queryDataSync<
    D,
    T extends QueryType,
    Q extends OptionalQueryRequest<D, T>
  >(
    query: Q
  ): QueryResponse<AnyValue>;

  queryDataAsync<
    D,
    T extends QueryType,
    Q extends OptionalQueryRequest<D, T>
  >(
    query: Q
  ): Promise<TQueryResponse<TAnyValue>>;
}
```

### Notes

- **Scope creation**: the scope is automatically created by the Dreamstate ScopeProvider and can
  also be mocked for testing purposes
- **Isolation**: the scope isolates the application from external components, storing all relevant data within
  its context instead of using global state
- **Signal and Query Processing**: all signals and queries are processed within the scope, ensuring
  that only components within the same scope interact with each other
- **Component Access**: React components can access the scope to work with signals and queries,
  facilitating efficient data management and communication
- **Testing and Mocking**: The scope can be accessed directly for testing or mocking purposes,
  providing flexibility during development and unit testing

## Fields

### INTERNAL
- **Type:** `IScopeContextInternals`
- **Description:**  
  Contains library internals. This field is not intended for regular use and is primarily available for
  unit testing and debugging purposes.

## Methods

### getContextOf

- **Description**:
  Retrieves the current context snapshot for the specified manager class.
- **Usage**:
  Use this method to access the state managed by a specific ContextManager within the current scope.
- **Returns**:
  Context of manager class if it is provided or default context value.

### emitSignal

- **Description**:
  Emits a signal for all subscribers within the current Dreamstate scope.
- **Parameters**:
  - base: The base signal object that contains a type and an optional data field.
  - emitter (optional): The reference of the signal emitter.
- **Returns**:
  A signal event instance that wraps the emitted signal.

### subscribeToSignals

- **Description**:
  Subscribes a listener to signal events within the current scope. The listener is invoked on each signal event.
- **Parameters**:
  - listener: A callback function that handles signal events.
- **Returns**:
  A callable function that, when invoked, unsubscribes the listener.

### unsubscribeFromSignals

- **Description**:
  Unsubscribes the specified listener from signal events within the current scope.
- **Parameters**:
  - listener: The callback function to remove from the subscription list.

### registerQueryProvider

- **Description**:
  Registers a query provider callback that will answer query data requests for the specified query type.
- **Parameters**:
  - queryType: The type of query for which the provider will supply data.
  - listener: The callback that processes queries and returns the required data.
- **Returns**:
  A callable function to unregister the query provider when invoked.

### unRegisterQueryProvider

- **Description**:
  Unregisters a previously registered query provider callback so that it no longer handles query data requests.
- **Parameters**:
  - queryType: The type of query for which the provider was registered.
  - listener: The callback to be unregistered.

### queryDataSync

- **Description**:
  Synchronously queries data from the current scope. The handler for the specified query type is executed and its
  result is wrapped as a query response.
- **Parameters**:
  - query: An object representing the query request, including the query type and an optional data field.
- Returns:
  A query response object if a valid handler is found, or null if no handler exists.

### queryDataAsync

- Description:
  Asynchronously queries data from the current scope. The handler for the specified query type is executed,
  and its result is returned as a promise that resolves with a query response.
- Parameters:
  - query: An object representing the query request, including the query type and an optional data field.
- Returns:
  A promise that resolves with the query response if a valid handler is found, or null if no handler exists.
