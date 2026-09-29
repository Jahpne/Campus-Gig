# CampusGig (webapp)

CampusGig is a minimal single-page demo marketplace for short campus gigs (Hirer & Runner roles). It is built with React + TypeScript, bundled with Vite, and styled with Tailwind CSS. The app is intentionally simple and uses in-memory state for jobs so you can iterate quickly.

This README focuses on development and maintenance of the `web` package. This repo uses pnpm as the package manager — commands shown use `pnpm` (not `npm`).

## Prerequisites

- Node.js (recommended 18+)
- pnpm (v8+ recommended). Install with:

```
npm install -g pnpm
```

## Quick start

From the project root or inside the `web` folder:

Install dependencies:

```
pnpm install
```

Run the dev server (hot reload):

```
cd web
pnpm dev
```

Build for production (TypeScript project build + Vite build):

```
cd web
pnpm build
```

Preview the production build locally:

```
cd web
pnpm preview
```

Lint and format:

```
cd web
pnpm lint
pnpm format
```

> Note: All commands above intentionally use `pnpm` to install/run scripts.

# CampusGig (webapp)

CampusGig is a minimal single-page demo marketplace for short campus gigs (Hirer & Runner roles). It is built with React + TypeScript, bundled with Vite, and styled with Tailwind CSS. The app is intentionally simple and uses in-memory state for jobs to facilitate rapid iteration.

This README focuses on the development and maintenance of the `web` package. This repository uses **pnpm** as the package manager — all commands shown use `pnpm` (not `npm`).

## Prerequisites

- **Node.js**: Recommended version 18+
- **pnpm**: Recommended version 8+. Install globally if needed:

```bash
npm install -g pnpm
```

## Quick Start

From the project root or inside the `web` folder:

Install dependencies:

```bash
pnpm install
```

Run the dev server (with hot reload):

```bash
cd web
pnpm dev
```

Build for production (TypeScript project build + Vite bundle):

```bash
cd web
pnpm build
```

Preview the production build locally:

```bash
cd web
pnpm preview
```

Lint and format:

```bash
cd web
pnpm lint
pnpm format
```

Note: All commands above intentionally use `pnpm` to install and run scripts.

## Scripts (web/package.json)

- `pnpm dev` — Runs the Vite development server (HMR).
- `pnpm build` — Runs `tsc -b` (type-check) followed by `vite build` (production bundle).
- `pnpm preview` — Serves the production build locally.
- `pnpm lint` / `pnpm lint:fix` — Runs ESLint checks.
- `pnpm format` / `pnpm format:check` — Runs Prettier code formatting.

## Project Layout

- `web/src/App.tsx` — Main application file containing the sample UI, page components (Landing, Register, Runner, Hirer), and UI primitives (BrandMark, DummyButton).
- `web/src/main.tsx` — React application entry point and DOM mount.
- `web/src/index.css` — Global styles (Tailwind CSS entry point).
- `web/package.json` — Package configuration and scripts for the web workspace.
- `tsconfig.app.json`, `tsconfig.node.json` — TypeScript configurations used by `tsc -b`.

## Architecture & Suggested Refactors

The codebase currently keeps application logic within a single file (`App.tsx`) to maintain a self-contained demo. As features expand, implementing one of the following architectural refactors is recommended:

### Option 1: Modularize Components

- What: Extract logical sections into `src/components/` and `src/pages/` (e.g., `LandingPage.tsx`, `HirerPage.tsx`, `RunnerPage.tsx`, `RegisterPage.tsx`, `ui/DummyButton.tsx`, `ui/BrandMark.tsx`).
- Why: Improves navigation, maintainability, testability, and reduces Git diff conflicts.
- Implementation notes: Move JSX and local state into individual files, share types via a dedicated `types.ts` file or re-export from `App.tsx`, and verify type safety using `pnpm build`.

### Option 2: Add localStorage Persistence

- What: Persist `jobs` state to browser `localStorage` so data survives page reloads.
- Why: Enhances the offline demo experience without requiring a backend.
- Implementation notes: Implement a custom `useLocalStorage` hook or use `useEffect` hooks in `App` to synchronize the `jobs` state.

### Option 3: Scaffold a Mock API

- What: Implement a lightweight mock API layer (using `msw` or a local dev server) to handle fetch/create job operations over HTTP.
- Why: Prepares the frontend for real backend integration and simplifies testing network states and error handling.
- Implementation notes: Add a `server/` or `mock/` folder with standard REST endpoints (`GET /jobs`, `POST /jobs`, `PATCH /jobs/:id`).

## Developer Notes

- **Type Safety**: Always run `pnpm build` to catch TypeScript errors early (`tsc -b` runs automatically prior to the Vite build).
- **Styling**: Tailwind utility classes are used throughout. Ensure your editor has the Prettier Tailwind plugin configured and run `pnpm format` before committing.
- **Testing**: There are currently no unit tests. When splitting components, consider adding React Testing Library tests for key user workflows.

## Collaboration Guidelines

When collaborating with other developers, share this README and ensure all team members use `pnpm`. Keep pull requests small and focused when refactoring component structures.

## License

No license file is currently included. Add a `LICENSE` file if you plan to distribute this package.

---

File pointers: start with `web/src/App.tsx`, `web/src/main.tsx`, and `web/package.json`.
pnpm lint
pnpm format

```

Scripts (what they do)

- `pnpm dev` — run Vite dev server (HMR)
- `pnpm build` — `tsc -b` (project build) then `vite build` (production bundle)
- `pnpm preview` — serve the production build locally
- `pnpm lint` / `pnpm lint:fix` — ESLint checks
- `pnpm format` / `pnpm format:check` — Prettier

Project layout (high level)

- `web/src/App.tsx` — main app file. Contains the sample UI and all page components (Landing, Register, Runner, Hirer) plus small UI primitives (`BrandMark`, `DummyButton`).
- `web/src/main.tsx` — React bootstrap / mount point.
- `web/src/index.css` — global styles (Tailwind entry).
- `web/package.json` — package and scripts for the `web` package.
- `tsconfig.app.json`, `tsconfig.node.json` — TypeScript configurations used by `tsc -b`.

Why `App.tsx` is a single file today

- The current code keeps everything in one file for quick iteration and to keep the demo self-contained. As features grow, splitting components improves readability, testability, and reusability.

Suggested refactors (choose one to implement next)

1. Split `src/App.tsx` into smaller components

- What: extract logical pieces into `src/components/` and `src/pages/` (e.g., `LandingPage.tsx`, `HirerPage.tsx`, `RunnerPage.tsx`, `RegisterPage.tsx`, `ui/DummyButton.tsx`, `ui/BrandMark.tsx`).
- Why: easier navigation, smaller files, simpler unit tests, better source control diffs.
- Work involved: create new files, move JSX and relevant local state into the new components, keep shared types in a `types.ts` or export from `App.tsx`, update imports/exports, run `pnpm build` to verify type safety.
- Risk: minimal — mostly mechanical code moves. Watch for closures over local variables when extracting functions.

2. Add localStorage persistence

- What: persist `jobs` state to `localStorage` so data survives reloads.
- Why: useful for a demo without a backend; quick UX improvement.
- Work involved: create a small `useLocalStorage` hook or add `useEffect` in `App` to sync `jobs` to `localStorage` and read on init.
- Pros: quick to implement, no server required.
- Cons: not multi-user; won't persist across browsers/devices.

3. Scaffold a small mock API (local dev server)

- What: add a lightweight mock JSON API (e.g., using `msw` or a simple Express/Koa dev server) and change fetch/create job operations to hit the mock server.
- Why: allows development of fetch flows, integrates nicely into future real backend work, and lets you demo network error handling.
- Work involved: add a `server/` or `mock/` folder, implement REST endpoints (`GET /jobs`, `POST /jobs`, `PATCH /jobs/:id`), switch `App` to load initial state from the API and POST changes instead of directly calling `setJobs` (you can still update local state optimistically).
- Pros: closer to real-world workflow; good for testing network behavior.
- Cons: more setup and slightly higher maintenance than localStorage.

Recommended next steps (practical)

- If you want the fastest improvement: implement localStorage persistence (option 2). I can add a `useLocalStorage` hook and wire it in minutes.
- If you want maintainability and tests: split `src/App.tsx` into components (option 1). I will also add index exports and update imports.
- If you plan to integrate a backend soon: scaffold the mock API (option 3).

Developer notes

- Type safety: run `pnpm build` to catch TypeScript errors early (`tsc -b` runs before Vite build).
- Styling: Tailwind utilities are used throughout; run `pnpm format` if Prettier + Tailwind plugin is configured in your editor.
- Tests: there are no tests currently. When extracting components, consider adding React Testing Library unit tests for key behaviors.

How I can help now

- I can implement any one of the above options. Tell me which one and I will:
  - create the required files and move code (option 1), or
  - add `localStorage` persistence and a small `useLocalStorage` hook (option 2), or
  - scaffold a mock API and update data flows to use it (option 3).

Contact / collaboration

- If you're collaborating with others, share this README and the `pnpm` commands. Keep PRs small when refactoring `src/App.tsx` so reviews are easy.

License

- No license file included. Add `LICENSE` if you plan to distribute.

---

File pointers: start with `web/src/App.tsx`, `web/src/main.tsx`, and `web/package.json`.
```
