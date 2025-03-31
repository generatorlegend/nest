---
title: Dependency Injection Guide
description: A comprehensive guide to understanding and using dependency injection in NestJS applications
---

# Dependency Injection Guide

Dependency Injection (DI) is a fundamental concept in NestJS that allows you to write more modular, testable, and maintainable code. This guide will help you understand how DI works in NestJS and how to use it effectively in your applications.

## Table of Contents

1. [Introduction to Dependency Injection](#introduction-to-dependency-injection)
2. [Injectable Decorator](#injectable-decorator)
3. [Injection Scopes](#injection-scopes)
4. [Inject Decorator](#inject-decorator)
5. [Module-based Dependency Injection](#module-based-dependency-injection)
6. [Best Practices](#best-practices)

## Introduction to Dependency Injection

Dependency Injection is a design pattern where objects receive their dependencies from an external source rather than creating them internally. In NestJS, the DI system is built-in and manages the creation and lifecycle of application components.

## Injectable Decorator

The `@Injectable()` decorator is used to mark a class as a provider that can be managed by the NestJS DI container. Here's how you can use it:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class MyService {
  // Service implementation
}
```

When you mark a class with `@Injectable()`, NestJS can inject this service into other classes that depend on it.

## Injection Scopes

NestJS provides different injection scopes that determine the lifetime of a provider instance. You can specify the scope when using the `@Injectable()` decorator:

```typescript
import { Injectable, Scope } from '@nestjs/common';

@Injectable({ scope: Scope.TRANSIENT })
export class MyTransientService {
  // This service will have a new instance for each injection
}
```

Available scopes are:
- `Scope.DEFAULT`: Singleton (shared) instance across the entire application
- `Scope.TRANSIENT`: New instance created for each injection
- `Scope.REQUEST`: New instance created for each request (in HTTP server applications)

## Inject Decorator

The `@Inject()` decorator is used to specify dependencies explicitly. While NestJS can often infer dependencies based on types, there are cases where you need to use `@Inject()`:

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class MyService {
  constructor(@Inject('CONFIG') private config: any) {}
}
```

This is particularly useful when working with custom providers or when you need to inject a specific token.

## Module-based Dependency Injection

NestJS organizes the application into modules, which play a crucial role in managing dependencies. The `@Module()` decorator is used to define a module:

```typescript
import { Module } from '@nestjs/common';
import { MyService } from './my.service';
import { MyController } from './my.controller';

@Module({
  providers: [MyService],
  controllers: [MyController],
  exports: [MyService],
})
export class MyModule {}
```

In this example:
- `MyService` is provided within the module scope
- `MyController` can inject `MyService`
- `MyService` is exported, making it available for injection in other modules that import `MyModule`

## Best Practices

1. Use constructor injection whenever possible for better testability:

```typescript
@Injectable()
export class MyService {
  constructor(private readonly dependencyService: DependencyService) {}
}
```

2. Avoid circular dependencies by using forward references or restructuring your code.

3. Use custom providers for more complex scenarios, such as dynamic providers or non-class-based dependencies.

4. Leverage NestJS's hierarchical injection system by organizing your application into feature modules.

5. Use the appropriate injection scope based on your application's needs, defaulting to singleton scope unless you have a specific reason to use a different scope.

By following these guidelines and understanding the core concepts of Dependency Injection in NestJS, you can build more maintainable and scalable applications.