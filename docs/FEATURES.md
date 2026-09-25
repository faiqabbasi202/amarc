# Functional Specifications & Dynamic Feature Matrix

> **Platform:** Enterprise Construction & Infrastructure Web Solution  
> **Purpose:** Turnkey Sample & Architectural Demonstration for Construction Clients  
> **Core Architecture:** 100% Dynamic Front-End & 23-Schema Admin CMS  
> **Live Demo:** [amarc-construction.lovable.app](https://amarc-construction.lovable.app/)

---

## 🏛️ 1. Client-Facing Dynamic Modules & Admin Controls

Every module below is designed for visual excellence on the front end and **100% operational autonomy** in the back office.

| Front-Facing Module | Client Experience & Functionality | ⚙️ Admin CMS Control (`/admin`) |
| :--- | :--- | :--- |
| **Hero & Credibility Bar** | Parallax depth scrolling, headline copy, PEC/ISO badges, and 4 live counter metrics. | Configured via `home_sections` & `site_settings`. Change text, badges, and numbers anytime. |
| **9 Engineering Disciplines** | Editorial 3×3 grid (Architecture, Structure, Construction, PM, Materials, etc.). | Managed via `services` table. Add, modify, or reorder service cards dynamically. |
| **Industry Sector Matrix** | 8 industry verticals (Residential, Commercial, Industrial, Infrastructure, etc.). | Managed via `sectors` table. Add emerging sectors or modify routing filters. |
| **Project Portfolio Grid** | Filterable catalog with status tags (`Newly Launched`, `Ongoing`, `Completed`) and PKR values. | Managed via `projects` table. Add projects, manage multi-image galleries, and update progress. |
| **Audited 6-Phase Lifecycle** | Step-by-step workflow (01 Feasibility $\rightarrow$ 06 Handover) guaranteeing quality assurance. | Configured via `process_steps` schema. Update phase deliverables and sign-off criteria. |
| **Real Estate Developments** | Dedicated listings for high-rises and residential villas with unit specs and handover quarters. | Managed via `developments` table. Update storeys, pricing tiers, and blueprint renders. |
| **Two-Decade Timeline** | Interactive chronological milestones highlighting corporate pedigree and achievements. | Managed via `milestones` table. Add new milestones, awards, or branch openings. |
| **Careers & Talent Portal** | Transparent culture metrics (65% promotions, 85% retention) and expandable job vacancies. | Managed via `jobs` table. HR can post vacancies, set expiration dates, and review applicants. |
| **Client Intake & Quotation** | Dual-column intake form collecting project city, service category, plot size, and budget tier. | Routed to `inquiries` inbox. Leads are categorized with status flags (`New`, `Proposal Sent`). |

---

## ⚙️ 2. Enterprise Admin CMS Portal Capabilities

* **23 Relational Schemas**: Spans Portfolio, Company, Content, and Operations.
* **Dynamic Form Generator**: Dynamically provisions text, textarea, select, numeric, calendar, and image controls based on table metadata.
* **Instant Cache Invalidation (`Purge Cache`)**: One-click invalidation flushes TanStack Query cache across all connected visitor instances.
* **Automated Asset Optimizer**: High-quality 95% image processing supporting `.webp`, `.jfif`, and `.jif` uploads.
* **Offline-Resilient Persistence**: Changes are preserved in a local fallback store, ensuring zero data loss during network drops.
