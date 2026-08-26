<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

# Project Guidelines

## Approval and Commit Workflow

- Never create a commit without explicit, scope-limited authorization.
- A commit request authorizes only the files and checkpoint explicitly identified by the user.
- After documentation review, "commit & proceed" means commit the reviewed documentation/specification changes only, then proceed with implementation while leaving implementation changes uncommitted.
- Never commit implementation code, tests, configuration, migrations, or generated files without a separate explicit request to commit those implementation changes.
- After implementation, run the required checks, report the implementation diff and verification results, then stop and wait for explicit commit approval.
- Do not include unrelated or unapproved changes in a commit, even if they are staged.

## Architecture

Project architecture, stack, folder structure, and placement conventions are documented in [`docs/architecture.md`](docs/architecture.md).

Read `docs/architecture.md` before creating or reorganizing files, adding a feature or module, or deciding where new code should live.

## Environment Variables

- Group `.env.example` variables by concern. When adding variables for a new service or feature, create a separate commented section. Use exactly one blank line between sections, no blank line between a section comment and its variables, and no consecutive blank lines.
- You may read `.env` to confirm which variables exist, but never echo or log secret values, and never copy real values into code, tests, docs, or commit messages. Treat `.env.example` as the canonical reference for names and placeholders.
- Do not write to `.env` or change existing values without explicit authorization.

## Database

- Never run `db:migrate` or `db:push` without explicit authorization.

## Spec Workflow

- When asked to create or update docs/specs, edit documentation only. Do not implement or commit.
- Treat issue or adjustment details as requirements to review, not automatic authorization to implement.
- Before changing code, read `docs/progress.md` and relevant specs. If documented behavior must change, update the spec first and wait for explicit implementation approval.
- Providing requested details alone does not authorize implementation.
- Implement a spec only when explicitly requested; implementation approval does not authorize committing the implementation.
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
