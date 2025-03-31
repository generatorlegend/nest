---
title: Middleware Usage in NestJS
---

# Middleware Usage in NestJS

Middleware functions are functions that have access to the request and response objects, and the next middleware function in the application's request-response cycle. This guide will walk you through how to use middleware in NestJS applications, including built-in middleware, custom middleware, and global middleware configuration.

## Table of Contents

1. [Introduction to Middleware](#introduction-to-middleware)
2. [Built-in Middleware](#built-in-middleware)
3. [Custom Middleware](#custom-middleware)
4. [Applying Middleware](#applying-middleware)
5. [Global Middleware](#global-middleware)
6. [Middleware Consumer](#middleware-consumer)
7. [Excluding Routes](#excluding-routes)

## Introduction to Middleware

Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

In NestJS, middleware can be function-based or class-based. Let's explore how to implement and use both types.

## Built-in Middleware

NestJS provides several built-in middleware functions that you can use out of the box. Some examples include:

- `json()`: Parses JSON request bodies
- `urlencoded()`: Parses URL-encoded request bodies
- `cors()`: Enables Cross-Origin Resource Sharing (CORS)

To use built-in middleware, you can apply them in your `main.ts` file:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.use(express.json());
  app.use(express.urlencoded({ extended: true }));
  await app.listen(3000);
}
bootstrap();
```

## Custom Middleware

You can create custom middleware as either a function or a class.

### Function Middleware

Function middleware is a simple function that takes three arguments: `req`, `res`, and `next`.

```typescript
import { Request, Response, NextFunction } from 'express';

export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
}
```

### Class Middleware

Class middleware implements the `NestMiddleware` interface:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

## Applying Middleware

To apply middleware, use the `configure()` method of a module:

```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```

## Global Middleware

To apply middleware globally, use the `use()` method of the NestJS application instance:

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```

## Middleware Consumer

The `MiddlewareConsumer` is a helper class that provides several built-in methods to manage middleware. Some key methods include:

- `apply()`: Specifies the middleware to be used
- `forRoutes()`: Specifies the routes to which the middleware should be applied
- `exclude()`: Excludes specific routes from middleware application

Here's an example of using these methods:

```typescript
configure(consumer: MiddlewareConsumer) {
  consumer
    .apply(LoggerMiddleware)
    .exclude(
      { path: 'cats', method: RequestMethod.GET },
      { path: 'cats', method: RequestMethod.POST }
    )
    .forRoutes(CatsController);
}
```

## Excluding Routes

You can exclude specific routes from middleware application using the `exclude()` method:

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST }
  )
  .forRoutes(CatsController);
```

This configuration will apply the `LoggerMiddleware` to all routes in `CatsController`, except for GET and POST requests to the 'cats' route.

By understanding and effectively using middleware in your NestJS applications, you can add powerful request processing capabilities and enhance your application's functionality.