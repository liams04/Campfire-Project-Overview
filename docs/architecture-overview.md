# High-Level Application Architecture

## Ecosystem Overview

Campfire is architected as a **Turborepo monorepo** separating different customer touchpoints (listeners, artists, platform administrators) while sharing core UI design systems, database schemas, and authentication logic.

```mermaid
flowchart TD
    subgraph Clients["Applications (apps/)"]
        WebApp["Web App (apps/web)<br/>Listener streaming platform, library, playlists"]
        ArtistPortal["Artist Portal (apps/artists)<br/>Release management, payout analytics"]
        AdminPortal["Admin Portal (apps/admin)<br/>Platform governance & verification"]
        MarketingSite["Marketing Site (apps/marketing)<br/>Landing pages & mission info"]
    end

    subgraph SharedPackages["Shared Packages (packages/)"]
        UI["@campfire/ui<br/>Shared component library & design tokens"]
        DB["@campfire/db<br/>Prisma schema, client & migrations"]
        Auth["@campfire/auth<br/>NextAuth & WebAuthn session config"]
        Config["@campfire/config<br/>ESLint, Prettier, Tailwind configurations"]
    end

    subgraph DataServices["Backend & Data Services"]
        Postgres[("PostgreSQL Database<br/>Prisma ORM")]
        Redis[("Redis Cache / Upstash<br/>Rate limiting & session caching")]
        AudioStorage[("Audio & Asset CDN<br/>High-fidelity music streaming")]
        PrefectEngine["Python / Prefect Flows<br/>Background data sync & royalties processing"]
    end

    %% Dependencies
    WebApp --> UI & DB & Auth
    ArtistPortal --> UI & DB & Auth
    AdminPortal --> UI & DB & Auth
    MarketingSite --> UI

    DB --> Postgres
    WebApp --> Redis
    WebApp --> AudioStorage
    PrefectEngine --> Postgres
```

---

## Architectural Highlights

### 1. Unified Design System (`packages/ui`)
* Component primitives (buttons, modals, input elements, dropdowns, and card layouts) are maintained inside a shared package.
* This ensures that visual brand standards, accessibility patterns, and Tailwind styles remain consistent across both the listener app (`apps/web`) and the creator dashboard (`apps/artists`).

### 2. Isolated Application Domains
* **Listener Web App (`apps/web`)**: Focused on fast streaming audio delivery, catalog discovery, responsive search, personal library management, and smooth playback interactions.
* **Artist Portal (`apps/artists`)**: Tailored for content creators to upload music tracks, manage cover art and album metadata, track stream counts, and monitor payout distributions.
* **Admin Portal (`apps/admin`)**: Internal tool used by foundation staff to verify artist accounts, moderate content, and oversee nonprofit operations.

### 3. Background Data Automation (Python & Prefect)
* While the client applications and primary APIs are built with TypeScript and Next.js, asynchronous tasks (such as periodic ingestion, analytics processing, and metadata synchronization) are orchestrated via **Python Prefect flows** running in the background.
