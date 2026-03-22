---
name: fastify-prisma
description: This skill should be used when implementing backend features with Fastify and Prisma, or when the /doctdd command dispatches the backend-expert agent during Phase 3. Provides project structure, patterns, and configuration knowledge.
disable-model-invocation: true
version: 1.0.0
---

**Fastify + Prisma** technical knowledge for backend implementation.

## Tech Stack

- Runtime: Node.js with TypeScript (strict mode)
- Framework: Fastify 5
- ORM: Prisma 7 with PostgreSQL adapter (`@prisma/adapter-pg`)
- Auth: `@fastify/jwt` + `@fastify/cookie` (httpOnly cookies)
- Password: `bcryptjs`
- Dev server: `tsx watch`

## Project Structure

```
packages/backend/
├── src/
│   ├── server.ts          # Fastify app initialization
│   ├── db.ts              # Prisma client
│   ├── routes/            # Route handlers
│   └── types/             # Type definitions
├── prisma/
│   └── schema.prisma      # Database schema
├── seed.sql               # Seed data
└── package.json
```

## Implementation Patterns

- Route handlers go in `src/routes/`, one file per domain
- Register routes in `src/server.ts`
- DB changes: update `prisma/schema.prisma` then run migration
- Test data: update `seed.sql`

## Code Style

- No comments (except TODO)
- Descriptive variable and method names
- TypeScript strict mode
- Follow existing patterns in the codebase

## Documentation Lookup

Before using Fastify or Prisma APIs, check Context7 for up-to-date documentation:
- `mcp__context7__resolve-library-id` to find Fastify / Prisma
- `mcp__context7__get-library-docs` to fetch API docs
