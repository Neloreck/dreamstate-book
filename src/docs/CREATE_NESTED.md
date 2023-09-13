#### Type
Function.

#### About
Utility for handling of nested data. <br/>
Create nested sub-state for deeper shallow checking. <br/>
Simple manager optimization util with no functional value.

#### Call Signature
```typescript
function createNested<T extends AnyObject>(initialValue: T): Nested<T>;
```

```typescript
interface NestedBase<T> {
  asMerged(state: Partial<T>): Nested<T>;
}
```

```typescript
type Nested<T> = T & NestedBase<T>;
```

#### Parameters
- initialValue - initial state of nested value object

#### Throws
- TypeError - nested stores should be initialized with an object.

#### Notes
- Intention of function is to mark some nested context object as sub-store. Dreamstate will check every field of nested
  object as part of its shallow comparison process before react tree update.

#### Usage
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
- Check context.exampleActions, looks like it is actions store so skipping it
- Check context.example, looks like it is nested, so:
  - Check example.loadable by reference
  - Check example.sampleString by reference
- Check computed, looks like computed so skipping it
- Check basic object by reference
- Check primitive by value
<br/>
<br/>
It means that react tree update will happen after setContext call only after following cases: <br/>
- example.loadable changed
- example.simpleString changed
- basicObject reference changed
- primitive value changed 
<br/>
OR
- context has new key
- example object has new key
