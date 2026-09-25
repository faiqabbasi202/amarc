# System Architecture & Technical Specifications

> **Project:** AMARC Engineering & Construction Portal (`amarc.com.pk`)  
> **Engineered by:** Faiq Abbasi  
> **Architecture Classification:** Enterprise Web Portal & Bespoke 100+ KB CMS Engine

---

## 1. System Overview

**AMARC Engineering & Construction** is an enterprise-tier web platform engineered for one of Pakistan's leading multi-disciplinary construction conglomerates. The system pairs an ultra-responsive, motion-enhanced client portal with an autonomous 100+ KB administrative CMS, enabling non-technical operators to control all site content, multi-billion PKR project portfolios, real estate developments, career listings, and client inquiries in real time.

```mermaid
flowchart TB
    subgraph ClientLayer["🖥️ Client Presentation Layer (SSR / CSR)"]
        direction TB
        A[Visitor / Enterprise Client] -->|Browse Portfolio & Services| R1[TanStack Router Engine]
        Admin[Corporate Executive] -->|Secure Auth Guard| R2[Admin Command Center /admin]
        R1 --> V1[Cinematic Hero & Parallax]
        R1 --> V2[9 Disciplines & Sector Matrix]
        R1 --> V3[Recently Delivered Portfolio]
        R1 --> V4[Real Estate Developments]
        R1 --> V5[Client Intake & Quotation Engine]
        R2 --> AD1[Portfolio & Sector Controller]
        R2 --> AD2[Company & Team Directory]
        R2 --> AD3[Careers & Inquiries Pipeline]
    end

    subgraph LogicLayer["⚡ Application & Data Layer"]
        direction TB
        Q[TanStack Query v5 Cache]
        VLD[Input Sanitizer & Zod Validation Engine]
        STR[Resilient Local Store Fallback]
        INV[Cache Invalidation Bus]
        
        R1 --> Q
        R2 --> VLD
        VLD --> Q
        Q <--> INV
        Q <--> STR
    end

    subgraph BackendLayer["☁️ Cloud & Infrastructure Layer (Supabase)"]
        direction TB
        SB_AUTH[Supabase Auth & Session Guard]
        SB_DB[(PostgreSQL Relational DB)]
        SB_RLS[Row Level Security Policies]
        SB_CDN[Storage Buckets & Media Delivery]
        
        Q <--> SB_DB
        R2 --> SB_AUTH
        SB_DB --- SB_RLS
        V1 & V2 <--> SB_CDN
    end
```

---

## 2. Technology Stack & Rationale

| Layer | Technology | Decision Rationale |
| :--- | :--- | :--- |
| **Framework** | **React 19 & TypeScript** | Modern concurrent rendering, strict type safety, zero compile-time compromises. |
| **Routing** | **TanStack Router / Start** | Fully type-safe routing, search params validation, loader-driven suspense data pre-fetching. |
| **Styling** | **Tailwind CSS v4** | Next-generation performance, CSS variables-first architecture, responsive utility design. |
| **Motion** | **Motion (Framer Motion v13)** | Fluid hardware-accelerated parallax, reveal animations, spring transitions without frame drops. |
| **UI Components** | **Radix UI Primitives** | Unstyled, accessible (WAI-ARIA compliant) headless primitives customized for quiet luxury aesthetic. |
| **State & Cache** | **TanStack Query v5** | Declarative asynchronous caching, automatic background refetching, optimistic UI updates. |
| **Backend & DB** | **Supabase (PostgreSQL 15+)** | Relational data integrity, Row Level Security (RLS), instant API generation, asset buckets. |
| **Validation** | **Custom Input Sanitizer & Zod** | Deep sanitization of rich text and form inputs to eliminate XSS and injection vulnerabilities. |

---

## 3. Data Architecture & Relational Schema

The data model is engineered around 23 tables structured within Supabase PostgreSQL:

```mermaid
erDiagram
    SECTORS ||--o{ PROJECTS : categorizes
    SERVICES ||--o{ PROJECTS : powers
    PROJECTS ||--o{ PROJECT_MEDIA : contains
    SECTORS ||--o{ DEVELOPMENTS : classifies
    LEADERSHIP ||--o{ TEAM_MEMBERS : manages
    DEPARTMENTS ||--o{ JOBS : offers
    INQUIRIES ||--o{ INQUIRY_RESPONSES : logs

    PROJECTS {
        uuid id PK
        string title
        string slug UK
        string sector_slug FK
        string primary_service FK
        string status "newly_launched | ongoing | completed | handed_over"
        string city
        string location
        string client
        string architect
        numeric value_pkr_millions
        integer progress_percent
        string covered_area
        string plot_area
        string storeys
        date start_date
        date completion_date
        string cover_image_url
        string[] gallery_urls
        boolean is_featured
    }

    DEVELOPMENTS {
        uuid id PK
        string title
        string slug UK
        string property_type "commercial | residential | mixed_use"
        string status "newly_launched | ongoing | completed"
        string location
        string storeys
        numeric starting_price_pkr
        string handover_quarter
        string banner_image_url
        jsonb specs
    }

    JOBS {
        uuid id PK
        string title
        string slug UK
        string department
        string location
        string employment_type "full_time | part_time | contract"
        string required_experience
        date closing_date
        text short_summary
        text full_description
        boolean is_published
        integer sort_order
    }

    INQUIRIES {
        uuid id PK
        string full_name
        string email
        string phone_whatsapp
        string company
        string project_city
        string service_interest
        string project_type
        numeric indicative_budget_pkr
        text project_description
        string status "new | contacted | proposal_sent | closed"
        timestamp created_at
    }
```

---

## 4. Administrative CMS Engineering (`/admin`)

The admin command center spans **100+ KB of robust TypeScript logic** designed for enterprise operations:

1. **Zero-Code Content Ingestion**:
   - Dynamic schema mapping via metadata tables.
   - Modals auto-render input types (`text`, `textarea`, `number`, `boolean`, `image`, `gallery`, `select`, `date`, `tags`) dynamically based on table metadata.
2. **Resilient Offline / Fallback Storage Layer**:
   - `mergeWithLocalRecords()`, `persistLocalRecord()`, and `removeLocalRecord()` ensure zero data loss during network hiccups or API rate limits.
3. **Cache Synchronization**:
   - Integrated with TanStack Query's cache invalidation bus (`invalidateContentCache()`), ensuring public visitor routes immediately reflect content changes without rebuilds or server restarts.
4. **Input Sanitization & Security**:
   - Centralized `sanitizeFormData()` strips malicious script payloads before persistence.
   - Enforces strict slug formatting, character length constraints, and required field validation.

---

## 5. Performance, SEO & Core Web Vitals

* **Suspense & Progressive Hydration**: Data loaders pre-populate critical above-the-fold queries (`homeQuery`, `siteSettingsQuery`, `pageSeoQuery`).
* **Adaptive Media Optimization**: Dedicated `ResponsiveImage` primitive serves responsive desktop and mobile asset variants with intrinsic aspect ratios and blur placeholders.
* **SEO Metadata Engine**:
  - Dynamically builds Open Graph, Twitter Cards, canonical links, and JSON-LD schema on a per-route basis.
  - Generates rich snippet schemas for construction projects, local business credentials, and corporate leadership.
* **Accessibility**: Full keyboard navigation, screen-reader annotations, focus trapping in modal dialogs, and reduced-motion mode via `useReducedMotion()`.
