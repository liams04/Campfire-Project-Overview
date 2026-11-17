# Campfire Music Foundation — Volunteer Software Developer Overview

> **Portfolio Note**: This repository is a high-level technical overview of my volunteer software engineering experience with **Campfire Music Foundation**. To respect the organization's proprietary codebase and intellectual property, **no source code is published in this repository**. Instead, this document outlines the product mission, system architecture, development workflows, and the specific frontend features and bug fixes I delivered.

---

## Organization & Mission

[**Campfire Music Foundation**](https://www.cfmusic.org/) is a nonprofit startup dedicated to creating a fair-compensation streaming and discovery ecosystem for independent musicians. 

Traditional music streaming platforms pay fractions of a cent per stream, heavily favoring mega-label artists while leaving grassroots creators struggling to sustain their careers. Campfire is built on an artist-first financial model, combining high-fidelity streaming, direct listener support, and transparent analytics so creators keep a meaningful share of their revenue.

---

## Tech Stack Overview

The platform is architected as a modern TypeScript/JavaScript monorepo managed with **Turborepo** and **pnpm**:

* **Frontend**: React, Next.js, TypeScript, Tailwind CSS
* **Monorepo Architecture**: Turborepo workspace separating user-facing applications (`apps/web`, `apps/artists`, `apps/admin`) from shared libraries (`packages/ui`, `packages/db`, `packages/config`)
* **State & Data Fetching**: React hooks, custom contexts, Next.js Server Components, SWR / React Query
* **Data Layer & Backend**: PostgreSQL, Prisma ORM, Redis (Upstash)
* **Data Processing & Pipelines**: Python (Prefect flows for background data syncing and analytics)
* **DevOps & Quality**: GitHub Actions (CI/CD), Husky pre-commit hooks, ESLint, Prettier, Sentry error telemetry

---

## My Role: Intern Software Developer (Volunteer)

I volunteered as an **Intern Software Developer**, contributing directly to the core web listener application and artist portal in an active startup environment.

### 1. Key Features Delivered

* **Library Section Overhaul**:
  * Rebuilt the listener **Library** section from the ground up in the main web application (`apps/web`).
  * Structured intuitive tabbed views for saved playlists, liked tracks, followed artists, and saved albums.
  * Ensured responsive grid/list view rendering and integrated fluid transitions when users added or removed media from their personal collections.

* **Audio Playhead & Playback Controls**:
  * Fixed an audio player timeline bug where the playhead scrubber experienced visual offset drifts during scrubbing and window resizes.
  * Implemented dedicated play controls for the **Liked Songs** collection, including shuffle and repeat toggles.

* **Mobile Viewport Gate**:
  * Built a responsive viewport gate for `apps/web` and `apps/artists` that detects unsupported mobile screen dimensions and surfaces a clean, branded holding banner guiding users to desktop browsers or upcoming native apps.

* **UI Polish & Defect Resolution**:
  * Resolved visual rendering and text contrast bugs on the **Artist Release** configuration pages.
  * Updated and debugged the user **Feedback Modal**, ensuring submissions validated input cleanly before sending payload data.

### 2. Development Workflow & Team Collaboration

* **Feature Branching & Code Reviews**: Worked across GitHub pull requests, adhering to the team's PR guidelines, receiving feedback from senior engineers, and reviewing peer branches.
* **Monorepo Task Coordination**: Utilized Turborepo caching to run local builds, typechecks, and tests (`pnpm turbo run dev`, `pnpm turbo run typecheck`) across multi-package dependencies.
* **Task Management**: Collaborated with product and design leads using modern issue tracking (Linear / GitHub Issues) to clarify acceptance criteria and verify staging deployments.

---

## Repository Contents

* **[High-Level Application Architecture](docs/architecture-overview.md)**: Visual diagram and explanation of the multi-app monorepo ecosystem.
* **[Monorepo Project Structure](docs/project-structure.md)**: Breakdown of how `apps/` and `packages/` are organized and shared.
* **[Development & QA Workflow](docs/development-workflow.md)**: How our team coordinated feature development, Git branching, and automated staging checks.
* **[Screenshots & Permissions Guide](docs/screenshots/README.md)**: Guidance regarding UI imagery and confidentiality boundaries.

---

## Key Takeaways from the Experience

1. **Thriving in an Existing Codebase**: Unlike class projects started from scratch, working at Campfire required diving into a large, existing TypeScript monorepo, understanding established design tokens, and adhering to strict linting and styling patterns.
2. **Component Reusability**: Leveraging shared UI component packages (`packages/ui`) reinforced the importance of building stateless, modular frontend elements that render consistently across different applications (e.g., sharing cards between the listener app and the artist portal).
3. **Startup Agility**: Fast-paced iteration taught me how to communicate blockers quickly, test edge cases across varied viewports, and deliver dependable frontend pull requests.
