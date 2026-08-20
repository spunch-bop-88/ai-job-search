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
- **Wichita, Kansas** - home base; local employers worth watching include Koch Industries and the Wichita aerospace corridor (Textron/Cessna, Spirit AeroSystems, Boeing)
- **Remote (US)** - ideal, actively searched
- **Anywhere else in the US** - acceptable; open to relocating for the right role
- No borderline/too-far tiers apply given the nationwide scope

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
