# OnQuery

### Type

Method decorator.

### About

Marks method as query handler. Every query request with provided type will be handled by service instance method. <br/>
Services handlers priority is same as instantiation order.

Mainly used to register query requests handlers with generic getter logic that does some job and returns results.

### Call Signature

```typescript
type QueryType<T extends string = string> = symbol | string | number | T;

interface QueryRequest<D extends any = undefined, T extends QueryType = QueryType> {
  type: T;
  data?: D;
}

@OnQuery(queryType: QueryType): MethodDecorator;
```

### Parameters

- queryType - specific query type to be handled by decorated method

### Throws

- TypeError : no query type parameter(s) present
- TypeError : decorated method does not belong to ContextManager class

### Parameters

- Should be used to mark some service getter methods as query handlers
- Handlers can be both sync and async
- Queries should be simple getters without side effects

### Usage

If few services need some data that should not be observed or handled by another class, but is needed for one-time operation, queries can help. <br/>
<br/>
As an example, some service has method that logs some important data.
It is not important where it is placed and who handles it, only specific data is needed. <br/>

```typescript
import { QueryResponse, ContextManager } from "dreamstate";

export class LoggingService extends ContextManager {
  public logImportantData(): void {
    const information: QueryResponse<string> = this.queryDataSync({ type: "IMPORTANT_DATA" });

    if (information) {
      console.info("Got some data for you:", information.data);
    } else {
      console.info("Could not get data, no one responded.");
    }
  }
}
```

Current implementation of 'logImportantData' method tries to get some data, but it cannot guarantee if anyone will respond. <br/>
@OnQuery decorated methods always return value packed in a 'QueryResponse' object.
<br/> <br/>
Manager with important data that handles query can look like this:

```typescript
import { QueryRequest, OnQuery, ContextManager } from "dreamstate";

export class ImportantDataService extends ContextManager {
  @OnQuery("IMPORTANT_DATA")
  public onImportantDataRequested(queryRequest: QueryRequest<void>): string {
    return "Really important data!";
  }
}
```
Or it could be event simple react component:

```typescript
import { QueryRequest, OnQuery, ContextManager, useScope, ScopeContext } from "dreamstate";
// ...

export function SomeComponent(): ReactElement {
  const scope: ScopeContext = useScope();

  useEffect(() => {
    return scope.registerQueryProvider("IMPORTANT_DATA", () => {
      return "Some important data.";  
    })
  }, []);
  
  return "Some label";
}
```

So, if instance of ImportantDataService is provided (or component is rendered) by the moment when LoggingService needs some important data, it will respond on the query.

##### With parameters:

Query parameters can be provided when sending query:

```typescript
export class LoggingService extends ContextManager<{}> {

  public context: {} = {};

  public logMultipliedData(): void {
    const multiplied: QueryResponse<number> = this.queryDataSync({
      type: "MULTIPLIED_BY_TWO",
      data: 16
    });

    if (multiplied) {
      console.info("Multiplied value:", information.data); // 32
    } else {
      console.info("Could not get data, no one responded.");
    }
  }

}
```

```typescript
export class MultiplicationService extends ContextManager {

  @OnQuery("MULTIPLIED_BY_TWO")
  public onImportantDataRequested(queryRequest: QueryRequest<number>): number {
    return queryRequest.data * 2;
  }

}
```

##### Async:

If getter should be async, alternative would look as simple as sync one:

```typescript
export class LoggingService extends ContextManager {

  public async logImportantData(): Promise<void> {
    const information: QueryResponse<string> = await this.queryDataAsync({ type: "IMPORTANT_DATA" });

    if (information) {
      console.info("Got some data for you:", information.data); // "Really important data!"
    } else {
      console.info("Could not get data, no one responded.");
    }
  }

}
```
