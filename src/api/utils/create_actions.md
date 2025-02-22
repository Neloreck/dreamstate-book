# createActions

### Type

Function.

### About

The `createActions` function creates an actions store - a container for read-only method references.
This store is visually and programmatically distinct, ensuring that the actions it contains are not
included in shallow comparisons during state updates. This separation helps maintain the immutability
of actions, preventing them from affecting the re-rendering process.

### Call Signature

```typescript
function createActions<T extends AnyObject>(actions: T): Readonly<T>;
```

### Parameters

- **actions**: an object containing the methods exposed by a context manager

### Notes

- **Intention**: this function creates a container that is both visually and programmatically distinguishable
  as a sub-storage for actions. Marking an object as an actions store ensures that it is excluded from
  the shallow comparisons performed during setContext calls, thereby keeping its reference unchanged across updates

### Usage

Dreamstate performs a shallow comparison on every setContext call before updating the React tree.
By marking a specific object as an actions store using createActions, you help the library distinguish
it from generic context fields. As a result, actions objects remain immutable and are not considered
during state update comparisons.

```typescript
export class SampleManager extends ContextManager<ISampleContext> {
  public context: ISampleContext = {
    // The actions field is marked as an actions store.
    // It will not be checked during context updates and will always remain the same.
    sampleActions: createActions({
      incrementSampleNumber: () => this.incrementSampleNumber(),
      setSampleString: (value: string) => this.setSampleString(value)
    }),
    sampleNumber: 0,
    sampleString: "default"
  };

  public setSampleString(value: string): void {
    this.setContext({ sampleString: value });
  }

  public incrementSampleNumber(): void {
    this.setContext(({ sampleNumber }) => {
      return { sampleNumber: sampleNumber + 1 };
    });
  }
}
```
