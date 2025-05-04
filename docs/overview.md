<!-- docs/overview.md -->
# Project Overview & Best Practices

This document outlines the directory structure, conventions, and guidelines for adding new pages, components, and features to keep the codebase clean, scalable, and maintainable.

## 1. Project Structure
```text
├── **public/**
│   └── Static assets (images, icons, manifest, etc.)
├── **src/**
│   ├── **client.tsx**    — Client entry point (hydrate with StartClient)
│   ├── **ssr.tsx**       — Server entry point (StartHandler)
│   ├── **router.tsx**    — TanStack Router + React-Query integration
│   ├── **routeTree.gen.ts** — Generated route manifest
│   ├── **api.ts**        — API handler setup (createStartAPIHandler)
│   ├── **routes/**       — File-based routes & API endpoints
│   ├── **components/**   — Reusable UI components (presentational)
│   ├── **integrations/** — Providers/wrappers (TanStack Query, AI SDK, etc.)
│   ├── **store/**        — Global state slices (TanStack Store)
│   ├── **lib/**          — Shared logic, store initializers
│   ├── **utils/**        — Helper functions (AI demos, SSE, tools)
│   ├── **data/**         — Static data files (JSON, TS)
│   └── **styles.css**    — Global styles (Tailwind imports)
├── **docs/**             — Markdown documentation (this folder)
├── package.json, tsconfig.json, etc.
```

## 2. Naming & Conventions
- **File names**: kebab-case (`user-profile.tsx`), except React components (`MyButton.tsx`).
- **Dynamic routes**: folder with `$param.tsx` (e.g. `src/routes/users/$userId.tsx`).
- **Root layout**: `src/routes/__root.tsx` exports the global `<Outlet />` and layout.
- **API routes**: prefix `api.` (e.g. `api.messages.ts`) under `src/routes/`.
- **Components**: PascalCase and grouped by domain if large (e.g. `components/Chat/MessageItem.tsx`).

## 3. Adding New Pages (Routes)
1. In `src/routes/`, create a new file or folder:
   - Simple page: `about.tsx` → `/about`
   - Nested/dynamic: `products/$id.tsx` → `/products/:id`
2. Export a React component; optionally export `loader` or `action`:
   ```ts
   export const loader = async () => fetch('/api/data').then(r => r.json())
   export default function MyPage() { /* use useLoaderData() */ }
   ```
3. Use `Link` from `@tanstack/react-router` for SPA navigation.
4. Data fetched in loaders is available via `useLoaderData()`.

## 4. Building UI Components
- **Presentational vs Container**: keep visual-only components in `components/`, containers or page-specific logic in `routes/` or a feature folder.
- **Styling**: use Tailwind utility classes; for complex variants, use [class-variance-authority (CVA)](https://github.com/joe-bell/cva).
- **Class merging**: use `clsx` or `tailwind-merge` to combine conditional classes.
- **Accessibility**: ensure semantic HTML, ARIA attributes where needed.

## 5. State Management
- Use **TanStack Store** for global or shared state (`src/store/`).
- Slice logic by feature and expose hooks or store instances.
- In components, consume state via `useStore()`.

## 6. Data Fetching & Caching
- **Route loaders**: for SSR data, define `loader` in route files.
- **React Query**: for client-side fetching and mutations, use `useQuery` / `useMutation` via the provided QueryClient.

## 7. API Endpoints
- File-based API routes in `src/routes/api.*.ts`.
- Use `createAPIFileRoute()` to define handlers (GET, POST, etc.).

## 8. Testing
- Co-locate tests next to code (`Component.test.tsx`) or in `__tests__/`.
- Use **Vitest** and `@testing-library/react`/`dom`.

## 9. Linting & Formatting
- ESLint (with `@tanstack/eslint-config`) + Prettier.
- Run `pnpm lint` / `pnpm format` / `pnpm check` before commits.

## 10. Git & CI
- Follow conventional commit messages (feat, fix, docs, chore).
- Create feature branches, open PRs for review.

---
_By adhering to these guidelines, we ensure consistency, scalability, and maintainability as the project grows._
