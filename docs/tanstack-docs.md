 # TanStack Project Documentation

 Welcome to our TanStack-based application! This guide will help new contributors understand the tech stack,
 project structure, and how to add new pages seamlessly using TanStack Router (file-based routing).

 ## Table of Contents
 1. [Tech Stack Overview](#tech-stack-overview)
 2. [Getting Started](#getting-started)
 3. [Project Structure](#project-structure)
 4. [Routing & Pages](#routing--pages)
    - [File-Based Routing](#file-based-routing)
    - [Adding a New Page](#adding-a-new-page)
    - [Dynamic & Nested Routes](#dynamic--nested-routes)
    - [Linking & Navigation](#linking--navigation)
 5. [Layouts & Outlets](#layouts--outlets)
 6. [Data Loading](#data-loading)
 7. [React Query Integration](#react-query-integration)
 8. [State Management](#state-management)
 9. [Scripts & Commands](#scripts--commands)

 ## Tech Stack Overview
 - **React** (v19)
 - **Vite** (bundler & dev server)
 - **TanStack Start** (`@tanstack/react-start`)
 - **TanStack Router** (`@tanstack/react-router`, file-based plugin)
 - **TanStack Query** (`@tanstack/react-query`, caching & data fetching)
 - **TanStack Store** (`@tanstack/store`, global state)
 - **Tailwind CSS** (utility-first styling)
 - **Shadcn UI** (optional component library)
 - **Vinxi** (dev/start/build wrapper)
 - **Vitest** (unit & integration tests)

 ## Getting Started
 1. Install dependencies:
    ```bash
    pnpm install
    ```
 2. Start dev server with HMR:
    ```bash
    pnpm dev  # or `vinxi dev`
    ```
 3. Build for production:
    ```bash
    pnpm build
    ```
 4. Preview the production build:
    ```bash
    pnpm serve  # uses `vite preview`
    ```

 ## Project Structure
 ```text
 ├─ app.config.ts            # TanStack Start & Vite config
 ├─ package.json
 ├─ docs/                    # <-- Documentation folder
 │  └─ tanstack-docs.md
 ├─ src/
 │  ├─ client.tsx            # Hydrate client entry
 │  ├─ ssr.tsx               # Server-side rendering entry
 │  ├─ router.tsx            # Creates router with React Query context
 │  ├─ routeTree.gen.ts      # Auto-generated file-based route manifest
 │  ├─ routes/               # File-based routes directory
 │  │  ├─ __root.tsx         # Root layout (HTML shell + DevTools)
 │  │  ├─ index.tsx          # `/` route
 │  │  ├─ example.chat.tsx   # `/example/chat` route
 │  │  └─ ...                # Add your own `.tsx` files here
 │  └─ components/           # Shared UI components (e.g. Header)
 └─ public/                  # Static assets (images, icons)
 ```

 ## Routing & Pages

 ### File-Based Routing
 TanStack Start leverages file-based routing: every `.tsx` file under `src/routes/` (and its subfolders)
 is treated as a page or API endpoint. Creating, renaming, or deleting files automatically regenerates
 `src/routeTree.gen.ts`, and your app reloads with the updated routes.

 #### Pages Directory & Naming
- Place all page components in `src/routes/`.
- File and folder names map directly to URL segments:
  - `src/routes/about.tsx` → `/about`
  - `src/routes/blog/$postId.tsx` → `/blog/:postId`
  - `src/routes/dashboard/settings.tsx` → `/dashboard/settings`
- No extra router configuration is needed beyond file placement.

 ### Adding a New Page
 1. In `src/routes/`, add a new `.tsx` file (or a folder with `index.tsx`).
    Example:
    ```bash
    # Top-level page
    touch src/routes/about.tsx

    # Nested page (dashboard)
    mkdir -p src/routes/dashboard && touch src/routes/dashboard/index.tsx
    ```
 2. Inside the file, export your page component (and optional route helpers):
    ```tsx
    // src/routes/about.tsx
    export default function AboutPage() {
      return (
        <div className="p-8">
          <h1>About Us</h1>
          <p>…</p>
        </div>
      )
    }

    // Optional: server-side data loader
    export const loader = async () => {
      const res = await fetch('/api/about-data')
      return res.json()
    }
    ```
 3. (Optional) Export an `action` function for form submissions or mutations:
    ```ts
    export const action = async ({ request }) => { /* handle POST */ }
    ```
 4. Save your file—TanStack Start will regenerate the router manifest and your new page becomes available.

 ### Dynamic & Nested Routes
 - **Dynamic segments**: Use `$param` in filenames:
   ```text
   src/routes/blog/$postId.tsx   # → /blog/:postId
   ```
 - **Nested routes**: Create folders:
   ```text
   src/routes/dashboard/
   ├─ index.tsx    # /dashboard
   └─ settings.tsx # /dashboard/settings
   ```

 ### Linking & Navigation
 Use the `Link` component from TanStack Router:
 ```tsx
 import { Link } from '@tanstack/react-router'

 function Nav() {
   return <Link to="/about">About</Link>
 }
 ```

 ## Layouts & Outlets
 - The root layout is `src/routes/__root.tsx`.
 - It uses `<Outlet />` to render child pages and includes global UI like `<Header />`.

 ## Data Loading
 You can use built-in **loaders** on routes:
 ```tsx
 export const Route = createFileRoute('/items')({
   loader: async () => fetch('/api/items').then(r => r.json()),
   component: () => {
     const items = Route.useLoaderData()
     return <List data={items} />
   },
 })
 ```

 ## React Query Integration
 - The router is wrapped with `routerWithQueryClient` in `src/router.tsx`.
 - The Query Client is provided via context.
 - Use `useQuery` in your components as usual.

 ## State Management
 - **TanStack Store** is used for global state.
 - Define stores in `src/store/` and consume via `useStore`.

 ## Scripts & Commands
 - `pnpm dev` / `vinxi dev`: start with HMR
 - `pnpm build` / `vinxi build`: production bundle
 - `pnpm serve` / `vite preview`: preview prod build
 - `pnpm test`: run `vitest`
 - `pnpm lint` / `pnpm format` / `pnpm check`: lint & format

 ---
 **Happy hacking!** 🎉