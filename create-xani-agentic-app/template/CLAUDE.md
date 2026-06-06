# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Commands

Package manager is **pnpm** (Node 20, see `.nvmrc`).

| Task | Command |
|---|---|
| Dev server (Turbopack) | `pnpm dev` |
| Production build (runs migrations first) | `pnpm build` |
| CI build (no migrations) | `pnpm build:ci` |
| Lint | `pnpm lint` |
| Type check | `pnpm typecheck` |
| Lint + typecheck together | `pnpm check` |
| Format | `pnpm format` / `pnpm format:check` |
| Generate migration from schema | `pnpm db:generate` |
| Apply migrations | `pnpm db:migrate` |
| Drizzle Studio | `pnpm db:studio` |

There is **no test runner** configured. Per `AGENTS.md`, if a change needs testing and no tooling exists, ask the user whether to skip testing. Playwright MCP tools are available for browser-based verification.

After completing features, run `pnpm check` and `pnpm build:ci` to verify code quality.

### Database workflow (critical)

`pnpm build` runs `db:migrate` before `next build`. Migrations live in `drizzle/`. Per `AGENTS.md`: on any schema change, run `db:generate` then `db:migrate` — **never** `db:push` (the `db:push`/`db:dev`/`db:reset` scripts exist but must not be used). All non-BetterAuth ID columns must be randomly-generated UUIDs.

### Local setup

`cp env.example .env` → `docker compose up -d` (Postgres via `pgvector/pgvector:pg18` on port 5432) → `pnpm db:migrate` → `pnpm dev`. `pnpm setup` runs the guided `scripts/setup.ts`.

## Architecture

Next.js 16 App Router + React 19 + TypeScript, Tailwind v4, Better Auth, Drizzle ORM (postgres-js), AI SDK via OpenRouter. Path alias `@/*` → `src/*`.

### Auth (Better Auth)

- `src/lib/auth.ts` — server config. Email/password enabled by default; **no OAuth, no email provider**. Password-reset and email-verification URLs are logged to the terminal in dev (search the dev server output for them). Adding Google OAuth or a real email service is a deliberate follow-up task.
- `src/lib/auth-client.ts` — React client hooks (`useSession`, `signIn`, `signUp`, etc.).
- `src/app/api/auth/[...all]/route.ts` — catch-all handler via `toNextJsHandler`.
- `src/lib/schema.ts` — the four Better Auth tables (`user`, `session`, `account`, `verification`) use `text` IDs (Better Auth requirement). Any new app tables must use UUIDs instead.

### Route protection (two layers)

1. `src/proxy.ts` — Next.js 16 proxy doing **optimistic** cookie-existence redirects for `/dashboard`, `/chat`, `/profile`. Does not validate the session.
2. `src/lib/session.ts` — `requireAuth()` does real session validation in Server Components and redirects to `/` if unauthenticated. `protectedRoutes` here must stay in sync with the proxy `matcher` and `next.config` is unaware of them.

Protected pages call `requireAuth()`; the proxy is just a fast first gate.

### AI chat

`src/app/api/chat/route.ts` — authenticated POST endpoint. Validates the request with Zod (max 100 messages, 10k chars/part), then `streamText` through OpenRouter (`OPENROUTER_MODEL`, default `openai/gpt-5-mini`) returning a UI message stream. The `/chat` page uses `@ai-sdk/react`.

### Storage abstraction

`src/lib/storage.ts` — single `upload`/`deleteFile` API that switches on `BLOB_READ_WRITE_TOKEN`: Vercel Blob when set, otherwise local `public/uploads/`. Includes filename sanitization, size limits (5MB default), and extension allow-listing.

### Env validation

`src/lib/env.ts` — Zod schemas for server/client env plus `checkEnv()` for runtime warnings. `src/app/api/diagnostics/route.ts` is an intentionally public endpoint returning boolean config flags (no secrets) consumed by the homepage setup checklist.

## Conventions

- UI must follow `DESIGN.md` (shadcn/ui new-york, neutral base, oklch tokens, Tailwind v4 CSS-first config in `globals.css` — no `tailwind.config.ts`). Components in `src/components/ui/`.
- `AGENTS.md` mandates a plan→split→sub-agent→verify workflow: ask clarifying questions in planning, delegate implementation to sub-agents in parallel where useful, and act as coordinator.

## Docs & specs

- `docs/technical/` — reference notes (AI streaming/structured-data, Better Auth + Polar, react-markdown).
- `specs/{feature}/` — spec-driven workflow output (requirements, waved task files). Use the `create-spec` / `implement-feature` skills for large multi-session features.
- `create-xani-agentic-app/` — the CLI scaffolder that publishes this repo as a template; `pnpm sync-template` syncs template files.
