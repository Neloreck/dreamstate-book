### Type
Object interface.

### About
Interface describing current dreamstate scope methods and data. <br/>
All active providers, managers, signals and queries are acting in context of current scope and do not function out of it. <br/>
Usage of current scope can give you access to some specific methods to work with it.

### Interface signature
```typescript
interface ScopeContext {

  /**
   * Lib internals.
   * Should not be used normally except unit testing.
   */
  INTERNAL: IScopeContextInternals;

  /**
   * Get current context snapshot for provided manager class.
   *
   * @param {TAnyContextManagerConstructor} manager - manager which context will be returned
   */
  getContextOf<T extends TAnyObject, D extends IContextManagerConstructor<T>>(manager: D): T;

  /**
   * Emit signal for all subscribers in current dreamstate scope.
   *
   * @param {Object} base - base signal that should trigger signal event for subscribers.
   * @param {TSignalType} base.type - signal type.
   * @param {*=} base.data - signal data that will be received by subscribers.
   * @param {(TAnyContextManagerConstructor | null)=} emitter - signal emitter reference.
   * @returns {Promise} promise that resolves after all handlers execution.
   */
  emitSignal<D = undefined>(base: IBaseSignal<D>, emitter?: TAnyContextManagerConstructor | null): ISignalEvent<D>;

  /**
   * Subscribe to signals in current scope.
   * Following callback will be triggered on each signal with signal event as first parameter.
   *
   * @param {TSignalListener} listener - signals listener callback.
   * @returns {TCallable} unsubscribing function.
   */
  subscribeToSignals<D = undefined>(listener: TSignalListener<D>): TCallable;

  /**
   * Unsubscribe provided callback from signals in current scope.
   * Following callback will not be triggered on scope signals anymore.
   *
   * @param {TSignalListener} listener - signals listener callback.
   */
  unsubscribeFromSignals<D = undefined>(listener: TSignalListener<D>): void;

  /**
   * Register callback as query provider and answer query data calls with it.
   *
   * @param {TQueryType} queryType - type of query for data provisioning.
   * @param {TQueryListener} listener - callback that will listen data queries and return evaluation data.
   * @returns {TCallable} function that unsubscribes provided callback.
   */
  registerQueryProvider<T extends TQueryType>(queryType: T, listener: TQueryListener<T, any>): TCallable;

  /**
   * Unregister callback as query provider and answer query data calls with it.
   *
   * @param {TQueryType} queryType - type of query for data provisioning.
   * @param {TQueryListener} listener - callback that will be unsubscribed from listening.
   */
  unRegisterQueryProvider<T extends TQueryType>(queryType: T, listener: TQueryListener<T, any>): void;

  /**
   * Query data from current scope in a sync way.
   * Handler that listen for provided query type will be executed and return value will be wrapped
   * and returned as QueryResponse.
   * Mainly used to get specific data in current context or receive current state of specific data without direct
   * referencing to it.
   *
   * @param {IOptionalQueryRequest} query - optional query request base for data retrieval, includes query type and
   *  optional data field.
   * @returns {TQueryResponse} response for provided query or null value if no handlers were found.
   */
  queryDataSync<D extends any, T extends TQueryType, Q extends IOptionalQueryRequest<D, T>>(query: Q): TQueryResponse<any>;

  /**
   * Query data from current scope in an sync way.
   * Handler that listen for provided query type will be executed and return value will be wrapped
   * and returned as QueryResponse.
   * Mainly used to get specific data in current context or receive current state of specific data without direct
   * referencing to it.
   *
   * @param {IOptionalQueryRequest} query - optional query request base for data retrieval, includes query type and
   *  optional data field.
   * @returns {Promise} response for provided query or null value if no handlers were found.
   */
  queryDataAsync<D extends any, T extends TQueryType, Q extends IOptionalQueryRequest<D, T>>(query: Q): Promise<TQueryResponse<any>>;

}
```

### Parameters
- Scope is created automatically by dreamstate ScopeProvider or can be mocked for tests
- Scope is used to isolate application from other components and store all the data in it instead of globals
- Scope is environment where signals and queries are processed
- Scope can be accessed by react components to work with signals and queries
- Scope can be accessed to test or mock some data
