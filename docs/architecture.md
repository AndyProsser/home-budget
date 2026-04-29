# Application Architecture

## Overview

The codebase should keep domain logic separate from UI and infrastructure concerns.

Proposed top-level structure:

- src/domain: entities, calculators, planning logic
- src/application: use-cases and orchestration
- src/infrastructure: persistence adapters, API adapters, import/export
- src/ui: React components, pages, hooks, theme
- src/server: HTTP layer and route handlers

## Frontend

Stack:

- React with TypeScript
- Vite dev/build pipeline
- Radix UI primitives with custom theme layer

Responsibilities:

- Present normalized data in fn, month, annual views
- Drive pay-cycle workflow interactions
- Render charts for category split, debt progress, and budget flow
- Provide accessible, keyboard-friendly interactions

## Backend

Stack:

- Node.js + TypeScript
- REST API (Express or Fastify)

Responsibilities:

- Validate and persist domain objects
- Expose planning and projection endpoints
- Support transaction CSV import processing

Persistence strategy:

- Start with in-memory or file-based repository interfaces
- Keep repository contracts stable for later Postgres migration

## Key modules

- normalization service
- pay-cycle planner service
- debt projection service
- transfer recommendation service

Each module should:

- take typed inputs
- return typed outputs
- avoid framework-specific dependencies

## API direction

Recommended resource groups:

- households
- users
- accounts
- income-sources
- expenses
- debts
- pay-cycles
- pay-cycle-items
- imports

## Quality and tooling

- TypeScript strict mode
- ESLint for static quality checks
- Prettier for formatting
- Vitest for domain and integration test coverage

## Deployment

- Docker-based local and deployment target
- npm scripts should support dev, build, test, lint, and format
