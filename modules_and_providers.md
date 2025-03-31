---
title: Modules and Providers in NestJS
description: Learn how to organize your NestJS application using modules and providers, and understand best practices for structuring your code.
---

# Modules and Providers in NestJS

NestJS provides a powerful way to organize your application structure using modules and providers. This guide will help you understand these core concepts and how to implement them effectively in your NestJS applications.

## Modules

Modules are the fundamental building blocks of a NestJS application. They help you organize related components and maintain a clear separation of concerns.

### What is a Module?

A module is a class annotated with the `@Module()` decorator. It encapsulates a cohesive set of capabilities within your application.

```typescript
import { Module } from '@nestjs/common';

@Module({
  // module metadata
})
export class MyModule {}
```

The `@Module()` decorator takes a metadata object that describes the module's characteristics:

- `imports`: Other modules that this module depends on
- `controllers`: The controllers defined in this module
- `providers`: The providers that will be instantiated by the Nest injector and that may be shared at least across this module
- `exports`: The subset of providers that should be available in other modules

### Creating a Module

To create a module, use the `@Module()` decorator from the `@nestjs/common` package:

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

This example defines a `CatsModule` that includes a controller and a service.

### Module Organization

Modules help you organize your application into functional units. It's a good practice to have:

1. A root `AppModule`
2. Feature modules for each major feature in your application
3. Shared modules for components used across multiple features

## Providers

Providers are a fundamental concept in NestJS. Many of the basic Nest classes may be treated as providers – services, repositories, factories, helpers, and so on.

### What is a Provider?

A provider is simply a class annotated with the `@Injectable()` decorator. Providers can be injected as dependencies into other classes, allowing you to create a loosely coupled architecture.

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  // service implementation
}
```

### Dependency Injection

NestJS uses dependency injection to manage providers. When you inject a provider into a class, NestJS will handle the instantiation and lifecycle of that provider.

```typescript
import { Controller } from '@nestjs/common';
import { CatsService } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  // controller methods
}
```

In this example, `CatsService` is injected into `CatsController`.

### Custom Providers

While the `@Injectable()` decorator is the most common way to define a provider, NestJS also supports more advanced provider patterns:

1. Value providers
2. Class providers
3. Factory providers
4. Existing providers

These allow for more complex dependency injection scenarios and can be useful in certain architectural patterns.

## Best Practices

1. **Single Responsibility**: Each module should have a single, well-defined responsibility.

2. **Feature Modules**: Create separate modules for distinct features of your application.

3. **Shared Modules**: Use shared modules for services, guards, or pipes that are used across multiple feature modules.

4. **Avoid Circular Dependencies**: Design your modules to avoid circular dependencies. If necessary, use forward references.

5. **Keep Providers Scoped**: By default, providers have a singleton scope. Be mindful of this when designing stateful services.

6. **Use Dependency Injection**: Leverage NestJS's dependency injection system to create loosely coupled, testable code.

7. **Export Wisely**: Only export providers from a module if they need to be used in other modules. This helps maintain encapsulation.

By following these best practices and understanding the concepts of modules and providers, you can create well-structured, maintainable NestJS applications that are easy to test and scale.