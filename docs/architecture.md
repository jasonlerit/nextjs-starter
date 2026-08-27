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
| Forms           | TanStack Form  | Form state management and validation                   |
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

### Placement Rules

- Treat route groups such as `(protected)` as organization only; their names do not enforce access control.
- Enforce authentication and authorization with server-side checks in the protected layout and at each sensitive data or mutation boundary.
- Shared response-parsing and API-error helpers may operate on a `Response`, but they must not issue requests or own endpoint paths; each domain service remains responsible for its request URL, query parameters, payload serialization, and protocol headers.
