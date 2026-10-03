# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (your market's job boards):
- **indeed.com** - largest general US job board
- **linkedin.com/jobs** - LinkedIn job listings (filter: United States, nationwide); also covered by `linkedin-search` CLI
- **dice.com** - tech-focused job board (optional)
- **ziprecruiter.com** - another major US board (optional)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. Write **each category in every language from your Languages table** (see Language scope above). Combine each query with your location terms (e.g. your city, region, or metro area) where the site supports it.

**Organize by function, not job title.** The same underlying work carries different titles across companies and markets (a "Data Scientist" role at one employer may be posted as "Insights Analyst" or "Data Consultant" at another). Name each priority category after the function it covers, and list several plausible job titles as query variants within that category rather than betting an entire priority tier on one exact title string.

### Priority 1: Full-Stack / Software Development (.NET)

These match Tony's strongest and most recent hands-on experience.

```
site:indeed.com "Software Engineer" entry level
site:indeed.com ".NET Developer" entry level
site:indeed.com "Software Developer" C# entry level
site:linkedin.com/jobs "Application Developer" United States
site:linkedin.com/jobs "Full Stack Developer" C# .NET
```

### Priority 2: IT / Business Systems Analyst & Data/Analytics

Tony's other genuine interest area - analyst work he enjoyed at Koch, not limited to "Business Systems Analyst"-titled roles.

```
site:indeed.com "IT Analyst" entry level
site:indeed.com "Business Systems Analyst" entry level
site:indeed.com "Data Analyst" SQL entry level
site:linkedin.com/jobs "Product Analyst" United States
site:linkedin.com/jobs "Systems Analyst" entry level United States
```

### Priority 3: Backend / API & Cloud (adjacent pivot)

Adjacent roles he could pivot into given AWS/Docker/Kubernetes exposure.

```
site:indeed.com "Backend Developer" C# SQL entry level
site:indeed.com "API Developer" .NET entry level
site:dice.com "Cloud Engineer" entry level AWS
site:linkedin.com/jobs "DevOps Engineer" entry level United States
```

### Priority 4: Broader IT / Technical Consulting

Wider net for general technical and IT roles.

```
site:indeed.com "IT Developer" entry level
site:indeed.com "Technical Consultant" .NET
site:linkedin.com/jobs "Junior Developer" United States
site:linkedin.com/jobs "Associate Software Engineer" United States
```

## Location Filter

Tony is open to relocation and remote work nationwide - this is a national search, not a commute-radius search:
- **Wichita, Kansas** - home base; local employers worth watching include Koch Industries and the Wichita aerospace corridor (Textron/Cessna, Boeing). Boeing absorbed Spirit AeroSystems in 2026 - Spirit no longer exists as a separate Wichita employer, don't search for it.
- **Remote (US)** - ideal, actively searched
- **Anywhere else in the US** - acceptable; open to relocating for the right role
- No borderline/too-far tiers apply given the nationwide scope

## Company Portal Checks

Beyond the role-keyword searches above, some companies are worth checking directly on their own career portal rather than relying on LinkedIn/freehire to surface them - large employers with many subsidiary brands (Koch) or huge posting volume (Microsoft) can bury a good match under LinkedIn's relevance ranking, or never post it to a third-party board at all.

**Method:** `WebSearch site:<company careers domain>` to find candidate postings, then `WebFetch` each one individually to confirm it's still live before treating it as real - job-search-engine indexes cache stale/closed postings (confirmed: a stale Koch link 404'd during a real check). Never present a posting you haven't verified live.

**Reliability is ATS-platform-dependent, confirmed by direct testing:**
- **Avature** (Koch): partial success - the category-listing page renders an initial slice server-side (~6 results) that WebFetch can read, but "load more" pagination is JS-driven and invisible to a plain fetch. The `&from=N` URL parameter does **not** paginate (tested - returns identical results regardless of N). Work around the cap with `site:` searches for specific role keywords, then verify each hit individually.
- **Workday** (Salesforce, and also the ATS behind the Baird and Kyndryl postings seen in `/rank`): renders **entirely client-side** - WebFetch gets nothing, not even a partial slice. Do not spend effort trying; these need a human browsing in an actual browser.
- **Custom in-house sites** (Microsoft): hit or miss per page - some pages return real static listings, others need a live search interaction WebFetch can't perform.
- **Marketing/pipeline-only pages** (Cisco, Adobe encountered so far): often show zero live listings even when the company is actively hiring - these run more on a "register your interest" waitlist model than published reqs. Don't treat an empty result as "nothing open," just as "nothing found this way."

**Watch list** (check periodically, not necessarily every `/scrape` run - this is manual per-company digging, not a CLI):
- **Koch Industries** (`koch.avature.net`) - Wichita HQ, Tony's former co-op employer (Flint Hills Resources is a Koch company). Check the "Information Systems & Technology" and "Data & Analytics" career-field categories.
- **Textron / Cessna** (`textron.com/careers` or LinkedIn company page) - Wichita aerospace corridor.
- **Boeing** (`boeing.com/careers`) - Wichita site; absorbed Spirit AeroSystems in 2026.
- **Garmin** (`garmin.com/careers`) - Olathe, KS; GPS/aerospace-adjacent tech, strong early-career hiring.
- **Cerner / Oracle Health** (`oracle.com/careers` or search "Oracle Health Kansas City") - Kansas City, MO; large enterprise healthcare-software employer, C#/.NET common in their stack.
- **T-Mobile** (`careers.t-mobile.com`) - Overland Park, KS HQ (former Sprint).
- **USAA** (`usaa.com/careers`) - large enterprise employer with a strong new-grad technology program; .NET/Java shop.
- **Applied Systems** (`applied.com/careers` or LinkedIn company page) - insurance software, direct C#/SQL/REST match; scored highest in Tony's `/rank` results, worth checking their own site for roles beyond what LinkedIn surfaced.
- **Microsoft** (`careers.microsoft.com`) - worth an occasional check of the "recent graduate" and "university internship" pages despite mixed hit rate; huge volume of postings, some genuinely entry-level.

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
