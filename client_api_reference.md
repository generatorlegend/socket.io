---
title: Socket.IO Client API Reference
sidebar_label: API Reference
slug: /client-api
---

# Socket.IO Client API Reference

This page provides a comprehensive reference for the Socket.IO client API, detailing all public methods, events, and options available for the Manager and Socket classes.

## Manager

The `Manager` class is responsible for managing the connection to the server and creating `Socket` instances.

### Constructor

```typescript
new Manager(uri?: string | Partial<ManagerOptions>, opts?: Partial<ManagerOptions>)
```

Creates a new Manager instance.

- `uri` (optional): The URI of the Socket.IO server.
- `opts` (optional): An object with options for the manager.

### Options

The `ManagerOptions` interface extends `EngineOptions` and includes the following properties:

- `forceNew` (boolean): Whether to create a new Manager instance. Default is `false`.
- `multiplex` (boolean): Whether to reuse an existing Manager instance. Default is `true`.
- `path` (string): The path to get the client file from. Default is `/socket.io`.
- `reconnection` (boolean): Whether to allow reconnections. Default is `true`.
- `reconnectionAttempts` (number): The number of reconnection attempts. Default is `Infinity`.
- `reconnectionDelay` (number): The initial delay between reconnection attempts in milliseconds. Default is `1000`.
- `reconnectionDelayMax` (number): The maximum delay between reconnection attempts in milliseconds. Default is `5000`.
- `randomizationFactor` (number): The randomization factor used in the exponential backoff jitter. Default is `0.5`.
- `timeout` (number): The timeout for the connection attempt in milliseconds. Default is `20000`.
- `autoConnect` (boolean): Whether to automatically connect. Default is `true`.
- `parser` (any): The parser to use. Defaults to an instance of the Parser that ships with Socket.IO.

### Methods

#### `open()`

Opens the connection to the server.

#### `reconnection(value?: boolean)`

Sets or gets the `reconnection` option.

#### `reconnectionAttempts(value?: number)`

Sets or gets the `reconnectionAttempts` option.

#### `reconnectionDelay(value?: number)`

Sets or gets the `reconnectionDelay` option.

#### `reconnectionDelayMax(value?: number)`

Sets or gets the `reconnectionDelayMax` option.

#### `timeout(value?: number)`

Sets or gets the `timeout` option.

#### `connect()`

Alias for `open()`.

#### `socket(nsp: string, opts?: Partial<SocketOptions>)`

Creates a new `Socket` instance for the given namespace.

## Socket

The `Socket` class represents a connection to a specific namespace on the server.

### Constructor

```typescript
new Socket(io: Manager, nsp: string, opts?: Partial<SocketOptions>)
```

Creates a new Socket instance.

- `io`: The Manager instance.
- `nsp`: The namespace.
- `opts` (optional): An object with options for the socket.

### Options

The `SocketOptions` interface includes the following properties:

- `auth`: Authentication data to be sent when connecting.

### Methods

#### `connect()`

Connects the socket.

#### `open()`

Alias for `connect()`.

#### `send(...args: any[])`

Sends a `message` event.

#### `emit<Ev extends EventNames<EmitEvents>>(ev: Ev, ...args: EventParams<EmitEvents, Ev>)`

Emits an event to the server.

#### `disconnect()`

Disconnects the socket.

#### `close()`

Alias for `disconnect()`.

#### `compress(compress: boolean)`

Sets the compress flag for subsequent event emissions.

#### `timeout(timeout: number)`

Sets a timeout for the next emitted event.

#### `onAny(listener: (...args: any[]) => void)`

Adds a listener that will be fired when any event is emitted.

#### `prependAny(listener: (...args: any[]) => void)`

Adds a listener that will be fired when any event is emitted (added to the beginning of the listeners array).

#### `offAny(listener?: (...args: any[]) => void)`

Removes the specified listener or all listeners for any event.

#### `listenersAny()`

Returns an array of listeners that are listening for any event.

#### `onAnyOutgoing(listener: (...args: any[]) => void)`

Adds a listener that will be fired when any event is emitted by the socket.

#### `prependAnyOutgoing(listener: (...args: any[]) => void)`

Adds a listener that will be fired when any event is emitted by the socket (added to the beginning of the listeners array).

#### `offAnyOutgoing(listener?: (...args: any[]) => void)`

Removes the specified listener or all listeners for any outgoing event.

#### `listenersAnyOutgoing()`

Returns an array of listeners that are listening for any outgoing event.

### Events

The `Socket` class emits the following events:

- `connect`: Fired upon connecting.
- `connect_error`: Fired upon a connection error.
- `disconnect`: Fired upon disconnection.

### Properties

- `connected` (boolean): Whether the socket is currently connected to the server.
- `disconnected` (boolean): Whether the socket is currently disconnected from the server.
- `id` (string): A unique identifier for the socket session.

## Utility Types

The Socket.IO client API includes several utility types to improve type safety:

- `EventNames<EmitEvents>`: Represents the names of events that can be emitted.
- `EventParams<EmitEvents, Ev>`: Represents the parameters of a specific event.
- `DecorateAcknowledgements<E>`: Utility type to decorate acknowledgement callbacks with a timeout error.

These types help in providing better type inference and catching potential errors at compile-time when using TypeScript.