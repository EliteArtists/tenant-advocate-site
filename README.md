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

	1.	tenant-advocate-site
	•	Fetches GET /api/agents
	•	Renders a grid of cards; each card has a data-slug attribute.
	•	Click a card or hash-link to filter by slug.
	2.	tenant-advocate-api
	•	Exposes /api/agents as a Vercel serverless function.
	•	Uses Supabase JS client to select(…) and return JSON.
	•	Supports POST to add new agents via the form on the frontend.
	3.	Supabase

id            serial
name          text
location      text
full_address  text
slug          text UNIQUE
image_url     text
google_rating numeric
trustpilot_rating numeric
created_at    timestamptz

•	Row-level security: public SELECT on agents.

	4.	Vercel
	•	Two projects: tenant-advocate-site (static) & tenant-advocate-api (functions).
	•	Managed via the Vercel dashboard; automatic deploys on main.

**QUICK START**

git clone https://github.com/EliteArtists/tenant-advocate-site.git
cd tenant-advocate-site

# No build step — just open index.html in your browser,
# or serve from a local static server:
npx serve .

3. Deploy
	•	API: vercel --prod from tenant-advocate-api
	•	Site: push to GitHub → automatic deploy on Vercel

**Bulk-Import Workflow**
	1.	Write a Node script that:
	•	Reads a list of agency names/addresses
	•	Calls Google Places API → retrieves photo_reference & rating
	•	Calls Trustpilot API → retrieves trustpilot_rating
	•	Upserts each row into Supabase via its REST or JS client
	2.	Run it once for each city—or automate with a cron job.
