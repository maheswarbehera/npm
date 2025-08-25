# node-js-api-response


*This package provides custom API response and error-handling middleware for Express.js applications. It helps standardize error handling, API responses, and logging based on the environment.*


[![node-js-api-response](https://img.shields.io/npm/v/node-js-api-response?label=node-js-api-response&color=purple&logo=npm)](https://www.npmjs.com/package/node-js-api-response)
[![npm downloads](https://img.shields.io/npm/dm/node-js-api-response.svg?color=green&logo=npm)](https://www.npmjs.com/package/node-js-api-response)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## 🛠 Tech Stack

[![Node.js](https://img.shields.io/badge/Node.js-18+-green?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express.js-4+-lightgrey?logo=express&logoColor=white)](https://expressjs.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow?logo=javascript)](https://developer.mozilla.org/docs/Web/JavaScript)
[![TypeScript](https://img.shields.io/badge/TypeScript-5+-blue?logo=typescript)](https://www.typescriptlang.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-6+-green?logo=mongodb)](https://www.mongodb.com/)
[![Mongoose](https://img.shields.io/badge/Mongoose-7+-red?logo=mongoose)](https://mongoosejs.com/)


---

## 🔗 Related Projects

You can also check out our **ns-init CLI**  for scaffolding Node.js + Express servers:   

[![ns-init](https://img.shields.io/npm/v/ns-init?label=ns-init&color=purple&logo=npm)](https://www.npmjs.com/package/ns-init)
[![npm downloads](https://img.shields.io/npm/dm/ns-init.svg?color=green&logo=npm)](https://www.npmjs.com/package/ns-init)
 
*A lightweight CLI tool to quickly scaffold Node.js + Express server projects with JavaScript or TypeScript templates, including optional logger integration.*


## 📢 What’s New in the Latest Release?

### 🎉 Our latest version now includes:

#### 📝 Winston-based Logger (console + file logging)
#### 🌐 Request/Response Logger Middleware with User-Agent support (browser, OS, device type)
#### 🐞 Bug Fixes & Improvements for better stability and smoother developer experience
👉 Update to the newest version with:


```
npm i node-js-api-response@latest
```

## Features

- **Custom error handling** using the `BaseError` class
- **Standardized API response format** using the `HttpSuccessResponse` class
- **Error logging** based on the environment (development/production)
- **Error handling middleware** (`globalErrorHandler`) for consistent error responses
- **Asynchronous route handling** with `asyncHandler` to catch unhandled promise rejections

---

## Installation

To install the package:

```bash
npm install node-js-api-response

OR

yarn add node-js-api-response
```

---

## `asyncHandler` for Express.js

`asyncHandler` is a utility function that helps manage asynchronous route handlers in Express.js applications. It automatically catches any errors and forwards them to the next middleware function, simplifying error handling for async code.

### Features

- Automatically handles asynchronous errors in Express.js route handlers.
- Eliminates the need for `try-catch` blocks for each route.
- Catches unhandled promise rejections and forwards them to error middleware.

The `asyncHandler` function is predefined as follows:

```javascript
import { Request, Response, NextFunction, RequestHandler } from 'express';

export const asyncHandler = (fn: (req: Request, res: Response, next: NextFunction) => Promise<any>): RequestHandler => {
    return (req: Request, res: Response, next: NextFunction) => {
        Promise.resolve(fn(req: Request, res: Response, next: NextFunction)).catch((err) => next(err));
    }
}
```

### Usage

`asyncHandler` wraps asynchronous route handlers in Express.js to ensure errors are forwarded to the error-handling middleware.

#### Basic Example
Wrap your async route handlers so you can stop writing repetitive try/catch blocks.
```javascript
import express, { Application, NextFunction, Request, Response } from "express";
import { asyncHandler } from 'node-js-api-response'; // Install via npm package

const app: Application = express(); 

// A sample asynchronous route using asyncHandler
app.get('/async-route', asyncHandler(async (req: Request, res: Response, next: NextFunction) => {
    // Simulating async operation, e.g., fetching data from a database
    const data = await fetchDataFromDatabase(); // Placeholder for async code
    res.json(data);  // Send data as JSON response
}));

// Default route to test
app.get('/', (req: Request, res: Response, next: NextFunction) => {
    res.send('Welcome to the API!');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});

export default app;
```

---

## API Response Helper for Express.js

The `HttpSuccessResponse` class extends `HttpBaseResponse` class and `SuccessResponse` function help standardize successful API responses in your Express.js application. They ensure that all successful responses follow a consistent structure, making your API easier to maintain and more predictable for clients.

### Features

- Simplifies sending successful API responses with a consistent format.
- Includes `statusCode`, `message`, `data`, and `status` properties in the response.
- Easy to integrate into your Express.js route handlers.

### Usage

You can use `HttpSuccessResponse` and `SuccessResponse` to send structured, consistent responses.

#### Example: Send a Successful Response
### Choose One:
```ts
new HttpSuccessResponse(...): You manually send the response.

SuccessResponse(...): Utility that handles res.status().json(...) internally.
```

```javascript
import { HttpSuccessResponse, SuccessResponse } from 'node-js-api-response'; 

app.get('/api/v1/success', (req: Request, res: Response, next: NextFunction) => {
  // Data to return in the response
  const data = {
    user: { name: 'John Doe', age: 30 },
  };

  // Send a success response
  // Option 1: Use HttpSuccessResponse class
  //res.status(200).json(new HttpSuccessResponse(200, data, "Request successful"));

  // Option 2: Use SuccessResponse utility function
  return SuccessResponse(res, 200, 'Request successful', data);
});
```
**Result:**

```json
{
  "statusCode": 200,
  "status": true,
  "message": "Request successful",
  "data": {
    "user": { "name": "John Doe", "age": 30 }
  }
}
```

---

## `globalErrorHandler` Middleware

The `globalErrorHandler` middleware for Express.js is a custom middleware designed to handle errors in a centralized way. It formats the error response and logs the details based on the environment (development or production). This ensures that errors are properly handled and logged, while providing clients with a consistent error response format.

### Features

- Customizable error format: Sends a standardized error response with `statusCode`, `message`, `status`, and `name`.
- **Development-Mode Logging**: Logs detailed error stack traces in development mode to help with debugging.
- **Production Logging**: In production, logs basic error information without exposing sensitive stack traces.
- Seamless integration with `HttpErrorResponse` or any other error class.
- Environment-based behavior: Modifies logging behavior based on the `NODE_ENV` environment variable.

`package.json` 
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "set NODE_ENV=development&& nodemon src/index.js",
    "prod": "set NODE_ENV=production&& nodemon src/index.js"
  },
```

Ensure that the required dependencies (`HttpErrorResponse`) are imported correctly into your application.

### Usage

You can use the `globalErrorHandler` middleware in your Express.js application to handle errors uniformly across your routes.

#### Example:

```javascript
import { globalErrorHandler, } from 'node-js-api-response'; 

// Use the error handler middleware at last
app.use(globalErrorHandler); // Place this after all routes
export default app;
```
It catches all thrown or passed errors and sends a structured response:
```json
{
  "status": false,
  "statusCode": 400,
  "message": "This is a test error"
}
```
---

## API Error Response Helper for Express.js

The `HttpErrorResponse` class extends `BaseError` class and `ErrorResponse` function help handle errors in your Express.js application in a consistent and structured manner. These utilities are designed to make it easier to manage error responses, especially when dealing with asynchronous code and middleware.

### Features

- `BaseError` Class: A custom error class that extends the built-in `Error` class to represent API errors with an HTTP status code and message.
- `ErrorResponse` Function: A utility function that creates an `HttpErrorResponse` instance and passes it to the next middleware or throws an error if no next function is provided.

### Usage

You can use `HttpErrorResponse` and `ErrorResponse` in your Express.js application to handle errors consistently.

#### Example: Handle Errors in Express.js Routes

- `HttpErrorResponse`: manually create and pass to next()

- `ErrorResponse`: utility that does it for you

- Use one or the other, not both

```javascript
import ErrorDefinitions, { HttpErrorResponse, ErrorResponse } from 'node-js-api-response'; 

// A sample route that throws an error
app.get('/api/v1/error', (req: Request, res: Response, next: NextFunction) => {
  // Trigger an API error response with a 400 status code
  // Option 1: Use HttpErrorResponse class
  // next(new HttpErrorResponse(400, 'Bad request: this is a test error'))

  // Option 2: Use ErrorResponse helper function
  // return ErrorResponse(next, 400, 'Bad request: this is a test error', BAD_REQUEST);
  // ErrorResponse 4th parameter optional
  return ErrorResponse(next, ErrorDefinitions.BAD_REQUEST.httpCode || 400, ErrorDefinitions.BAD_REQUEST.message || "custom message",  ErrorDefinitions.BAD_REQUEST.errorCode);
});

// Example route to simulate an internal server error
app.get('/api/v1/server-error', (req: Request, res: Response, next: NextFunction) => {
  // Trigger a 500 error for internal server problems
  return ErrorResponse(next, 500, 'Internal server error occurred' );
});
```
**Result:**

In production Response looks like
```json
{
  "statusCode": 400,
  "status": false,
  "message": "Bad request: this is a test error",
}
{
  "statusCode": 500,
  "status": false,
  "message": "Internal server error occurred",
}

```
### Development Mode (NODE_ENV=development)
Includes stack trace:
```json
{
  "statusCode": 400,
  "status": false,
  "message": "Bad request: this is a test error",
  "name": "BadRequestError",
  "errorCode": "BAD_REQUEST",
  "stack": "BadRequestError: Bad request: this is a test error at ErrorResponse (D:\WorkSpace\Node.js\typescript\src\utils\BadRequestError.ts:21:19) at D:\WorkSpace\Node.js\typescript\src\app.ts:32:25 at Layer.handle [as handle_request] (D:\WorkSpace\Node.js\typescript\node_modules\express\lib\router\layer.js:95:5) at next (D:\WorkSpace\Node.js\typescript\node_modules\express\lib\router\index.js:280:10) at cors (D:\WorkSpace\Node.js\typescript\node_modules\cors\lib\index.js:188:7)"
}

{
  "statusCode": 500,
  "status": false,
  "message": "Internal server error occurred",
  "name": "HttpErrorResponse", 
  "stack": "HttpErrorResponse: Internal server error occurred at ErrorResponse (D:\WorkSpace\Node.js\typescript\src\utils\HttpError.ts:21:19) at D:\WorkSpace\Node.js\typescript\src\app.ts:32:25 at Layer.handle [as handle_request] (D:\WorkSpace\Node.js\typescript\node_modules\express\lib\router\layer.js:95:5) at next (D:\WorkSpace\Node.js\typescript\node_modules\express\lib\router\index.js:280:10) at cors (D:\WorkSpace\Node.js\typescript\node_modules\cors\lib\index.js:188:7)"
}

```

---

# 📖 Logger Utility

A **Winston-based logging utility** with colored console logs, file logging, and an Express middleware for request/response logging.
Supports **environment-based log files** (`development.server.log`, `development.error.log`, etc.) and **Indian Standard Time (IST)** timestamps.

---

## ✨ Features

* ✅ **Console + File Logging** (separate log files per environment)
* ✅ **Colored Console Output** (status codes, HTTP methods, device type, etc.)
* ✅ **Uncolored File Logs** (clean log files without escape codes)
* ✅ **Express Middleware** for automatic request/response logging
* ✅ **User-Agent Support** (platform, browser, device type)

---

## 📦 Installation

```bash
npm install express-useragent node-js-api-response
```

---

## 🚀 Usage

### 1. Import the logger

```ts
import express from 'express';
import useragent from 'express-useragent';
import { appLogger, errorLogger, requestResponseLogger } from './logger';
```

---

### 2. Add middleware

```ts
const app = express();

// Parse User-Agent (required for requestResponseLogger)
app.use(useragent.express());

// Attach request/response logger
app.use(requestResponseLogger);
```

---

### 3. Log messages manually
Instead of using raw console.log or console.error, use the provided loggers.
They automatically handle timestamps, colors, file storage, and environment-based separation.

✅ Use **appLogger.info** for normal events (e.g., server started, requests processed).

⚠️ Use **appLogger.warn** for warnings (e.g., low memory, deprecations).

❌ Use **errorLogger.error** for errors and exceptions.

👉 Tip:

`appLogger = use for general & debugging logs`

`errorLogger = use for errors & exceptions`
```ts
// Info log
appLogger.info("🚀 Server started successfully!");

// Warning log
appLogger.warn("⚠️ API rate limit almost reached!");

// Error log
try {
  throw new Error("Something went wrong");
} catch (err) {
  errorLogger.error((err as Error).message);
}

```
👉 This ensures logs are consistent, structured, and saved in the right place —
instead of getting lost in plain console.log output.

---

### 4. Example Express App

```ts
app.get('/api/v1/hello', (req, res) => {
  res.status(200).json({ message: "Hello World" });
});

app.get('/api/v1/error', (req, res) => {
  res.status(400).json({ error: "Bad Request" });
});

// Start server
app.listen(8080, () => {
  appLogger.info("🚀 Server is running on http://localhost:8080/api/v1");
});
```

---

## 📂 Log File Structure

Logs are stored in the `logs/` directory.
Each environment generates separate files:

```
logs/
 ├── development.server.log   # Info logs (requests, app messages)
 ├── development.error.log    # Error logs
 ├── production.server.log    # Generated in production mode
 ├── production.error.log
```

---

## 📊 Example Console Log

```bash
[19 Aug 2025, 10:30:20 pm] INFO: [14s-dq2606tu] [200] GET | /api/v1/ - 12ms | Microsoft Windows | Chrome/139.0.0.0 | Desktop
```

---

## ⚡ Environment Support

Set `NODE_ENV` to control log filenames:

```bash
set NODE_ENV=production node dist/index.js
```

---

✅ With this setup:

* Console logs = **pretty & colorful**
* File logs = **clean & structured**
* Middleware = **zero-config request/response logging**

---

## 🛠️ API Reference

### `SuccessResponse(res, statusCode, message?, data)`

* Sends a consistent success JSON structure

### `ErrorResponse(next, statusCode, message?, errorCode?)`

* Triggers an error that the global error handler can catch

### `HttpErrorResponse`

* Custom error class with `statusCode` and `status`

### `asyncHandler(fn)`

* Wraps async functions to handle errors automatically

### `globalErrorHandler`

* Global error middleware for Express

---
## License

This package is licensed under the MIT License.

---

This documentation provides a quick overview of the features, installation instructions, and usage examples for each utility. Let me know if you need further clarifications!
