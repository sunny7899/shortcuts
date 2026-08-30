# Important NPM Scripts for Development

A comprehensive cheatsheet of essential `package.json` scripts and useful CLI commands for modern JavaScript/TypeScript, Angular, React, Next.js, and Node.js projects.

---

## 1. Development & Local Server

Scripts for starting local development servers, watch mode, and LAN/mobile testing.

```json
"scripts": {
  "start": "node index.js",
  "dev": "vite",
  "dev:host": "vite --host",
  "dev:poll": "next dev --turbo",
  "start:dev": "nodemon --watch src -e js,ts,json --exec ts-node src/index.ts",
  "start:opened": "npm run build && ng serve --disable-host-check --host 0.0.0.0 --open",
  "watch": "tsc -w"
}
```

> **LAN / Network Testing Tip:**
> - Run `ipconfig` (Windows) or `ifconfig` / `ip a` (Linux/Mac) to find your local IPv4 address.
> - Using `--host 0.0.0.0` or `--host <your-ip>` allows access from mobile devices or other computers on the same network.

---

## 2. Build & Bundling

Scripts for compiling, optimizing, and creating production bundles.

```json
"scripts": {
  "build": "tsc && vite build",
  "build:prod": "cross-env NODE_ENV=production vite build",
  "build:stats": "vite build --mode analyze",
  "analyze": "source-map-explorer 'dist/**/*.js'",
  "preview": "vite preview"
}
```

---

## 3. Linting, Formatting & Type Checking

Ensure code consistency, catch syntax errors, and validate TypeScript types without generating output.

```json
"scripts": {
  "lint": "eslint . --ext .js,.jsx,.ts,.tsx",
  "lint:fix": "eslint . --ext .js,.jsx,.ts,.tsx --fix",
  "format": "prettier --write \"src/**/*.{js,jsx,ts,tsx,css,md,json}\"",
  "format:check": "prettier --check \"src/**/*.{js,jsx,ts,tsx,css,md,json}\"",
  "typecheck": "tsc --noEmit",
  "typecheck:watch": "tsc --noEmit --watch",
  "check-all": "npm run typecheck && npm run lint && npm run format:check"
}
```

---

## 4. Testing & Code Coverage

Unit tests, integration tests, end-to-end tests, and test coverage reports.

```json
"scripts": {
  "test": "jest",
  "test:watch": "jest --watch",
  "test:coverage": "jest --coverage",
  "test:ci": "jest --ci --runInBand --coverage",
  "test:e2e": "playwright test",
  "test:e2e:ui": "playwright test --ui"
}
```

---

## 5. Cleaning & Fresh Setup

Clean up temporary build artifacts, caches, and perform fresh dependency installations.

```json
"scripts": {
  "clean": "rimraf dist build .next out coverage",
  "clean:all": "rimraf dist build node_modules package-lock.json && npm install",
  "fresh": "npm run clean && npm install"
}
```

> **Windows PowerShell Equivalent for fast cleaning:**
> ```powershell
> Remove-Item -Recurse -Force node_modules, dist, package-lock.json
> npm install
> ```

---

## 6. Concurrent & Multi-Task Execution

Running multiple services simultaneously (e.g., backend API + frontend + mock server).

*Requires packages:* `concurrently` or `npm-run-all`

```json
"scripts": {
  "dev:all": "concurrently \"npm run dev:api\" \"npm run dev:frontend\"",
  "dev:mock": "concurrently -k \"json-server --watch db.json --port 5000\" \"npm start\"",
  "build:all": "npm-run-all clean --parallel build:frontend build:backend"
}
```

---

## 7. Lifecycle Hooks (Pre & Post)

NPM automatically runs `pre<script>` before `<script>`, and `post<script>` after `<script>`.

```json
"scripts": {
  "prebuild": "npm run clean && npm run typecheck",
  "build": "vite build",
  "postbuild": "echo \"Build completed successfully!\"",
  
  "pretest": "npm run lint",
  "test": "jest",
  
  "prepare": "husky install"
}
```

---

## 8. Database & Environment Utilities

Common scripts for managing ORMs (Prisma, TypeORM, Drizzle) and environment files.

```json
"scripts": {
  "db:generate": "prisma generate",
  "db:push": "prisma db push",
  "db:migrate": "prisma migrate dev",
  "db:seed": "ts-node prisma/seed.ts",
  "db:studio": "prisma studio",
  "env:copy": "copy .env.example .env"
}
```

---

## 9. Essential NPM CLI Diagnostics & Commands

Quick CLI commands to debug dependency trees, vulnerability checks, and scripts.

| Command | Purpose |
| :--- | :--- |
| `npm run` | List all available scripts declared in `package.json` |
| `npm run <script> -- --flag` | Pass arguments/flags to the underlying script |
| `npm ls` | Display full dependency tree |
| `npm ls <package>` | Show which packages require a specific dependency |
| `npm explain <package>` (or `npm why <package>`) | Detailed explanation of why a dependency is installed |
| `npm outdated` | Check for outdated packages against registry versions |
| `npm audit` | Check dependencies for known security vulnerabilities |
| `npm audit fix --force` | Attempt automated security vulnerability fixes |
| `npm cache clean --force` | Force clean npm global cache |
| `npx envinfo --system --binaries --browsers` | Output system environment info for bug reporting |