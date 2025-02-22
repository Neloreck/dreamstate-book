# OnSignal

### Type

Method decorator.

### About

Marks method as signal handler. Every signal event with provided type will be handled by a service instance. <br/>
Services handlers priority is same as instantiation order.

Mainly used to register signal events handlers with generic logic that reacts on application events.

### Call Signature

```typescript
type SignalType = symbol | string | number;

@OnSignal(signalType: Array<SignalType> | SignalType): MethodDecorator;
```

### Parameters

- signalType - specific signal type or array of signal types to be handled by decorated method

### Throws

- TypeError : no signal type parameter(s) present
- TypeError : decorated method does not belong to ContextManager class

### Parameters

- Should be used to mark some service methods as signal event handlers
- Signal handlers are used to notify every service at once about something
- Same approach as with 'reducer-action' but without functional data stores

### Usage

If services need to handle some global events that would require manual action calls each time, signals can be used. <br/>
Also useful when one context service requires reaction from others. <br/>
For example, connection manager can notify other services about state change or network issues. <br/>

```typescript
enum EGlobalSignal {
  CONNECTION_STATE_CHANGE = "CONNECTION_STATE_CHANGE"
}
```

```typescript
export class NetworkManager extends ContextManager {
  public onConnectionStateChanged(connectionState: boolean): void {
    this.emitSignal({ type: "CONNECTION_STATE_CHANGE", data: connectionState });
  }
}
```

Current implementation of 'onConnectionStateChanged' emits signal when being called. Emitted signal contains type and current connection state information.
<br/> <br/>
Manager that handles signal can look like this:

```typescript
type TConnectionStateSignal = SignalEvent<EGlobalSignal.CONNECTION_STATE_CHANGE, boolean>;
```

```typescript
export class HandlerManager extends ContextManager {
  @OnSignal(EGlobalSignal.CONNECTION_STATE_CHANGE)
  public onConnectionStateChanged(signalEvent: TConnectionStateSignal): string {
    if (signalEvent.data) {
      log.info("Network state changed to connected:", signalEvent);
    } else {
      log.info("Network state changed to disconnected:", signalEvent);
    }
  }
}
```

Every time something emits 'CONNECTION_STATE_CHANGE' event, HandlerManager will react with strictly declared logic.
