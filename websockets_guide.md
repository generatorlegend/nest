```yaml
---
title: WebSockets Guide
description: Learn how to implement real-time features using WebSockets in NestJS
---
```

# WebSockets Guide

This guide will walk you through implementing real-time features using WebSockets in NestJS. You'll learn how to set up gateways and handle client-server communication effectively.

## Introduction to WebSockets in NestJS

NestJS provides built-in support for WebSockets, allowing you to create real-time, bidirectional communication between clients and servers. The WebSockets module in NestJS is designed to work seamlessly with the rest of the framework, providing a consistent developer experience.

## Setting Up WebSocket Gateways

WebSocket gateways in NestJS are classes annotated with the `@WebSocketGateway()` decorator. They serve as a bridge between your application and WebSocket clients.

### Creating a Gateway

To create a WebSocket gateway, follow these steps:

1. Create a new file for your gateway (e.g., `chat.gateway.ts`).
2. Import the necessary decorators and interfaces from `@nestjs/websockets`.
3. Define your gateway class and annotate it with `@WebSocketGateway()`.

Here's a basic example:

```typescript
import { WebSocketGateway, WebSocketServer, SubscribeMessage } from '@nestjs/websockets';
import { Server } from 'socket.io';

@WebSocketGateway()
export class ChatGateway {
  @WebSocketServer()
  server: Server;

  @SubscribeMessage('message')
  handleMessage(client: any, payload: any): string {
    return 'Hello, client!';
  }
}
```

In this example, we've created a simple chat gateway that listens for 'message' events and responds with a greeting.

## Handling Client-Server Communication

### Receiving Messages

To handle incoming messages from clients, use the `@SubscribeMessage()` decorator. This decorator takes a string argument representing the event name it should listen for.

```typescript
@SubscribeMessage('chatMessage')
handleMessage(client: any, payload: any): void {
  console.log('Received message:', payload);
  // Process the message
}
```

### Sending Messages

To send messages to connected clients, you can use the `server` property, which is annotated with `@WebSocketServer()`. This property gives you access to the underlying Socket.IO server instance.

```typescript
@WebSocketGateway()
export class ChatGateway {
  @WebSocketServer()
  server: Server;

  sendToAll(message: string) {
    this.server.emit('chatToClient', message);
  }
}
```

## Advanced WebSocket Features

### Namespace and Path Configuration

You can configure the namespace and path for your WebSocket gateway by passing options to the `@WebSocketGateway()` decorator:

```typescript
@WebSocketGateway({
  namespace: '/chat',
  path: '/socket.io'
})
export class ChatGateway {
  // ...
}
```

### Authentication and Guards

To implement authentication for your WebSocket connections, you can use guards. Create a guard that implements the `CanActivate` interface and use it in your gateway:

```typescript
import { CanActivate, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class WsAuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const client = context.switchToWs().getClient();
    // Perform authentication logic
    return true; // or false if authentication fails
  }
}

@UseGuards(WsAuthGuard)
@WebSocketGateway()
export class ChatGateway {
  // ...
}
```

## Conclusion

This guide has covered the basics of implementing WebSockets in NestJS, including setting up gateways, handling client-server communication, and exploring some advanced features. WebSockets provide a powerful way to add real-time functionality to your NestJS applications, enabling you to build interactive and responsive user experiences.

For more detailed information on specific WebSocket features or advanced use cases, refer to the official NestJS documentation or explore the WebSockets module source code.