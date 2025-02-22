### Type

Utility function.

### About

The `createNested` function is a utility for handling nested data within a Dreamstate context.
It creates a nested sub-state designed for deeper shallow checking, allowing individual fields of
a nested object to be compared during state updates. This optimization utility does not add functional
value by itself but helps Dreamstate determine which parts of the nested state have changed.

### Call Signature

```typescript
function createNested<T extends AnyObject>(
  initialValue: T
): Nested<T>;
```

```typescript
interface NestedBase<T> {
  asMerged(state: Partial<T>): Nested<T>;
}
```

```typescript
type Nested<T> = T & NestedBase<T>;
```

### Parameters

- **initialValue**: the initial state of the nested value object. This must be an object because nested stores
  are intended to hold state that will be compared field by field during context updates

### Throws

- TypeError: thrown if the nested store is initialized with a value that is not an object

### Notes

The primary purpose of createNested is to mark a nested context object as a sub-store. Dreamstate will include
every field of this nested object in its shallow comparison process before updating the React tree,
rather than only comparing the object reference. This means that changes to individual properties within
the nested object will trigger re-renders appropriately, without needing to replace the entire nested object.

### Usage

For example, if we want nested value to be shallow checked every time instead of only 'example' object reference check,
we can write following code:

```typescript
export class ManagerWithUtils extends ContextManager<IContextWithUtils> {
  public readonly context: IContextWithUtils = {
    exampleActions: createActions({}),
    example: createNested({
      loadable: createLoadable(10),
      simpleString: "10"
    }),
    computed: createComputed((context: IContextWithUtils) => ({
      booleanSwitch: context.nested.simpleString.length > 2,
      concatenated: context.nested.simpleString + context.nested.simpleString + "!"
    })),
    basicObject: {
      first: 1,
      second: 2  
    },
    primitive: 10,
  };
}
```

How comparison will happen after "setContext call": <br/>
- Check `context.exampleActions`, looks like it is actions store so skipping it
- Check `context.example`, looks like it is nested, so:
  - Check `example.loadable` by reference
  - Check `example.sampleString` by reference
- Check `computed`, looks like computed so skipping it
- Check `basicObject` by reference
- Check `primitive` by value
<br/>
<br/>
It means that react tree update will happen after setContext call only after following cases: <br/>
- example.loadable changed
- example.simpleString changed
- basicObject reference changed
- primitive value changed 
<br/>
OR
- `context` has new key added
- `example` has new key added
