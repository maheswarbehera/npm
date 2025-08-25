
# 🚀 ns-init — Node Server Initializer CLI


*A simple CLI tool to bootstrap **Node.js server projects** with support for **TypeScript** or **JavaScript** templates.  
Includes optional **logger** integration with [`node-js-api-response`](https://www.npmjs.com/package/node-js-api-response).*

[![npm version](https://img.shields.io/npm/v/ns-init?color=blue&logo=npm)](https://www.npmjs.com/package/ns-init)
[![npm downloads](https://img.shields.io/npm/dm/ns-init.svg?color=green&logo=npm)](https://www.npmjs.com/package/ns-init)
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

You can also check out our **Node.js Centralised API Response and Error Handling** utility:  

[![node-js-api-response](https://img.shields.io/npm/v/node-js-api-response?label=node-js-api-response&color=purple&logo=npm)](https://www.npmjs.com/package/node-js-api-response)
[![npm downloads](https://img.shields.io/npm/dm/node-js-api-response.svg?color=green&logo=npm)](https://www.npmjs.com/package/node-js-api-response)

*A lightweight package to standardize API success/error responses and logging in Express.js apps.*


---

## 📦 Installation

```bash
npm install -g ns-init@latest
````

or run directly with **npx** (no install required):

```bash
npx ns-init my-app 
```

---

## ⚡ Usage

```bash
ns-init [project-name] [options]
```

If no options are passed, the CLI runs in interactive mode (project name, template, logger setup).

---

## 🛠 Options

| Option       | Description                                        |
| ------------ | -------------------------------------------------- |
| `--ts`       | Use **TypeScript** template                        |
| `--js`       | Use **JavaScript** template                        |
| `--log`      | Include `node-js-api-response` logger + middleware |setup        |
| `-h, --help` | Show help                                |
---

## 📋 Examples


### 1. Interactive (recommended)

```bash
npx ns-init
```

### 2. TypeScript 

```bash
npx ns-init my-app --ts
npx ns-init my-app --ts --log   # with logger
```

### 3. JavaScript 

```bash
npx ns-init my-app --js
npx ns-init my-app --js --log   # with logger
```

---

## 📂 Project Structure

Example output for **TypeScript** template:

```
my-app/
├── src/
│   ├── app.ts
│   └── index.ts
├── package.json
└── tsconfig.json
```

For **JavaScript** template:

```
my-app/
├── src/
│   ├── app.js
│   └── index.js
├── package.json
```

---
## 📸 Screenshots / Demo

### 🚀 Interactive CLI Prompt

When you run `ns-init` without arguments, you’ll be guided interactively:

```bash
npx ns-init
```
![CLI Init Prompt](https://raw.githubusercontent.com/maheswarbehera/ns-init/master/assets/screenshots/cli-init.png)

### ⚡ Direct Command with Options

You can also skip prompts and pass flags directly:

```bash
npx ns-init my-app --ts --log
```

![CLI Init Prompt](https://raw.githubusercontent.com/maheswarbehera/ns-init/master/assets/screenshots/cli-init-option.png)


---

### 📦 Generated Project Structure

After scaffolding, you’ll get a clean project:

![Generated Project](https://raw.githubusercontent.com/maheswarbehera/ns-init/master/assets/screenshots/project-structure.png)

---
## 📝 Logger Integration
If you enable logging with the **`--log`** option, the generated project will include a request/response logger powered by **node-js-api-response**.

Depending on whether you choose **`TypeScript` or `JavaScript`**, the boilerplate will auto-inject the following:

```ts
import useragent from "express-useragent";
import { requestResponseLogger, appLogger } from "node-js-api-response";

const app: Application = express(); 

app.use(useragent.express());
app.use(requestResponseLogger);

appLogger.info(`✅ Server running on http://localhost:${PORT}`);
```


---

## 🤝 Contributing

Pull requests and feature requests are welcome!
Feel free to open an issue for bugs or suggestions.

---

## 📜 License

This package is licensed under the MIT License.

This documentation provides a quick overview of the features, installation instructions, and usage examples for each utility. Let me know if you need further clarifications!
