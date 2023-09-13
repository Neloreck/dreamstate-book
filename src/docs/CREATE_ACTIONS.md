#### Type
Utility function.

#### About
Function to create actions store.
Actions store is readonly methods links containing object.

#### Call Signature
```typescript
function createActions<T extends AnyObject>(actions: T): Readonly<T>;
```

#### Parameters
- actions - object containing context manager exposed methods

#### Notes
- Intention of this function is to create container that is visually and programmatically distinguishable as actions containing sub-storage

#### Usage
Normally dreamstate does shallow comparison on every 'setContext' call before updating react tree.
Marking specific object as 'actions' will help library to distinguish it from generic context fields. 
As result, actions objects will never be accounted in comparison before update.

```typescript
export class SampleManager extends ContextManager<ISampleContext> {

  public context: ISampleContext = {
    /**
     * Marked field as actions object, it will never affect context updates and always will remain same.
     */
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
