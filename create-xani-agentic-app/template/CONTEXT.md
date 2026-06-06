# Xani Agentic Starter Kit

A production-oriented starter kit for building AI-powered web apps with an agentic development workflow. This document is the canonical glossary for the project — especially its three easily-confused names.

## Language

**Xani Agentic Starter Kit**:
The human-readable brand / display name of the project, shown in the README title, site footer, setup script, and skill description.
_Avoid_: Agentic Coding Starter Kit, Xani Agentic App.

**xani-agentic-starter-kit**:
The repository and the root `package.json` name — i.e. the kit itself (the Next.js app, schema, skills, docs). GitHub slug: `AlexandreXavier/xani-agentic-starter-kit`.
_Avoid_: agentic-coding-starter-kit.

**create-xani-agentic-app**:
The npm package and `npx` command that scaffolds a new copy of the kit (`npx create-xani-agentic-app my-app`). Lives in the repo folder of the same name and follows npm's `create-*` convention (package name == bin name). Distinct from the kit it publishes.
_Avoid_: create-agentic-app, create-app, create-xani-app.

## Relationships

- The **create-xani-agentic-app** CLI scaffolds an instance of the **xani-agentic-starter-kit**.
- Both live in the **xani-agentic-starter-kit** repo (`AlexandreXavier/xani-agentic-starter-kit`); the CLI is published from the `create-xani-agentic-app/` subfolder.
- **Xani Agentic Starter Kit** is the display name of the **xani-agentic-starter-kit**; it is never the name of the CLI.
- The skill installs with `npx skills add AlexandreXavier/xani-agentic-starter-kit@create-xani-agentic-app` — slug is the kit, the suffix is the CLI.

## Flagged ambiguities

- "create-app" was proposed as the command name — rejected: too generic, unpublishable on npm, and loses the "xani" identity. Canonical command is **create-xani-agentic-app**.
- The repo name (the kit) and the npm package name (the CLI) are deliberately different. They are not interchangeable.
