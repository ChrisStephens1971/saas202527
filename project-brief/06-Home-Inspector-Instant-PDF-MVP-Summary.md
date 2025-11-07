# Home Inspectors: Same‑Day Branded PDF from Phone

**Project Type:** Wedge MVP (4–6 weeks)  
**Primary Buyer:** Independent inspector / small firm  
**Target Users:** Inspectors, clients, agents  
**Problem Statement:** Word/Excel reports take days; photos scattered; cash collection delayed.

---

## 1) Objective
Build a narrow, high-ROI MVP that removes the most expensive step in the workflow, without rip-and-replace. Ship in 4–6 weeks with production quality.

## 2) Pain Signals (from the field)
- Agents chase updates
- Clients confused by 50‑page docs
- Inspectors drown in formatting

## 3) Core Jobs-To-Be-Done
- Capture on site
- Auto‑structure report
- Deliver same day
- Collect payment promptly

## 4) Wedge MVP Scope (what ships in 4–6 weeks)
- Mobile checklist with inline photos
- Findings short/long text + severity tagging
- Branded PDF generator with summary first
- Payment link and receipt
- Client/agent share link

**Out of scope for MVP (build later):**
- Calendar marketplace
- Insurance binders
- Drone scans

## 5) 60‑Day ROI Metrics (must improve or kill it)
- Turnaround time cut from days to same‑day
- DSO improved by 50%
- Repeat/referral rate up 15%

## 6) Pricing Hypothesis (launch)
- Pilot: $79/mo org, up to 30 reports/mo
- Pro: $149/mo org, up to 150 reports/mo
- Ops: $299/mo org, unlimited users + priority support

## 7) Key Workflows (E2E)
1. Inspector completes guided checklist; attaches photos
2. App generates PDF and secure link
3. Client pays invoice; agent receives summary
4. Archive with search

## 8) Core Entities & ERD Notes
- Entities: Inspection, Section, Finding, Photo, Invoice
- Relationships: Inspection 1‑N Sections; Section 1‑N Findings; Finding 1‑N Photos
- Multi‑tenant isolation by `org_id`; all queries scoped.

## 9) Integrations (MVP)
- Payments, PDF, email

## 10) Architecture Blueprint (AI can scaffold)
- Web: Next.js 14+ (App Router), React Server Components, Tailwind
- API: Node/TS (tRPC or REST) or Go (Fiber/Chi) behind a gateway
- DB: PostgreSQL (Prisma), row‑level security by `org_id`
- Auth: Passwordless/email + OAuth; organization membership & roles
- Realtime: Pusher/Ably (pub/sub) where needed
- Background jobs: durable queue (BullMQ or Cloud tasks)
- Files: S3‑compatible blob storage (signed URLs)
- Notifications: Transactional email + SMS provider
- Payments: Stripe (standard or Connect as noted)
- Hosting: Cloud provider with per‑env stacks; infra as code
- Observability: structured logs, traces, error tracker
- Feature flags: simple boolean flags in DB
- Admin: internal admin panel with audit log

## 11) Data Model Highlights
- `organizations(id, name, plan, …)`
- `users(id, email, …)`
- `memberships(user_id, org_id, role)`
- Domain entities: inspections, sections, findings, photos, invoices
- All tables include `org_id`, `created_at`, `updated_at`, `actor_id`

## 12) Security & Compliance
- RBAC: owner, manager, staff (expand per domain)
- Audit trail on create/update/delete of domain entities
- PII minimization; encryption at rest & TLS in transit
- Rate limits on public endpoints; signed URLs for files
- Least‑privilege DB roles; weekly offsite backups

## 13) Analytics & Telemetry
- Product analytics funnels on the top 3 workflows
- Business KPIs: the ROI metrics above, per org and cohort
- Export CSVs and webhook for BI tools

## 14) Launch Plan (Weeks 1–6)
- Week 1: confirm scope with 3 pilot customers; finalize schema; scaffold repo
- Week 2: implement domain entities and primary workflow
- Week 3: notifications + payments + basic admin
- Week 4: polish UX; seed data; write acceptance tests
- Week 5: onboard pilots; instrument metrics; fix field bugs
- Week 6: prove ROI; decision to expand or stop

## 15) Risks & Go/No‑Go Gates
- Template sprawl; photo storage cost; legal wording
- No‑Go if pilots don’t hit the ROI metrics by day 60

## 16) Acceptance Tests (MVP)
- Inspector delivers a clean PDF before leaving the property.

## 17) Sample User Stories
- As a client, I can see a prioritized summary before the full detail.

## 18) API Surface (outline)
- POST /auth/sign‑in
- POST /orgs
- CRUD /inspections
- CRUD /findings
- POST /webhooks/payment-succeeded
- GET  /reports/cycle-time

## 19) Backlog After MVP
- Template marketplace, e‑sign addenda, realtor CRM bridge

---

**Notes for the AI:** keep tenancy airtight, ship the smallest workflow that proves money saved or revenue recovered, collect evidence by default (timestamps, deltas, before/after). Do not gold‑plate. 
