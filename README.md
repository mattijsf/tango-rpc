# Simple TS RPC

This is a TypeScript-based Remote Procedure Call (RPC) library that allows bidirectional communication between two endpoints through a string-based channel. This library works using promises and supports methods with callbacks.

## Key Features

- Type safety with TypeScript
- Ultra simple string-based channel interface:
```typescript
interface Channel = {
    sendMessage(message: string): void
    addMessageListener(listener: (message: string) => void): void
    removeMessageListener(listener: (message: string) => void): void
}
```
- Support for methods with on or more callback parameters.
- Server-side rrror handling


## Installation

```sh
npm install simple-ts-rpc
```
or
```sh
yarn add simple-ts-rpc
```


## Usage

To use this library, you need to define an API interface and a server class implementing the API.

```typescript
interface MyAPI {
  add(a: number, b: number): Promise<number>;
  greet(name: string): Promise<string>;
  processItems(items: string[], callback: (processedItem: string) => void): Promise<void>;
  subscribeToEvents(callback: (event: string) => void): Promise<void>;
  triggerEvent(event: string): Promise<void>;
  errorProne(): Promise<void>;
}

class MyAPIServer implements MyAPI {
  // Implement your API methods here...
}
```

You need to instantiate a `Server` and `Client` with your API and channel.

```typescript
const testChannel = new TestChannel();
const myAPIServer = new MyAPIServer();
const server = new Server<MyAPI>(testChannel, myAPIServer);
const client = new Client<MyAPI>(testChannel);
```

You can use the client's proxy to call API methods as if they were local.

```typescript
const myAPIClient = client.proxy;
myAPIClient.add(1, 2).then(result => console.log(`1 + 2 = ${result}`));
myAPIClient.greet('World').then(result => console.log(result));
myAPIClient.processItems(['apple', 'banana', 'cherry'], item => console.log(`Processed item: ${item}`));
myAPIClient.subscribeToEvents(event => console.log(`Received event: ${event}`));
myAPIClient.triggerEvent('Test event');
myAPIClient.errorProne().catch(error => console.log(`Caught error: ${error.message}`));
```
