<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

# Project Guidelines

## General

- Follow existing project conventions before introducing a new pattern.
- Prefer the smallest focused change that satisfies the task.
- Reuse existing utilities, components, schemas, clients, and abstractions before creating new ones.
- Do not refactor unrelated code.
- Do not add a dependency when the project already provides an equivalent solution.
- Prefer the `@/*` alias for imports from `src`.
- Never commit unless explicitly requested.

## Architecture

Project architecture, stack, folder structure, and placement conventions are documented in [`docs/architecture.md`](docs/architecture.md).

Read `docs/architecture.md` before:

- Creating, moving, or reorganizing files or folders.
- Adding a new feature or module.
- Deciding where new code should live.
- Adding routes, components, hooks, services, repositories, schemas, or database code.
- Introducing a new dependency or technology.

For small changes within an existing module, follow the surrounding structure and conventions.

## Next.js and React

- Use Server Components by default. Add `"use client"` only when browser APIs, client-side state, or event handlers require it.
- Keep secrets, database access, and other server-only code out of Client Components.
- Add `import 'server-only'` to modules that access secrets, the database, or other server-only resources.
- Use framework APIs and conventions documented in the installed Next.js guides.

## Data Fetching

- Use Server Components for initial server-side data fetching when client-side caching, refetching, or synchronization is unnecessary.
- Use TanStack Query for client-side data fetching, caching, synchronization, and mutation state.
- Use the shared `getQueryClient` from `src/lib/react-query.ts`; do not create ad hoc `QueryClient` instances.
- Use API routes and domain-specific modules under `src/services/` for client-side requests; service functions may use `fetch` directly or an existing configured HTTP client such as Axios.
- Do not create generic HTTP wrappers such as `services/api.ts`, `apiFetch`, `http.ts`, or `request.ts`.
- Components and TanStack Query hooks must delegate HTTP requests to domain service functions instead of calling `fetch` or an HTTP client directly.
- Domain service functions must have typed parameters and typed return values. Reuse existing domain types where available; runtime schema parsing is not required by this rule.
- Define API request and response DTOs shared by server and client under domain-specific files in `src/common/types/`; both API producers and client service consumers must use these canonical contracts instead of declaring duplicate transport types.
- Keep types used by only one implementation colocated with that implementation; do not move every local helper or internal type into `src/common/types/`.
- Shared response-parsing and API-error helpers may parse a `Response` or construct an error, but they must not issue requests, own endpoint paths, or become generic request wrappers. Domain services remain responsible for `fetch`, URLs, query parameters, payload serialization, and protocol headers.
- Prefer TanStack Query mutations for client-driven mutations that need loading, error, cache invalidation, or optimistic-update behavior.
- Use Server Actions only when they clearly simplify a server-only or form-driven workflow and do not bypass established API/service conventions without a reason.
- Invalidate or update affected queries after successful TanStack Query mutations.

## UI and Styling

- Use shadcn/ui components from `src/components/ui` and do not overwrite customized components without explicit authorization.
- Keep application-specific UI out of `components/ui`; colocate it with its route or place reusable components in `components/shared`.
- Use `cn` from `src/lib/utils.ts` for conditional Tailwind classes.
- Prefer semantic theme utilities over hard-coded colors.
- Use Lucide icons through `lucide-react`.
- Follow Base UI APIs; do not assume Radix UI APIs.

## HTTP Status

- Use `HttpStatusCodes` and `HttpStatusPhrases` from `@/common/constants/http-status.constant`.
- Do not hardcode HTTP status codes or standard HTTP status phrases.
- Add a missing status to `http-status.constant.ts` when needed.

## Environment Variables

- Define and validate environment variables in `src/common/env.ts` with Zod.
- Import the validated `env` object; do not access named `process.env` variables elsewhere.
- Add required variables to `.env.example` with safe placeholder values.
- Group `.env.example` variables by concern. When adding variables for a new service or feature, create a separate commented section. Use exactly one blank line between sections, no blank line between a section comment and its variables, and no consecutive blank lines.
- Never expose server-only values with `NEXT_PUBLIC_`.
- You may read `.env` to confirm which variables exist, but never echo or log secret values, and never copy real values into code, tests, docs, or commit messages. Treat `.env.example` as the canonical reference for names and placeholders.
- Do not write to `.env` or change existing values without explicit authorization.

## Database

- Use Drizzle ORM with PostgreSQL.
- Keep schemas in `src/db/schemas`, export them from `src/db/schemas/index.ts`, and keep migrations in `src/db/migrations`.
- Treat everything in `src/db/migrations`, including SQL migrations and `meta/` files, as generated artifacts. Do not create, edit, rename, or delete these files manually; make changes through the Drizzle schemas and `pnpm run db:generate`.
- Use camelCase in TypeScript and snake_case in PostgreSQL.
- Run `pnpm run db:generate` after schema changes.
- Never run `db:migrate` or `db:push` without explicit authorization.

## TypeScript and Validation

- Keep TypeScript strict and prefer inferred types when clear.
- Validate untrusted input and external data with Zod at system boundaries.
- Prefer named exports for shared modules.
- Avoid `any`, unsafe casts, and non-null assertions unless necessary and justified.
- Do not log secrets or sensitive values.

## Formatting

- Follow the repository Prettier configuration.
- Use single quotes, omit semicolons, and keep trailing commas where supported.
- Let `prettier-plugin-tailwindcss` order Tailwind classes.
- Run `pnpm run format` after editing supported files.

## Spec Workflow

- When asked to create or update docs/specs, edit documentation only. Do not implement or commit.
- Treat issue or adjustment details as requirements to review, not automatic authorization to implement.
- Before changing code, read `docs/progress.md` and relevant specs. If documented behavior must change, update the spec first and wait for explicit implementation approval.
- Providing requested details alone does not authorize implementation.
- Implement a spec only when explicitly requested.
- Authorization applies only to the specific task identified when it is given and expires when that task is completed.
- Resolve “next spec” or “next task” once; prior approval does not apply to whichever item becomes next afterward.
- Questions such as “what’s next?” request information only. Report the next item and wait for explicit implementation approval.
- `docs/progress.md` records status and recommendations; it never authorizes implementation.
- When implementing an approved spec, follow it and update `docs/progress.md`.
- Preserve the canonical section order in `docs/progress.md`: `Status`, `Phase`, `Current`, `Last completed`, `Next`, `Blockers`, and `Notes`.
- Each canonical section must appear exactly once. Update existing sections in place, and consolidate any duplicates instead of appending another section.
- Keep only active work in progress under `## Current`; move or remove items when they are completed or no longer in progress.
- Keep `## Notes` as the final section. Use it only for important handoff context that does not fit another section; write `None.` when there is none.
- After editing `docs/progress.md`, check for missing, duplicated, reordered, or unknown sections.
- Before planning, resuming, implementing, or reporting progress, read `docs/progress.md` and inspect the working tree for existing changes.
- Treat `docs/progress.md` as a concise handoff index. Follow any referenced specifications or files for full details instead of duplicating them there.

## Commit Messages

- Use Conventional Commits: `<type>(optional-scope): <description>`.
- Allowed types: `feat`, `fix`, `chore`, `refactor`, `docs`, `test`, `style`, `perf`, `ci`, `build`, `revert`.
- Use lowercase imperative descriptions without a trailing period.
- Mark breaking changes with `!` and a `BREAKING CHANGE:` footer when needed.
- Review the staged diff before creating a commit.
- Never push, force-push, rebase, amend, or create/delete branches or stashes without explicit request.

Example: `feat(auth): add password reset flow`

## Testing

- Use Vitest as the test runner.
- Colocate tests as `*.test.ts` next to the files they cover.
- Do not install or configure any other test framework without authorization.

## Verification

- Run `pnpm run check` after code changes.
- Run relevant tests when they exist or when a change adds them.
- Run `pnpm run db:generate` after database schema changes and review the generated SQL.
- Before committing, ensure `pnpm run check` passes.
- Report checks that could not be run or failures unrelated to the change.
