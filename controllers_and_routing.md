# Controllers and Routing in NestJS

## Introduction

Controllers are a fundamental concept in NestJS, responsible for handling incoming requests and returning responses to the client. They are the main building blocks of your application's routing system, allowing you to define how your application responds to different HTTP methods and URLs.

## Creating a Controller

To create a controller in NestJS, you use the `@Controller()` decorator. This decorator can be used in three ways:

1. Without any arguments
2. With a string or array of strings to define a route prefix
3. With an options object for more advanced configuration

Here's a basic example of a controller:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller()
export class AppController {
  @Get()
  getHello(): string {
    return 'Hello World!';
  }
}
```

## Routing

### Route Prefixes

You can add a route prefix to all routes in a controller by passing a string to the `@Controller()` decorator:

```typescript
@Controller('users')
export class UsersController {
  // ...
}
```

This will prefix all routes in this controller with `/users`.

### HTTP Methods

NestJS provides decorators for all standard HTTP methods:

- `@Get()`
- `@Post()`
- `@Put()`
- `@Delete()`
- `@Patch()`
- `@Options()`
- `@Head()`

Use these decorators on your controller methods to define which HTTP method they respond to:

```typescript
@Controller('users')
export class UsersController {
  @Get()
  findAll(): string {
    return 'This action returns all users';
  }

  @Post()
  create(): string {
    return 'This action adds a new user';
  }

  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `This action returns a #${id} user`;
  }
}
```

### Route Parameters

As shown in the example above, you can use route parameters by adding a colon `:` before the parameter name in your route path. To access these parameters in your method, use the `@Param()` decorator.

### Request Object

You can access the request object by using the `@Req()` decorator:

```typescript
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    console.log(request.url);
    return 'This action returns all cats';
  }
}
```

### Response Formatting

By default, NestJS will automatically serialize the returned value to JSON. You can override this behavior by using the `@Res()` decorator to directly access the response object:

```typescript
import { Controller, Get, Res } from '@nestjs/common';
import { Response } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Res() res: Response) {
    res.status(200).json([{ name: 'mittens' }]);
  }
}
```

## Advanced Controller Options

The `@Controller()` decorator accepts an options object for more advanced configuration:

```typescript
export interface ControllerOptions {
  path?: string | string[];
  host?: string | RegExp | Array<string | RegExp>;
  scope?: Scope;
  version?: string | string[];
}
```

### Host Filtering

You can use the `host` option to filter requests based on the host header:

```typescript
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```

### Versioning

NestJS supports API versioning. You can set the version for all routes in a controller using the `version` option:

```typescript
@Controller({ version: '1' })
export class CatsControllerV1 {
  @Get()
  findAll(): string {
    return 'This action returns all cats (version 1)';
  }
}
```

## Conclusion

Controllers and routing are core concepts in NestJS that allow you to structure your application and handle HTTP requests effectively. By using decorators and leveraging NestJS's powerful routing system, you can create clean, maintainable, and scalable APIs.

For more advanced topics and in-depth information, refer to the official NestJS documentation on [Controllers](https://docs.nestjs.com/controllers) and [Routing](https://docs.nestjs.com/controllers#routing).