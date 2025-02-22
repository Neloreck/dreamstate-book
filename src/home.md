# 🎸 [Dreamstate book (v4)](https://github.com/Neloreck/dreamstate)
[![npm version](https://img.shields.io/npm/v/dreamstate.svg?style=flat-square)](https://www.npmjs.com/package/dreamstate)
[![start with book](https://img.shields.io/badge/docs-book-blue.svg?style=flat)](https://neloreck.github.io/dreamstate-book/)
[![license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://github.com/Neloreck/dreamstate/blob/master/LICENSE)

## Summary

- [Pros and cons](benefits.md)
- [Requirements](requirements.md)
- [Usage](usage.md)
- [Changelog](changelog.md)

## API

  - [Scope](api/scope/scope)
    - [ScopeContext](api/scope/scope_context)
    - [ScopeProvider](api/scope/scope_provider)
    - [useScope](api/scope/use_scope)

  - Management
    - [ContextManager](api/data_management/context_manager.md)
    - [createProvider](api/data_management/create_provider.md)
    - [useManager](api/data_management/use_manager.md)

  - Signaling / Queries
    - [@OnQuery](api/scope_signalling/on_query.md)
    - [@OnSignal](api/scope_signalling/on_signal.md)

  - Utils
    - [@Bind](api/utils/bind.md)
    - [createActions](api/utils/create_actions.md)
    - [createComputed](api/utils/create_computed.md)
    - [createLoadable](api/utils/create_loadable.md)
    - [createNested](api/utils/create_nested.md)

  - Testing
    - [getCurrent](api/testing/get_current.md)
    - [getReactConsumer](api/testing/get_react_consumer.md)
    - [getReactProvider](api/testing/get_react_provider.md)
    - [mockManager](api/testing/mock_manager.md)
    - [mockRegistry](api/testing/mock_registry.md)
    - [mockScopeProvider](api/testing/mock_scope_provider.md)
    - [mockScope](api/testing/mock_scope.md)
    - [nextAsyncQueue](api/testing/next_async_queue.md)

### Examples

#### Applications:

- [simple app](https://github.com/Neloreck/dreamstate/tree/master/examples/simple_application)
- [signals using app](https://github.com/Neloreck/dreamstate/tree/master/examples/signal_application)
- [queries using app](https://github.com/Neloreck/dreamstate/tree/master/examples/query_application)
 
#### Usage:

- [Checking manager disposal status](./examples/checking_disposal_status.md)
- [Decoupling managers logics](./examples/decoupling_managers.md)
- [Dynamic manager provision](./examples/dynamic_manager_provision.md)
- [Forcing manager update](./examples/forcing_manager_update.md)
- [Getting manager scope](./examples/getting_manager_scope.md)
- [Listen signals from component](./examples/listen_from_component.md)
- [Providing initial state](./examples/providing_initial_state.md)
- [Provision query from component](./examples/accessing_scope.md)
- [Provision query from manager](./examples/provision_from_manager.md)
- [Query data from component](./examples/query_from_component.md)
- [Query data from manager](./examples/query_from_manager.md)
- [Reinit whole scope on HMR](./examples/hmr_reinit_scope.md)
- [Send signal from component](./examples/signal_from_component.md)
- [Send signal from manager](./examples/signal_from_manager.md)
- [Testing components](./examples/testing_components.md)
- [Testing managers](./examples/testing_managers.md)
- [Using default context value](./examples/default_context_value.md)
- [Using parts of context](./examples/using_parts_of_context.md)
- [Using scope](./examples/accessing_scope.md)

## Links

- [Dreamstate book](https://neloreck.github.io/dreamstate-book/)
- [Dreamstate repository](https://github.com/Neloreck/dreamstate)
- [Dreamstate npm package](https://www.npmjs.com/package/dreamstate)

## Licence

MIT

