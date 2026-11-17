# Monorepo Project Structure

## Turborepo Workspace Layout

The Campfire repository leverages **pnpm workspaces** and **Turborepo** to manage multiple interrelated applications and shared internal libraries within a single Git repository.

```
dev/ (Repository Root)
├── apps/                        # Deployable applications
│   ├── web/                     # Primary listener web platform (Next.js / React)
│   ├── artists/                 # Artist onboarding & release management dashboard
│   ├── admin/                   # Internal platform moderation & governance portal
│   ├── marketing/               # Public promotional website
│   ├── mobile/                  # React Native / Expo mobile app
│   └── desktop/                 # Electron / Tauri desktop wrapper
│
├── packages/                    # Shared internal packages
│   ├── ui/                      # Reusable UI component library (Tailwind + React)
│   ├── db/                      # Prisma ORM schema, client generation, and migrations
│   ├── auth/                    # Shared authentication configuration & session providers
│   ├── eslint-config/           # Centralized linting rules
│   └── typescript-config/       # Base tsconfig files extended by apps
│
├── prefect_flows/               # Python background jobs & data pipelines (Prefect)
├── scripts/                     # Local developer automation & staging verification scripts
├── .github/workflows/           # Automated CI/CD pipelines (test, lint, deploy)
├── package.json                 # Monorepo dependencies & root scripts
├── pnpm-workspace.yaml          # Workspace definitions
└── turbo.json                   # Turborepo task pipeline cache configuration
```

---

## Why a Monorepo?

### 1. Atomic Dependency Management
When a database model changes in `packages/db`, the Prisma client is generated across all apps simultaneously. TypeScript immediately surfaces type errors in any application consuming that model before code is merged.

### 2. High-Performance Build Caching with Turborepo
Turborepo creates a computation graph of tasks:
* If a pull request only touches files in `apps/web/src/components/Library/`, Turborepo knows that `apps/artists` has not changed and can reuse cached artifacts for testing and building.
* Developers can run `pnpm turbo run build` or `pnpm turbo run typecheck` across all apps concurrently with zero wasted rebuilds.

### 3. Shared Design System without npm Publishing Overhead
By keeping `@campfire/ui` inside the same repository, changes to UI components are instantly available in `apps/web` without the friction of versioning, publishing, and updating private npm packages.
