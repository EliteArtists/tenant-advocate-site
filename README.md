# Tenant Advocate

**Your guide to the safest letting agents in the UK.**

Tenant Advocate is composed of three main pieces:

1. **tenant-advocate-site** – A static frontend (plain HTML/CSS/JS) that fetches data from our API, renders agent cards, and lets users click to filter by agency.  
2. **tenant-advocate-api** – A serverless Node.js function (hosted on Vercel) backed by Supabase, exposing a REST endpoint to `GET` and `POST` letting agents.  
3. **TenantAdvocate (app)** – A future mobile/web application (React Native / React) that will consume the same API and offer a richer UX.

All data (agents’ names, locations, slugs, images, ratings) is stored in a **Supabase** Postgres database. Both the API and the frontend are deployed via **Vercel**.

---

## 📁 Repositories

- **frontend:** [`tenant-advocate-site`](https://github.com/EliteArtists/tenant-advocate-site)  
- **backend:** [`tenant-advocate-api`](https://github.com/EliteArtists/tenant-advocate-api)  
- **mobile/app:** _TBD_ (`TenantAdvocate`)

---

## 🔧 Prerequisites

- **Node.js** ≥14 (for local dev of the API)  
- **Supabase** account & CLI  
- **Vercel** account & CLI  
- Google Places API key (for future bulk-import)  
- (Optional) Trustpilot API credentials

---

## 🏗️ Architecture

```text
[ Supabase Postgres ]
      ▲       ▲
      |       |
[tenant-advocate-api]  ←─ HTTP ─→  [tenant-advocate-site]
      ▲
      |
[TenantAdvocate app]
