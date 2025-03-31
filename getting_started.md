---
title: Getting Started with NestJS
description: A comprehensive guide to help you get started with NestJS, including installation, project setup, and creating a basic application with a simple API endpoint.
---

# Getting Started with NestJS

Welcome to NestJS! This guide will help you set up your development environment, create a new NestJS project, and build a simple API endpoint. By the end of this guide, you'll have a basic understanding of NestJS and be ready to explore more advanced features.

## Prerequisites

Before you begin, make sure you have the following installed on your system:

- Node.js (version 12 or later)
- npm (Node Package Manager, usually comes with Node.js)

## Installation

To get started with NestJS, you'll need to install the Nest CLI. The CLI is a powerful tool that helps you create, develop, and manage your NestJS projects.

Open your terminal and run the following command:

```bash
npm i -g @nestjs/cli
```

This command installs the Nest CLI globally on your system.

## Creating a New Project

Now that you have the Nest CLI installed, you can create a new NestJS project. Run the following command:

```bash
nest new my-nest-project
```

The CLI will prompt you to select a package manager. Choose your preferred option (npm, yarn, or pnpm).

Once the project is created, navigate to the project directory:

```bash
cd my-nest-project
```

## Project Structure

Your new NestJS project will have the following structure:

```
my-nest-project/
├── src/
│   ├── app.controller.spec.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
├── nest-cli.json
├── package.json
├── tsconfig.json
└── README.md
```

The `src` directory contains the core application files:

- `main.ts`: The entry point of the application
- `app.module.ts`: The root module of the application
- `app.controller.ts`: A basic controller with a single route
- `app.service.ts`: A basic service with a single method

## Running the Application

To start your NestJS application, run the following command:

```bash
npm run start
```

This will start the development server. By default, your application will be available at `http://localhost:3000`.

To enable hot-reloading during development, use:

```bash
npm run start:dev
```

## Creating a Simple API Endpoint

Let's create a simple API endpoint to demonstrate how NestJS works. We'll create a "Hello, World!" endpoint.

1. Open `src/app.controller.ts` and replace its contents with the following:

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }

  @Get('hello')
  sayHello(): string {
    return 'Hello, World!';
  }
}
```

2. Open `src/app.service.ts` and replace its contents with the following:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello from NestJS!';
  }
}
```

3. Save the changes and restart your application if it's not running in development mode.

Now, you have two endpoints:

- `http://localhost:3000/`: Returns "Hello from NestJS!"
- `http://localhost:3000/hello`: Returns "Hello, World!"

## Testing Your API

You can test your new API endpoints using a web browser or a tool like cURL or Postman. Here are some example cURL commands:

```bash
curl http://localhost:3000
# Output: Hello from NestJS!

curl http://localhost:3000/hello
# Output: Hello, World!
```

## Next Steps

Congratulations! You've created your first NestJS application with a simple API endpoint. Here are some suggestions for what to explore next:

1. Learn more about [Controllers](https://docs.nestjs.com/controllers) and how they handle incoming requests.
2. Explore [Providers](https://docs.nestjs.com/providers) and [Modules](https://docs.nestjs.com/modules) to organize your application logic.
3. Implement [Dependency Injection](https://docs.nestjs.com/fundamentals/custom-providers) to manage your application's components.
4. Add [Database integration](https://docs.nestjs.com/techniques/database) to persist and retrieve data.
5. Implement [Authentication](https://docs.nestjs.com/security/authentication) and [Authorization](https://docs.nestjs.com/security/authorization) for secure APIs.

Remember to check the official [NestJS documentation](https://docs.nestjs.com/) for in-depth information on these topics and more.

Happy coding with NestJS!