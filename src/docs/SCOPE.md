## Scope

### What is scope

In Dreamstate, a **scope** represents an isolated context that manages a set of states, signals, and
queries within a React application. It encapsulates all the context managers and their associated
state, ensuring that updates and changes remain localized to a specific area of your app.

### Purpose

The primary purpose of using scopes in Dreamstate is to improve state management by isolating states,
strictly defining lifetimes and lifecycle, localizing signals and queries. Idiomatic usage of queries, signals
and isolated stores allow building reusable stores without circular references and hard dependencies.

### Registry

The **registry** in Dreamstate acts as a centralized store that tracks all active context managers,
listeners, signals, and queries. It facilitates state updates, provision, signaling and subscriptions.

**Registry** instance is hidden behind Dreamstate implementation, but can be easily accessed for testing or
debugging purposes. Direct runtime access is not recommended and usually is not needed.

### Signals

**Signals** are event notifications used within Dreamstate to communicate changes or trigger actions between
different parts of your application. By emitting signals, context managers can inform subscribers about
state changes without creating tight coupling, fostering a more reactive and decoupled architecture.

**Signals** can be emitted by managers, React components and basically from anywhere if scope reference is acquired.

**Signals** subscription can be established from any place with scope reference, including managers and React components.

### Queries

**Queries** in Dreamstate provide a mechanism to retrieve data from the current **scope**.
They allow you to get data without relying on specific implementation and are handled by matching query responding
handlers within current **scope**.

### Testing

Dreamstate includes testing utilities designed to facilitate isolated testing of your application's state management.
By creating isolated scopes and mocking context managers, you can simulate state transitions, signals, and
queries in a controlled environment, leading to more reliable and maintainable code.

