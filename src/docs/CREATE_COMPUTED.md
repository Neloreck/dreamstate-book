#### Type
Utility function.

#### About
Function to create computed value. <br/>
Useful when something can be re-calculated automatically on manager updates.

#### Call Signature
```typescript
function createComputed<T extends TAnyObject, C extends TAnyObject>(selector: (context: C) => T, memo?: (context: C) => Array<any>): TComputed<T, C>;
```

#### Parameters
- selector - selector function that generates computed values container object
- memo - optional memo selector, indicates whether computed value should be re-calculated after context update

#### Throws
- TypeError - computed value should be initialized with functional selector and optional memo function.

#### Notes
- Intention of function is to calculate computed values automatically instead of doing additional work every time you need some generic data.
- Computed value should return object
- Computation should not do any side effects, just simple getter
- Computed values are excluded on 'setContext' shallow comparison

#### Usage
In case if some value can be computed in runtime and should be re-calculated on some data changes, it can be automated: <br/>

```typescript
class ComputedManager extends ContextManager<IComputedContext> {

  public context: IComputedContext = {
    // Every time numbers array update, computed.greaterThanFive will be updated accordingly.
    computed: createComputed((context: IComputedContext) => ({
      greaterThanFive: context.numbers.filter((it: number) => it > 5)
    })),
    numbers: [ 0, 2, 4, 6, 8, 12 ]
  };

}
```
In case if updates should happen only with dependency on some field, memo check can be added:
```typescript
class ComputedManager extends ContextManager<IComputedContext> {

  public context: IComputedContext = {
    // Every time numbers array update, computed.greaterThanFive will be updated accordingly.
    computed: createComputed((context: IComputedContext) => ({
      greaterThanFive: context.numbers.filter((it: number) => it > 5)
    }), ({ numbers }) => [ numbers ]),
    numbers: [ 0, 2, 4, 6, 8, 12 ],
    someStringValue: "example",
    someNumberValue: 5000,
  };

}
```
