# 📘 Package.json Scripts - Complete Guide

A comprehensive guide to writing npm scripts in `package.json` with real-world examples and practice questions.

---

## 📑 Table of Contents
1. [Basic Concepts](#basic-concepts)
2. [Common Script Commands](#common-script-commands)
3. [File Operations](#file-operations)
4. [Cross-Platform Scripts](#cross-platform-scripts)
5. [Lifecycle Scripts](#lifecycle-scripts)
6. [Real-World Examples](#real-world-examples)
7. [Practice Questions](#practice-questions)

---

## 🎯 Basic Concepts

### What are npm scripts?
Scripts in `package.json` are commands that can be run using `npm run <script-name>`.

```json
{
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  }
}
```

Run with: `npm run start` or `npm start` (for special scripts)

### Special Scripts (Don't need "run")
- `npm start` → runs "start" script
- `npm test` → runs "test" script
- `npm install` → runs "install" script

---

## 🛠️ Common Script Commands

### 1. **Running Node Applications**

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "serve": "node dist/server.js"
  }
}
```

**Use Case:** Starting your application in different modes

---

### 2. **Building TypeScript**

```json
{
  "scripts": {
    "build": "tsc",
    "build:watch": "tsc -w",
    "build:prod": "tsc --sourceMap false"
  }
}
```

**Use Case:** Compiling TypeScript to JavaScript

---

### 3. **Cleaning Directories**

**Windows:**
```json
{
  "scripts": {
    "clean": "rmdir /s /q dist",
    "clean:safe": "if exist dist rmdir /s /q dist"
  }
}
```

**Cross-Platform (using rimraf):**
```json
{
  "scripts": {
    "clean": "rimraf dist"
  }
}
```

**Use Case:** Removing build folders before rebuilding

---

## 📁 File Operations

### 1. **Copying Files (Windows)**

**Single File:**
```json
{
  "scripts": {
    "copy:config": "copy .env.example .env",
    "copy:json": "copy src\\config.json dist\\config.json"
  }
}
```

**Multiple Files:**
```json
{
  "scripts": {
    "copy:assets": "copy src\\assets\\* dist\\assets\\",
    "copy:all": "xcopy /E /I /Y src\\assets dist\\assets"
  }
}
```

**Explanation:**
- `copy` - Basic copy command
- `xcopy /E /I /Y` - Copy directories recursively
  - `/E` - Copy subdirectories (including empty)
  - `/I` - Assume destination is directory
  - `/Y` - Overwrite without prompting

---

### 2. **Copying Files (Cross-Platform)**

**Using copyfiles package:**
```bash
npm install --save-dev copyfiles
```

```json
{
  "scripts": {
    "copy:single": "copyfiles src/config.json dist",
    "copy:pattern": "copyfiles -u 1 \"src/**/*.json\" dist",
    "copy:flat": "copyfiles -f src/assets/* dist/assets"
  }
}
```

**Explanation:**
- `-u 1` - Strip first directory level (src/)
- `-f` - Flatten structure (no subdirectories)
- `**/*.json` - All JSON files recursively

---

### 3. **Creating Directories**

**Windows:**
```json
{
  "scripts": {
    "mkdir": "if not exist dist mkdir dist",
    "mkdir:nested": "mkdir dist\\src\\functions"
  }
}
```

**Cross-Platform (using mkdirp):**
```json
{
  "scripts": {
    "mkdir": "mkdirp dist/src/functions"
  }
}
```

---

### 4. **Moving Files**

**Windows:**
```json
{
  "scripts": {
    "move:file": "move src\\temp.json dist\\temp.json",
    "move:overwrite": "move /Y src\\*.log logs\\"
  }
}
```

---

## 🌍 Cross-Platform Scripts

### PowerShell (Works on Windows)

```json
{
  "scripts": {
    "copy:ps": "powershell -Command \"Copy-Item src/config.json dist/\"",
    "clean:ps": "powershell -Command \"Remove-Item dist -Recurse -Force\"",
    "mkdir:ps": "powershell -Command \"New-Item -ItemType Directory -Force -Path dist/src\""
  }
}
```

**Advantages:** More reliable on Windows, better error handling

---

### Node.js Scripts (100% Cross-Platform)

```json
{
  "scripts": {
    "copy:node": "node -e \"require('fs').copyFileSync('src/file.json', 'dist/file.json')\"",
    "mkdir:node": "node -e \"require('fs').mkdirSync('dist', {recursive:true})\""
  }
}
```

---

### Using cross-env for Environment Variables

```bash
npm install --save-dev cross-env
```

```json
{
  "scripts": {
    "start:dev": "cross-env NODE_ENV=development node index.js",
    "start:prod": "cross-env NODE_ENV=production node index.js"
  }
}
```

---

## 🔄 Lifecycle Scripts

### Pre and Post Hooks

npm automatically runs `pre` and `post` scripts:

```json
{
  "scripts": {
    "prestart": "npm run build",
    "start": "node dist/index.js",
    "poststart": "echo 'Server started successfully'",
    
    "prebuild": "npm run clean",
    "build": "tsc",
    "postbuild": "npm run copy:assets"
  }
}
```

**Order of execution for `npm start`:**
1. `prestart` → runs first
2. `start` → runs second
3. `poststart` → runs last

---

### Common Lifecycle Patterns

```json
{
  "scripts": {
    "clean": "rimraf dist",
    "build": "tsc",
    "postbuild": "copyfiles -u 1 src/**/*.json dist",
    "prestart": "npm run clean && npm run build",
    "start": "node dist/index.js"
  }
}
```

---

## 💼 Real-World Examples

### Example 1: TypeScript + Azure Functions Project

```json
{
  "scripts": {
    "clean": "rimraf dist",
    "build": "tsc",
    "postbuild": "copyfiles -u 1 \"src/**/*.json\" dist && copyfiles firebase-service-account.json dist",
    "watch": "tsc -w",
    "prestart": "npm run clean && npm run build",
    "start": "func start",
    "test": "jest",
    "lint": "eslint src/**/*.ts",
    "format": "prettier --write \"src/**/*.ts\""
  }
}
```

---

### Example 2: React Application

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "prebuild": "npm run lint",
    "postbuild": "echo 'Build completed!'",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write \"src/**/*.{js,jsx,ts,tsx,json,css,md}\""
  }
}
```

---

### Example 3: Express API with Database

```json
{
  "scripts": {
    "start": "node dist/server.js",
    "dev": "nodemon --exec ts-node src/server.ts",
    "build": "tsc",
    "prebuild": "rimraf dist",
    "postbuild": "copyfiles -u 1 src/**/*.sql dist",
    "test": "jest --coverage",
    "test:watch": "jest --watch",
    "migrate": "node dist/database/migrate.js",
    "seed": "node dist/database/seed.js",
    "db:setup": "npm run migrate && npm run seed"
  }
}
```

---

### Example 4: Monorepo/Multi-Package

```json
{
  "scripts": {
    "build": "npm run build:common && npm run build:api && npm run build:web",
    "build:common": "cd packages/common && npm run build",
    "build:api": "cd packages/api && npm run build",
    "build:web": "cd packages/web && npm run build",
    "test": "npm run test:common && npm run test:api && npm run test:web",
    "test:common": "cd packages/common && npm test",
    "test:api": "cd packages/api && npm test",
    "test:web": "cd packages/web && npm test"
  }
}
```

---

## 🎓 Practice Questions

### **Beginner Level**

#### Question 1: Basic Start Script
**Task:** Create a script named "start" that runs `node app.js`

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "start": "node app.js"
  }
}
```
</details>

---

#### Question 2: Copy Single File
**Task:** Copy `config.json` from `src/` to `dist/` folder (Windows)

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "copy": "copy src\\config.json dist\\config.json"
  }
}
```
</details>

---

#### Question 3: Clean Directory
**Task:** Delete the `dist` folder using rimraf

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "clean": "rimraf dist"
  }
}
```

Note: Requires `npm install --save-dev rimraf`
</details>

---

### **Intermediate Level**

#### Question 4: Build and Copy
**Task:** Create a "build" script that:
1. Compiles TypeScript (`tsc`)
2. Then copies all `.json` files from `src/functions/` to `dist/functions/`

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "build": "tsc",
    "postbuild": "copyfiles -u 1 \"src/functions/**/*.json\" dist/functions"
  }
}
```

Or using Windows commands:
```json
{
  "scripts": {
    "build": "tsc && xcopy /E /I /Y src\\functions\\*.json dist\\functions\\"
  }
}
```
</details>

---

#### Question 5: Pre-build Cleanup
**Task:** Before building, automatically clean the `dist` folder

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "prebuild": "rimraf dist",
    "build": "tsc"
  }
}
```

When you run `npm run build`, it automatically runs `prebuild` first!
</details>

---

#### Question 6: Multiple File Copy
**Task:** Copy these files to `dist/`:
- `package.json`
- `host.json`
- `local.settings.json`

<details>
<summary>Click to see answer</summary>

**Windows:**
```json
{
  "scripts": {
    "copy:config": "copy package.json dist\\ && copy host.json dist\\ && copy local.settings.json dist\\"
  }
}
```

**Cross-platform:**
```json
{
  "scripts": {
    "copy:config": "copyfiles package.json host.json local.settings.json dist"
  }
}
```
</details>

---

### **Advanced Level**

#### Question 7: Complete Build Pipeline
**Task:** Create a complete build process:
1. Clean `dist` folder
2. Build TypeScript
3. Copy all JSON files from `src/` to `dist/`
4. Copy `firebase-service-account.json` to `dist/`

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "clean": "rimraf dist",
    "build": "tsc",
    "postbuild": "copyfiles -u 1 \"src/**/*.json\" dist && copyfiles firebase-service-account.json dist",
    "prebuild": "npm run clean"
  }
}
```

Run with: `npm run build`

Execution order:
1. `prebuild` (clean)
2. `build` (tsc)
3. `postbuild` (copy files)
</details>

---

#### Question 8: Conditional Directory Creation
**Task:** Create `dist/src/functions/notification` folder only if it doesn't exist, then copy `function.json` (Windows)

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "copy:function": "if not exist dist\\src\\functions\\notification mkdir dist\\src\\functions\\notification && copy src\\functions\\notification\\function.json dist\\src\\functions\\notification\\"
  }
}
```
</details>

---

#### Question 9: Development Workflow
**Task:** Create scripts for development:
- `dev` - Watch TypeScript and restart on changes
- `dev:debug` - Same as dev but with debugging enabled

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "dev": "nodemon --exec ts-node src/index.ts",
    "dev:debug": "nodemon --exec node --inspect -r ts-node/register src/index.ts"
  }
}
```

Requires: `npm install --save-dev nodemon ts-node`
</details>

---

#### Question 10: Multi-Environment Setup
**Task:** Create scripts to run your app in different environments with proper environment variables

<details>
<summary>Click to see answer</summary>

```json
{
  "scripts": {
    "start:dev": "cross-env NODE_ENV=development PORT=3000 node dist/index.js",
    "start:staging": "cross-env NODE_ENV=staging PORT=4000 node dist/index.js",
    "start:prod": "cross-env NODE_ENV=production PORT=8080 node dist/index.js"
  }
}
```

Requires: `npm install --save-dev cross-env`
</details>

---

## 🔧 Useful Packages for Scripts

### Essential Packages

```bash
# Clean directories
npm install --save-dev rimraf

# Copy files cross-platform
npm install --save-dev copyfiles

# Create directories
npm install --save-dev mkdirp

# Cross-platform environment variables
npm install --save-dev cross-env

# Run multiple scripts in parallel
npm install --save-dev npm-run-all

# Watch for file changes
npm install --save-dev nodemon
```

---

## 🚀 Advanced Patterns

### Running Scripts in Parallel

```json
{
  "scripts": {
    "watch:ts": "tsc -w",
    "watch:func": "func start",
    "dev": "npm-run-all --parallel watch:*"
  }
}
```

---

### Running Scripts in Sequence

```json
{
  "scripts": {
    "build:clean": "rimraf dist",
    "build:compile": "tsc",
    "build:copy": "copyfiles -u 1 src/**/*.json dist",
    "build": "npm-run-all build:clean build:compile build:copy"
  }
}
```

---

### Passing Arguments to Scripts

```json
{
  "scripts": {
    "test": "jest",
    "test:file": "jest"
  }
}
```

Run with: `npm run test:file -- src/utils.test.ts`

The `--` passes arguments to the script

---

## 📚 Cheat Sheet

| Task | Windows Command | Cross-Platform Package |
|------|----------------|----------------------|
| Copy file | `copy src\\file.txt dist\\` | `copyfiles src/file.txt dist` |
| Copy folder | `xcopy /E /I /Y src\\folder dist\\folder` | `copyfiles -u 1 "src/folder/**/*" dist` |
| Delete folder | `rmdir /s /q dist` | `rimraf dist` |
| Create folder | `mkdir dist` | `mkdirp dist` |
| Move file | `move src\\file.txt dist\\` | Use Node.js script |
| Check if exists | `if exist dist echo "exists"` | Use Node.js script |

---

## 💡 Best Practices

1. **Use cross-platform packages** for portability (rimraf, copyfiles, cross-env)
2. **Keep scripts simple** - If too complex, create a separate `.js` file
3. **Use lifecycle hooks** (pre/post) for automatic execution
4. **Name scripts clearly** - Use `:` for namespacing (e.g., `build:dev`, `build:prod`)
5. **Document complex scripts** in README.md
6. **Don't hardcode paths** - Use relative paths
7. **Use `&&` to chain commands** that must run in sequence
8. **Use `npm-run-all`** for complex parallel/sequential execution

---

## 🎯 Quick Reference

```json
{
  "scripts": {
    // Basic
    "start": "node index.js",
    "build": "tsc",
    
    // With hooks
    "prebuild": "npm run clean",
    "postbuild": "npm run copy",
    
    // Chaining
    "deploy": "npm run build && npm run test && npm run upload",
    
    // Cross-platform
    "clean": "rimraf dist",
    "copy": "copyfiles -u 1 src/**/*.json dist",
    
    // Environment
    "start:dev": "cross-env NODE_ENV=dev node index.js",
    
    // Parallel
    "dev": "npm-run-all --parallel watch:*"
  }
}
```

---

**Happy Scripting! 🚀**

Remember: When in doubt, check if there's an npm package that does it cross-platform!