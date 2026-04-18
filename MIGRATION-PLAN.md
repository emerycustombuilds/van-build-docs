# Docs Subdomain Migration Plan

**Date:** April 18, 2026
**Goal:** Redirect all docs.emerycustombuilds.com traffic to equivalent pages on the main Astro site, then sunset the subdomain.

## Why

The docs subdomain (Mintlify) has 13 pages covering electrical, plumbing, build process, and tools. The main Astro site now has **better, more complete versions** of all this content under `/systems/`, `/our-process/`, `/van-conversion-cost-guide/`, and `/tools/`. The docs site is:

- Splitting domain authority (subdomain = separate domain for Google)
- Outdated (still says "Costa Mesa", old pricing ranges)
- Redundant — every topic is covered better on the main site

## Content Audit Summary

| Docs Page | Main Site Equivalent | Verdict |
|---|---|---|
| `/docs/introduction` | `/systems/` | Redundant — general welcome page |
| `/docs/getting-started` | `/systems/` | Redundant — tier overview already on main site |
| `/docs/electrical/overview` | `/systems/electrical/` | Main site is better (answer summaries, schema) |
| `/docs/electrical/system-sizing` | `/systems/electrical/battery-sizing/` | Main site covers same calc walkthrough |
| `/docs/electrical/solar-guide` | `/systems/electrical/solar/` | Main site is more detailed |
| `/docs/electrical/battery-bank` | `/systems/electrical/lithium-battery-guide/` | Main site goes deeper |
| `/docs/plumbing/overview` | `/systems/plumbing/` | Main site is better |
| `/docs/plumbing/water-systems` | `/systems/plumbing/freshwater/` + `/greywater/` + `/blackwater/` | Split into 3 dedicated pages on main site |
| `/docs/plumbing/sizing` | `/systems/plumbing/freshwater/` | Covered in freshwater page |
| `/docs/build-process/overview` | `/our-process/` | Main site version is polished, has schema markup |
| `/docs/build-process/budgeting` | `/van-conversion-cost-guide/` + `/van-conversion-cost/` section | Multiple dedicated cost pages |
| `/docs/build-process/timeline` | `/our-process/` | Timeline info is on the process page |
| `/docs/tools/electrical-calculator` | `/tools/solar-calculator/` | Interactive calculator on main site |

**Unique content worth salvaging:** None. Main site covers everything with better SEO optimization.

## Redirect Map

| Old URL (docs.emerycustombuilds.com) | New URL (emerycustombuilds.com) |
|---|---|
| `/docs/introduction` | `/systems/` |
| `/docs/getting-started` | `/systems/` |
| `/docs/electrical/overview` | `/systems/electrical/` |
| `/docs/electrical/system-sizing` | `/systems/electrical/battery-sizing/` |
| `/docs/electrical/solar-guide` | `/systems/electrical/solar/` |
| `/docs/electrical/battery-bank` | `/systems/electrical/lithium-battery-guide/` |
| `/docs/plumbing/overview` | `/systems/plumbing/` |
| `/docs/plumbing/water-systems` | `/systems/plumbing/freshwater/` |
| `/docs/plumbing/sizing` | `/systems/plumbing/freshwater/` |
| `/docs/build-process/overview` | `/our-process/` |
| `/docs/build-process/budgeting` | `/van-conversion-cost-guide/` |
| `/docs/build-process/timeline` | `/our-process/` |
| `/docs/tools/electrical-calculator` | `/tools/solar-calculator/` |
| `/docs/*` (catch-all) | `/systems/` |

## Implementation Steps

### Step 1: Try Mintlify Redirects (Done)
Added `redirects` array to `mint.json` with all 13 page-level redirects + wildcard catch-all. Need to push to Mintlify and test.

### Step 2: If Mintlify Doesn't Support External Redirects → Cloudflare Bulk Redirect
1. Go to Cloudflare dashboard → emerycustombuilds.com zone
2. Bulk Redirects → Create a Bulk Redirect List
3. Add each redirect (source URL → destination URL, 301 permanent)
4. Deploy the redirect list as a Bulk Redirect Rule

### Step 3: Verify Redirects
Test each URL to confirm it 301s to the correct main site page.

### Step 4: Remove Google Site Verification
The docs site has `"google-site-verification": "k7fdwSKSzeTbibtBlfjwsiqut4nSXb_DpnfdQlkoIVU"` — once redirects are confirmed, remove the docs property from Google Search Console (after any remaining crawl credit transfers via the 301s).

### Step 5: Sunset Subdomain (After ~30 days of redirects)
1. Keep redirects running for at least 30 days so Google crawls and transfers signals
2. After 30 days, optionally remove the DNS CNAME for `docs.` subdomain
3. Archive the `van-build-docs/` folder (it's already in `Strategy & Planning/Future Projects/`)

## Notes
- The docs site's Google site verification suggests it may have been submitted to Search Console at some point. Check if there's a separate property for `docs.emerycustombuilds.com` and use the URL Removal tool or Change of Address if applicable.
- The old budget guide PDF link in the budgeting page (`/wp-content/uploads/2026/02/ECB-Van-Build-Budget-Guide.pdf`) points to the old WordPress site — that's dead now. Not our problem since we're redirecting away from the docs page entirely.
