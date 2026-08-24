# Architecture

## Installed Stack

| Category        | Technology     | Purpose                                                |
| --------------- | -------------- | ------------------------------------------------------ |
| Language        | TypeScript     | Primary language                                       |
| Framework       | Next.js        | React framework and application foundation             |
| Styling         | Tailwind CSS   | Utility-first CSS framework                            |
| UI              | shadcn/ui      | Reusable UI components                                 |
| Icons           | Lucide React   | Icon library                                           |
| Data Fetching   | TanStack Query | Data fetching, caching, synchronization, and mutations |
| Validation      | Zod            | Schema validation and type inference                   |
| ORM             | Drizzle ORM    | Type-safe database access and queries                  |
| Database        | PostgreSQL     | Primary relational database                            |
| Package Manager | pnpm           | Dependency and package management                      |
| Linting         | ESLint         | Static analysis and code quality                       |
| Formatting      | Prettier       | Code formatting                                        |

## Optional Technologies

These technologies are approved choices when a feature requires them, but they are not installed or configured in the starter by default.

| Category           | Technology       | Purpose                                   |
| ------------------ | ---------------- | ----------------------------------------- |
| Forms              | TanStack Form    | Form state management and validation      |
| Tables             | TanStack Table   | Headless tables and data grids            |
| Virtualization     | TanStack Virtual | Virtualized lists and tables              |
| State Management   | Zustand          | Lightweight application state management  |
| HTTP Client        | Axios            | HTTP requests and API communication       |
| Authentication     | Better Auth      | Authentication and session management     |
| Schema Integration | Drizzle Zod      | Generate Zod schemas from Drizzle schemas |
| Cache / Storage    | Redis            | Caching and temporary/shared data         |

Install and configure an optional technology only when required by the task. Before adding a dependency, check whether the project already provides an equivalent solution.

## Folder Structure

```text
src/
├── app/
│   ├── (server)/
│   │   ├── api/
│   │   │   └── v1/
│   │   │       ├── users/
│   │   │       │   └── route.ts
│   │   │       └── me/
│   │   │           └── route.ts
│   │   │
│   │   ├── repositories/
│   │   │   └── user.repository.ts
│   │   │
│   │   └── services/
│   │       └── user.service.ts
│   │
│   └── (web)/
│       ├── (auth)/
│       │   └── login/
│       │       ├── _components/
│       │       │   └── login-form.tsx
│       │       └── page.tsx
│       │
│       ├── (protected)/
│       │   ├── layout.tsx
│       │   └── (dashboard)/
│       └── (public)/
│
├── common/
│   ├── constants/
│   │   ├── app.constant.ts
│   │   ├── pagination.constant.ts
│   │   ├── user.constant.ts
│   │   └── http-status.constant.ts
│   │
│   ├── schemas/
│   │   ├── auth.schema.ts
│   │   ├── pagination.schema.ts
│   │   ├── user.schema.ts
│   │   └── product.schema.ts
│   │
│   ├── types/
│   │   ├── user.type.ts
│   │   └── role.type.ts
│   │
│   └── utils/
│       ├── color.util.ts
│       ├── date.util.ts
│       ├── string.util.ts
│       └── user.util.ts
│
├── components/
│   ├── shared/
│   │   ├── data-table/
│   │   │   ├── data-table.tsx
│   │   │   ├── data-table-column-header.tsx
│   │   │   ├── data-table-faceted-filter.tsx
│   │   │   ├── data-table-pagination.tsx
│   │   │   └── data-table-view-options.tsx
│   │   │
│   │   ├── filters/
│   │   ├── form/
│   │   ├── input/
│   │   ├── navbar/
│   │   ├── sidebar/
│   │   ├── mode-toggle.tsx
│   │   ├── react-query-client-provider.tsx
│   │   └── theme-provider.tsx
│   │
│   └── ui/
│
├── db/
│   ├── migrations/
│   │
│   ├── schemas/
│   │   ├── index.ts
│   │   ├── users.schema.ts
│   │   └── products.schema.ts
│   │
│   └── index.ts
│
├── hooks/
│   ├── queries/
│   │   └── use-user.query.ts
│   │
│   ├── stores/
│   │   └── use-user.store.ts
│   │
│   └── use-mobile.ts
│
├── lib/
│   ├── auth.ts
│   ├── auth-client.ts
│   ├── axios.ts
│   ├── react-query.ts
│   ├── resend.ts
│   └── s3.ts
│
└── services/
    ├── auth.service.ts
    └── user/
        └── me.service.ts
```

### Directory Conventions

| Directory                    | Purpose                                                |
| ---------------------------- | ------------------------------------------------------ |
| `app/(server)/api/v1/`       | Versioned Next.js API route handlers                   |
| `app/(server)/repositories/` | Database access and persistence logic                  |
| `app/(server)/services/`     | Server-side business and application logic             |
| `app/(web)/`                 | User-facing Next.js routes                             |
| `app/(web)/.../_components/` | Components specific to the nearest page or route       |
| `common/constants/`          | Shared application and domain constants                |
| `common/schemas/`            | Shared Zod schemas                                     |
| `common/types/`              | Shared TypeScript types                                |
| `common/utils/`              | Shared utility functions                               |
| `components/shared/`         | Components reused across pages or features             |
| `components/ui/`             | shadcn/ui components                                   |
| `db/`                        | Database and Drizzle ORM setup                         |
| `db/schemas/`                | Drizzle database schema definitions                    |
| `db/migrations/`             | Generated Drizzle migrations                           |
| `hooks/queries/`             | Reusable TanStack Query hooks                          |
| `hooks/stores/`              | Reusable Zustand stores                                |
| `hooks/`                     | General reusable React hooks                           |
| `lib/`                       | Configured clients, SDKs, and third-party integrations |
| `services/`                  | Domain-specific client-side HTTP/API requests          |

### Placement Rules

- Use `app/(server)/` for server-side API routes, repositories, and business logic.
- Use `app/(web)/` for user-facing routes.
- Treat route groups such as `(protected)` as organization only; their names do not enforce access control.
- Enforce authentication and authorization with server-side checks in the protected layout and at each sensitive data or mutation boundary.
- Keep protected route groups nested under `(protected)` so they share its layout checks.
- Add `import 'server-only'` to repositories, services, and other modules that access the database, secrets, or server-only resources.
- Keep page-specific components inside the nearest `_components/` directory.
- Move components to `components/shared/` when they are reused across pages or features.
- Keep shadcn/ui components in `components/ui/`.
- Keep shared constants, schemas, types, and utilities under `common/`.
- Keep API request and response DTOs shared by server and client in domain-specific files under `common/types/`; API producers and client service consumers must use these canonical transport contracts rather than duplicate them.
- Keep implementation-only types colocated with their owning module instead of promoting every local type into `common/types/`.
- Keep Drizzle schemas in `db/schemas/`.
- Re-export Drizzle schemas through `db/schemas/index.ts`.
- Keep generated Drizzle migrations in `db/migrations/`.
- Keep the configured database client in `db/index.ts`.
- Keep reusable TanStack Query hooks in `hooks/queries/`.
- Keep reusable Zustand stores in `hooks/stores/`.
- Keep general reusable hooks directly under `hooks/`.
- Keep configured clients, SDKs, and third-party integrations in `lib/`.
- Keep client-side HTTP requests in domain-specific modules under the root `services/` directory, such as `auth.service.ts` or `user/me.service.ts`; do not add generic request modules.
- Shared response-parsing and API-error helpers may operate on a `Response`, but they must not issue requests or own endpoint paths; each domain service remains responsible for its request URL, query parameters, payload serialization, and protocol headers.
- Keep shared Axios configuration in `lib/axios.ts` only when Axios is installed and shared configuration is needed.
- Keep server-side business logic in `app/(server)/services/`.
- Keep database access logic in `app/(server)/repositories/`.
- Prefer colocating page-specific code with its page before promoting it to a shared directory.
- Follow the existing project structure before introducing a new top-level directory.
