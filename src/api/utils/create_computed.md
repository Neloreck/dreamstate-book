# createComputed

### Type

Utility function.

### About

The `createComputed` function creates a computed value that automatically recalculates when the context updates.
This is particularly useful when you have derived data that depends on the current state and should be refreshed
without manual intervention. The computed value is generated via a selector function, with an optional memo
function to determine when recalculation is necessary.

### Call Signature

```typescript
function createComputed<
  T extends TAnyObject,
  C extends TAnyObject
>(
  selector: (context: C) => T,
  memo?: (context: C) => Array<unknown>
): TComputed<T, C>;
```

### Parameters

- **selector**: a function that takes the current context and returns an object representing the computed values
- **memo**: an optional function that returns an array of dependencies. This array is used to decide if the computed 
  value should be recalculated after a context update

### Throws

- **TypeError**: thrown if the computed value is not initialized with a valid functional selector,
  or if the provided memo parameter is not a function

### Notes

- **Automatic Calculation**: designed to automatically compute values rather than requiring manual updates
  each time data is needed
- **Pure Computation**: the computed value should be a simple getter without side effects, always returning an object
- **Exclusion from Updates**: computed values are excluded from the shallow comparisons made during setContext calls,
  ensuring they do not trigger unnecessary updates

### Usage

When you have data that can be computed at runtime and needs to update automatically as the underlying data changes,
createComputed offers an elegant solution.

For example, to compute a value based on an array in the context:

```typescript
class ComputedManager extends ContextManager<IComputedContext> {
  public context: IComputedContext = {
    // Every time any context value changes, computed.greaterThanFive will be recalculated.
    computed: createComputed((context: IComputedContext) => ({
      greaterThanFive: context.numbers.filter((it: number) => it > 5)
    })),
    numbers: [0, 2, 4, 6, 8, 12],
    strings: ["a", "b", "c"]
  };
}
```

If you want the computed value to update only when specific dependencies change, you can add a memo function:

```typescript
class ComputedManager extends ContextManager<IComputedContext> {
  public context: IComputedContext = {
    // Every time the numbers array updates, computed.greaterThanFive will be recalculated.
    computed: createComputed(
      (context: IComputedContext) => ({
        greaterThanFive: context.numbers.filter((it: number) => it > 5)
      }),
      ({ numbers }) => [numbers]
    ),
    numbers: [0, 2, 4, 6, 8, 12],
    someStringValue: "example",
    someNumberValue: 5000,
  };
}
```
