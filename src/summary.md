# Summary

- [General](home.md)

- [Pros and cons](benefits.md)

- [Requirements](requirements.md)

- [Usage](usage.md)

- [Changelog](changelog.md)

- [API]()
  - [Scope](api/scope/scope.md)
    - [ScopeContext](api/scope/scope_context.md)
    - [ScopeProvider](api/scope/scope_provider.md)
    - [useScope](api/scope/use_scope.md)

  - [Management]()
    - [ContextManager](api/data_management/context_manager.md)
    - [createProvider](api/data_management/create_provider.md)
    - [useManager](api/data_management/use_manager.md)

  - [Signaling / Queries]()
    - [@OnSignal](api/scope_signalling/on_signal.md)
    - [@OnQuery](api/scope_signalling/on_query.md)

  - [Utils]()
    - [@Bind](api/utils/bind.md)
    - [createActions](api/utils/create_actions.md)
    - [createComputed](api/utils/create_computed.md)
    - [createLoadable](api/utils/create_loadable.md)
    - [createNested](api/utils/create_nested.md)

  - [Testing]()
    - [getCurrent](api/testing/get_current.md)
    - [getReactConsumer](api/testing/get_react_consumer.md)
    - [getReactProvider](api/testing/get_react_provider.md)
    - [mockManager](api/testing/mock_manager.md)
    - [mockRegistry](api/testing/mock_registry.md)
    - [mockScopeProvider](api/testing/mock_scope_provider.md)
    - [mockScope](api/testing/mock_scope.md)
    - [nextAsyncQueue](api/testing/next_async_queue.md)

- [Examples](./examples.md)
  - [Accessing without hooks](./examples/access_without_hook.md)
  - [Decoupling managers](./examples/decoupling_managers.md)
  - [Listen signals from component](./examples/listen_from_component.md)
  - [Provision query from component](./examples/accessing_scope.md)
  - [Provision query from manager](./examples/provision_from_manager.md)
  - [Query data from component](./examples/query_from_component.md)
  - [Query data from manager](./examples/query_from_manager.md)
  - [Send signal from component](./examples/signal_from_component.md)
  - [Send signal from manager](./examples/signal_from_component.md)
  - [Testing components](./examples/testing_components.md)
  - [Testing managers](./examples/testing_managers.md)
  - [Using scope](./examples/accessing_scope.md)
