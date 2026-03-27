# Google Search Operators

> Most people think Google is just a search bar. It is not. It is a filtering
> engine with dozens of built-in commands that let you slice through billions
> of pages with surgical precision. This section covers every operator worth
> knowing — what it does, real syntax, and what actually works in 2026.

---

## What Is a Search Operator?

A search operator is a special command you type directly into the Google search
bar to modify what results come back. No special software. No accounts. No
setup. Just you, a browser, and the right syntax.

Think of it like the difference between walking into a library and saying
"I want something about dogs" versus walking in and saying "I want a hardcover
book published between 2018 and 2022, written by a veterinarian, about golden
retrievers specifically, that is available in this branch right now." Both are
valid requests. One gets you a pile of random books. The other gets you exactly
what you need.

Operators are how you go from the first request to the second.

---

## The Golden Rules of Operator Syntax

Before anything else — the mistakes that break most dorks happen here.

| Rule | Correct | Wrong |
|---|---|---|
| No space after the colon | `site:example.com` | `site: example.com` |
| Quotes for exact phrases | `"internal use only"` | `internal use only` |
| Operators are lowercase | `filetype:pdf` | `FileType:PDF` |
| Combine without AND | `site:example.com filetype:pdf` | `site:example.com AND filetype:pdf` |
| Exclude with minus | `site:example.com -inurl:login` | `site:example.com NOT login` |

A single space after a colon breaks the operator completely. Google will treat
it as a regular word and your dork will return garbage results. This is the
number one syntax mistake beginners make.

---

## Core Operators — The Foundation

These are the operators that form the backbone of almost every dork. Learn
these first.

---

### `site:`

**What it does:** Limits all results to a specific website or domain.

**Syntax:**
```
site:example.com
site:gov
site:edu
site:.mil
```

**Real examples:**
```
site:tesla.com filetype:pdf
site:reddit.com "forgot my password"
site:gov "budget report" filetype:xlsx
```

**What it finds:** Everything Google has indexed from that domain. This is
your starting point for almost every target-specific investigation.

**Common mistake:** People use `www` when they do not need to.
`site:example.com` already covers `www.example.com` and all subdomains.
`site:www.example.com` is more restrictive and will miss subdomains.

**2026 status:** ✅ Fully working. One of the most reliable operators.

---

### `filetype:`

**What it does:** Returns only results of a specific file type.

**Syntax:**
```
filetype:pdf
filetype:xlsx
filetype:docx
filetype:txt
filetype:xml
filetype:json
filetype:sql
filetype:log
filetype:env
filetype:bak
```

**Real examples:**
```
site:company.com filetype:pdf "confidential"
filetype:xlsx "employee" "salary"
filetype:sql "password" "insert into"
filetype:env "DB_PASSWORD"
```

**What it finds:** File types that were uploaded to public servers and indexed.
The dangerous ones — `.env`, `.sql`, `.log`, `.bak` — are files that should
never be publicly accessible but frequently are due to misconfiguration.

**Also works as:** `ext:` — same operator, different syntax. `ext:pdf` and
`filetype:pdf` return the same results.

**2026 status:** ✅ Fully working.

---

### `inurl:`

**What it does:** Returns results where the specified word or phrase appears
somewhere in the URL of the page.

**Syntax:**
```
inurl:admin
inurl:login
inurl:dashboard
inurl:config
inurl:backup
```

**Real examples:**
```
inurl:admin site:targetdomain.com
inurl:login filetype:php
inurl:"/wp-admin/" site:wordpress.com
inurl:config filetype:xml
```

**What it finds:** Admin panels, login pages, configuration files, backup
directories — anything where the path itself reveals what the page is.

**The difference between `inurl:` and `site:`:**
- `site:example.com` — must be from this domain
- `inurl:admin` — the word "admin" must appear in the URL, any domain

**2026 status:** ✅ Fully working.

---

### `intitle:`

**What it does:** Returns results where the specified word or phrase appears
in the title of the page — the text that shows at the top of your browser tab.

**Syntax:**
```
intitle:"index of"
intitle:"login page"
intitle:"dashboard"
intitle:"admin panel"
```

**Real examples:**
```
intitle:"index of" "parent directory"
intitle:"webcam" inurl:view
intitle:"phpMyAdmin" inurl:index.php
intitle:"Dashboard" inurl:admin
```

**What it finds:** This is one of the most powerful operators for finding
exposed systems. Page titles are set by developers and often reveal exactly
what a page is — "Index of /" means an open directory listing. "phpMyAdmin"
means an exposed database management interface.

**`intitle:` vs `allintitle:`:**
- `intitle:admin login` — "admin" must be in the title, "login" can be anywhere
- `allintitle:admin login` — both "admin" AND "login" must be in the title

**2026 status:** ✅ Fully working.

---

### `intext:`

**What it does:** Returns results where the specified word or phrase appears
in the body text of the page.

**Syntax:**
```
intext:"password"
intext:"api_key"
intext:"confidential"
```

**Real examples:**
```
intext:"password" intext:"username" filetype:log
intext:"api_key" site:github.com
intext:"BEGIN RSA PRIVATE KEY"
```

**What it finds:** Content buried inside pages — credentials in log files,
API keys in public documents, private keys accidentally pasted somewhere.

**2026 status:** ✅ Fully working.

---

### `cache:`

**What it does:** Shows Google's cached (saved) version of a webpage from
the last time Google crawled it.

**Syntax:**
```
cache:example.com
cache:example.com/specific-page
```

**What it finds:** Pages that may have been taken down or modified since
Google last crawled them. If a company removes an exposed document, the cached
version may still be accessible.

**2026 status:** ⚠️ Partially working. Google has been reducing cache
availability. For reliable cached page access, use the
[Wayback Machine](https://web.archive.org) instead.

---

### `link:`

**What it does:** Was supposed to show pages that link to a specific URL.

**2026 status:** ❌ Deprecated. No longer returns reliable results. Use
[Ahrefs](https://ahrefs.com) or [Moz Link Explorer](https://moz.com/link-explorer)
for backlink research.

---

### `related:`

**What it does:** Returns websites that Google considers similar to the
specified site.

**Syntax:**
```
related:nytimes.com
related:github.com
```

**What it finds:** Competitor sites, similar platforms, related services.
Useful for mapping an industry or finding alternative targets in scope.

**2026 status:** ✅ Works, though results can be inconsistent.

---

### `define:`

**What it does:** Returns the definition of a word directly in Google results.

**2026 status:** ✅ Works. Limited OSINT use but included for completeness.

---

## Combining Operators — Where the Power Is

Single operators are useful. Combinations are where dorking becomes a real
intelligence tool.

The logic is simple: every operator you add narrows the results further. You
are stacking filters on top of each other until only exactly what you are
looking for remains.

**Syntax rules for combining:**
- Just put them next to each other with a space between them
- No need for AND — Google assumes AND by default
- Use `-` (minus) to exclude something
- Use `OR` (uppercase) to allow either of two things
- Use quotes for exact phrases

**Examples:**
```
# Find PDFs on a specific site that mention a specific phrase
site:company.com filetype:pdf "internal use only"

# Find login pages on a domain that are PHP-based
site:company.com inurl:login filetype:php

# Find exposed directory listings
intitle:"index of" site:company.com

# Find pages mentioning username and password in log files
filetype:log intext:"username" intext:"password"

# Find either of two file types
site:company.com (filetype:pdf OR filetype:docx) "confidential"

# Find something but exclude a specific subdomain
site:company.com -site:blog.company.com filetype:pdf

# Find exposed environment files with database credentials
filetype:env intext:"DB_PASSWORD"

# Find pages with admin in the title but exclude common login pages
intitle:"admin" -inurl:"/login" -inurl:"/signin"
```

---

## Operators That No Longer Work (2026)

Google has quietly retired several operators over the years. Knowing which
ones are dead saves you from wasting time debugging syntax that was never
going to work.

| Operator | Status | What Happened |
|---|---|---|
| `link:` | ❌ Dead | Removed. Use Ahrefs or Moz. |
| `inanchor:` | ❌ Dead | No longer functional. |
| `allinanchor:` | ❌ Dead | No longer functional. |
| `phonebook:` | ❌ Dead | Removed years ago. |
| `info:` | ⚠️ Unreliable | Sometimes works, often doesn't. |
| `daterange:` | ❌ Dead | Use Tools → Any time filter instead. |
| `author:` | ❌ Dead | Was for Google Groups. Gone. |
| `cache:` | ⚠️ Reduced | Still exists but increasingly limited. |

---

## Using the Date Filter Without Operators

One of the most underused features in Google search is the **time filter** —
and it does not require any operator syntax at all.

After running any search:
1. Click **Tools** (below the search bar, to the right)
2. Click **Any time**
3. Select a time range — Past hour, Past 24 hours, Past week, Past month,
   Past year, or Custom range

This is essential for finding **recent** exposures rather than old indexed
results. If you are researching a fresh breach or a new vulnerability, filter
to the past week to cut through the noise.

---

## Quick Reference Table

| Operator | What It Does | Example |
|---|---|---|
| `site:` | Limit to a domain | `site:example.com` |
| `filetype:` | Limit to file type | `filetype:pdf` |
| `ext:` | Same as filetype | `ext:sql` |
| `inurl:` | Word in URL | `inurl:admin` |
| `intitle:` | Word in page title | `intitle:"index of"` |
| `intext:` | Word in page body | `intext:"api_key"` |
| `allintitle:` | All words in title | `allintitle:admin panel` |
| `allinurl:` | All words in URL | `allinurl:admin login` |
| `allintext:` | All words in body | `allintext:username password` |
| `cache:` | Cached version | `cache:example.com` |
| `related:` | Similar sites | `related:github.com` |
| `"phrase"` | Exact phrase | `"internal use only"` |
| `-word` | Exclude word | `-inurl:login` |
| `OR` | Either term | `filetype:pdf OR filetype:doc` |
| `*` | Wildcard | `"password * reset"` |
| `..` | Number range | `2020..2024` |

---

## Common Mistakes and Fixes

| Problem | Likely Cause | Fix |
|---|---|---|
| No results at all | Space after colon | Remove space: `site:example.com` not `site: example.com` |
| Too many results | Not specific enough | Add more operators to narrow down |
| Results from wrong sites | Missing `site:` | Add `site:` to lock to a target |
| Exact phrase not matching | Missing quotes | Wrap phrase in quotes: `"exact phrase"` |
| Operator not working | Deprecated operator | Check the deprecated list above |
| Getting login pages | Need to exclude | Add `-inurl:login` to filter them out |
| Old results showing up | No date filter | Use Tools → time filter |

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*