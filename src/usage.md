# Usage

### Install package

```bash
npm install dreamstate
```

### Create a ContextManager

Create a manager by extending the ContextManager class.\
This manager defines your initial context and any logic.

```typescript
import { ContextManager, createActions } from 'dreamstate';

export interface ISimpleContext {
  simpleActions: {
    increment(): void;    
  }
  value: number;
}

export class SimpleManager extends ContextManager<ISimpleContext> {
  public context: ISimpleContext = {
    simpleActions: createActions({
      increment: () => this.increment(),
    }),
    value: 0,
  };

  public increment(): void {
    this.setContext(({ value }) => ({ value: value + 1 }));
  }
}
```

### Create a Provider

Generate a React provider using the createProvider function, passing your managers classes as arguments.

```typescript
import { createProvider } from 'dreamstate';

import { SimpleManager } from './SimpleManager';

export const Provider = createProvider(SimpleManager);
```

### Wrap your React tree with Scope and Context providers

Wrap your application (or a portion of it) with the provider to make the manager's context available to all child components.

```tsx
import { ScopeProvider } from "dreamstate";
import React, { ReactElement } from "react";

import { Provider } from "./Provider";
import { ExampleComponent } from "./ExampleComponent";

export function Application(): ReactElement {
  return (
    <ScopeProvider>
      <Provider>
        <ExampleComponent/>
      </Provider>
    </ScopeProvider>
  );
}
```

### Use your context and update it

Later you can access your manager context data from component and call manager actions in hooks/event handlers.

```tsx
import { useManager } from "dreamstate";
import React, { ReactElement } from "react";

import { SimpleManager } from "./SimpleManager";

export function ExampleComponent({
  simpleContext: { value, simpleActions } = useManager(SimpleManager),
}): ReactElement {
  return (
    <div>
      <div>
        value: {value}
      </div>
      <div>
        <button onClick={simpleActions.increment}>
          increment
        </button>
      </div>
    </div>
  );
}
```

## Advanced usage

For advanced usage check API documentation and examples sections in doc book.
