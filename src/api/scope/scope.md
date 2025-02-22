## Scope

### What is scope

In Dreamstate, a **scope** represents an isolated context that manages a set of states, signals, and
queries within a React application. It encapsulates all the context managers and their associated
state, ensuring that updates and changes remain confined to a specific area of your app.

### Purpose

The primary purpose of scopes in Dreamstate is to enhance state management by isolating state,
defining lifetimes, and localizing signals and queries. The idiomatic use of queries, signals,
and isolated stores enables the creation of reusable state containers without circular references or hard dependencies.

### Registry

The **registry** in Dreamstate acts as a centralized store that tracks all active context managers,
listeners, signals, and queries. It facilitates state updates, provisioning, signaling, and subscriptions.

The registry instance is hidden behind the Dreamstate implementation but can be easily accessed for testing or
debugging purposes. Direct runtime access is not recommended and is usually unnecessary.

### Signals

**Signals** are event notifications used within Dreamstate to communicate changes or trigger actions between
different parts of your application. By emitting signals, context managers can inform subscribers about
state changes without creating tight coupling, fostering a more reactive and decoupled architecture.

Signals can be emitted by managers, React components, or virtually any part of the application
that has access to the scope reference.

Similarly, signal subscriptions can be established from any location with a scope reference, including
managers and React components.

### Queries

**Queries** in Dreamstate provide a mechanism to retrieve data from the current scope.
They allow you to access state data without relying on specific implementation details, as they are handled
by matching query response handlers within the current scope.

### Testing

Dreamstate includes testing utilities designed to facilitate isolated testing of your application's state management.
By creating isolated scopes and mocking context managers, you can simulate state transitions, signals, and
queries in a controlled environment, leading to more reliable and maintainable code.
