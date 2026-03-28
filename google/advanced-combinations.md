# Advanced Operator Combinations

> This is where everything comes together. Individual operators are tools.
> Combinations are weapons. This section covers chaining operators into
> real-world dork recipes, CTF-specific techniques, vulnerability research
> workflows, and recon methodologies that professionals use on actual
> engagements. If you read nothing else in this guide, read this section.

---

## How Chaining Actually Works

When you combine operators in Google, you are building a filter stack.
Every operator you add narrows the result set further. Think of it like
a funnel — the more layers you add, the more precisely you control what
comes out the bottom.

The logic Google uses by default:

- **Space between terms** = AND (both must be present)
- **OR** (uppercase) = either one or the other
- **Minus `-`** = exclude this completely
- **Quotes `""`** = this exact phrase, in this exact order
- **Parentheses `()`** = group logic together

Understanding these five rules lets you build searches of arbitrary
complexity. There is no limit to how many operators you can chain —
though in practice, more than five or six often produces zero results
because you have narrowed too far.

**The sweet spot:** Three to four operators usually produces the best
balance between precision and result volume.

---

## Part 1 — Real World Dork Recipes

These are complete, tested dork combinations organized by use case.
These are not theoretical — these are the searches that find real
exposed data in the wild.

---

### Recipe 1 — Full Target Reconnaissance

When you have a target domain and want to map everything Google has
indexed about it, run these in sequence. Each one reveals a different
layer of the target's public footprint.
```
# Step 1 — Everything Google has indexed from this domain
site:targetdomain.com

# Step 2 — All subdomains
site:*.targetdomain.com -site:www.targetdomain.com

# Step 3 — All indexed documents
site:targetdomain.com filetype:pdf OR filetype:docx OR filetype:xlsx

# Step 4 — Login and admin panels
site:targetdomain.com inurl:admin OR inurl:login OR inurl:dashboard

# Step 5 — Configuration and sensitive files
site:targetdomain.com ext:env OR ext:config OR ext:xml OR ext:yml

# Step 6 — Open directory listings
site:targetdomain.com intitle:"index of"

# Step 7 — Error pages revealing technology
site:targetdomain.com intext:"Fatal error" OR intext:"Warning:" OR intext:"stack trace"

# Step 8 — Exposed APIs
site:targetdomain.com inurl:api OR inurl:v1 OR inurl:v2 OR inurl:swagger

# Step 9 — Camera and device interfaces
site:targetdomain.com inurl:view OR inurl:cam OR inurl:webcam

# Step 10 — Third party mentions with sensitive context
"targetdomain.com" site:pastebin.com OR site:paste.ee OR site:ghostbin.com
```

Run these in sequence, document every result, and you have a complete
picture of a target's public Google footprint before touching a single
system.

---

### Recipe 2 — Exposed Credentials Hunt
```
# Environment files with database passwords
filetype:env intext:"DB_PASSWORD" -github.com

# Environment files with cloud credentials
filetype:env intext:"AWS_SECRET_ACCESS_KEY"
filetype:env intext:"AZURE_CLIENT_SECRET"
filetype:env intext:"GOOGLE_APPLICATION_CREDENTIALS"

# Private keys accidentally exposed
filetype:pem intext:"PRIVATE KEY"
filetype:key intext:"PRIVATE KEY"
intitle:"index of" "id_rsa" OR "id_dsa" OR ".pem"

# API keys in configuration files
filetype:yml intext:"api_key" intext:"secret"
filetype:json intext:"api_key" intext:"secret"
filetype:xml intext:"api_key" intext:"password"

# Credentials in log files
filetype:log intext:"password" intext:"username"
filetype:log intext:"Authorization: Bearer"
filetype:log intext:"api_key"

# SQL dumps with user tables
filetype:sql intext:"INSERT INTO" intext:"password"
filetype:sql intext:"CREATE TABLE users"

# Credentials on paste sites
site:pastebin.com "targetdomain.com" "password"
site:pastebin.com intext:"password" intext:"username" intext:"@gmail.com"
```

---

### Recipe 3 — Vulnerability Research Workflow

When researching whether a specific vulnerability affects systems in
the wild, Google dorking lets you find exposed instances without
touching any of them.
```
# Find specific software versions known to be vulnerable
intitle:"Apache/2.4.49" OR intitle:"Apache/2.4.50"
intitle:"phpMyAdmin 4.8" inurl:index.php
intitle:"WordPress 5.0" inurl:wp-login.php

# Find software with known default credentials
intitle:"Tomcat" inurl:":8080/manager/html"
intitle:"Jenkins" inurl:"/login" intext:"Remember me"
intitle:"Grafana" inurl:"/login" intext:"v6" OR intext:"v7"

# Find exposed Swagger/OpenAPI documentation
inurl:swagger-ui.html
inurl:api-docs
inurl:swagger/index.html
intitle:"Swagger UI"
intitle:"API Documentation"

# Find exposed Spring Boot actuators (information disclosure)
inurl:"/actuator/health"
inurl:"/actuator/env"
inurl:"/actuator/mappings"

# Find exposed Laravel debug pages
intext:"APP_DEBUG=true" intext:"Laravel"
intitle:"Whoops" intext:"Laravel"

# Find exposed Django debug pages  
intext:"DEBUG = True" intext:"Django"
intitle:"DisallowedHost" intext:"Django"

# Find exposed .git directories
intitle:"Index of /.git"
inurl:"/.git/config"
inurl:"/.git/HEAD" intext:"ref:"
```

**The exposed .git directory issue deserves special attention.** When a
developer deploys a web application and accidentally includes the `.git`
folder in the public web root, the entire source code history of the
application becomes accessible. Every commit. Every branch. Every file
that was ever in the repository — including files that were deleted but
remain in git history, like old configuration files with credentials.

Tools like **GitTools** and **git-dumper** can reconstruct the entire
repository from an exposed `.git` directory.

---

### Recipe 4 — Corporate Intelligence Gathering

For security assessments, competitive intelligence, and due diligence.
All passive. All legal. All from public sources.
```
# Technology stack from job postings
site:linkedin.com/jobs "company name" "engineer"
site:indeed.com "company name" "developer"
"company name" site:jobs.lever.co OR site:greenhouse.io

# Internal documents accidentally made public
site:targetdomain.com filetype:pdf "internal" OR "confidential" OR "draft"
site:targetdomain.com filetype:pptx OR filetype:ppt "roadmap" OR "strategy"

# Meeting notes and internal communications
"company name" site:notion.so
"company name" site:confluence.atlassian.net
"company name" filetype:pdf "meeting notes" OR "action items"

# Financial information
"company name" filetype:pdf "annual report" OR "quarterly report"
"company name" site:sec.gov filetype:pdf

# Employee contact information
"@targetdomain.com" site:linkedin.com
"@targetdomain.com" filetype:pdf OR filetype:docx
site:targetdomain.com "email" filetype:pdf

# Vendor and partner relationships
"targetdomain.com" "partner" OR "vendor" OR "integration" filetype:pdf
"powered by" "targetdomain.com"
```

---

### Recipe 5 — Shadow IT and Cloud Exposure

Organizations lose track of what their employees spin up. Cloud services,
SaaS tools, collaboration platforms — all of them can expose data.
```
# AWS S3 buckets (public buckets are indexed)
site:s3.amazonaws.com "company name"
site:s3.amazonaws.com "targetdomain"
inurl:".s3.amazonaws.com" "index"
intitle:"index of" site:s3.amazonaws.com

# Azure Blob Storage
site:blob.core.windows.net "company name"
inurl:".blob.core.windows.net"

# Google Cloud Storage
site:storage.googleapis.com "company name"

# Exposed collaboration tools
"company name" site:docs.google.com
"company name" site:trello.com
"company name" site:airtable.com
"company name" site:notion.so
"company name" site:miro.com

# Exposed code repositories
"company name" site:github.com
"company name" site:gitlab.com
"company name" site:bitbucket.org

# Staging and development environments
site:staging.targetdomain.com
site:dev.targetdomain.com
site:test.targetdomain.com
site:uat.targetdomain.com
site:sandbox.targetdomain.com
```

**The S3 bucket problem:** Amazon S3 is a cloud file storage service.
When an S3 bucket is configured as public — intentionally or by mistake —
its contents are accessible to anyone and indexed by Google. Public S3
buckets have exposed medical records, financial data, source code,
database backups, and internal documents from thousands of organizations.
Finding your own organization's public S3 buckets before an attacker does
is a standard cloud security audit step.

---

## Part 2 — CTF-Specific Dork Techniques

Capture The Flag competitions present different challenges than real-world
investigations. CTF targets are intentionally vulnerable, specifically
designed to be found, and usually hosted on specific platforms or IP ranges.

---

### CTF Reconnaissance Approach
```
# Finding CTF challenges on common platforms
site:hackthebox.com "challenge name"
site:tryhackme.com "challenge name"
site:ctftime.org "challenge name"

# Finding writeups for completed CTFs
"challenge name" "CTF" "writeup" site:github.com
"challenge name" "CTF" filetype:md site:github.com
"challenge name" "flag{" writeup
"challenge name" site:medium.com writeup

# Finding CTF infrastructure
"challenge name" inurl:ctf
intitle:"CTF" inurl:challenge
```

**The writeup research strategy:** When you are stuck on a CTF challenge,
searching for writeups from previous times the same challenge (or very
similar challenges) was run gives you methodology hints without necessarily
giving you the answer. Search for the general technique rather than the
specific challenge name to avoid spoilers while still learning the approach.

---

### CTF-Specific File and Flag Discovery
```
# Finding flag format patterns (most CTFs use a specific format)
intext:"flag{" site:pastebin.com
intext:"CTF{" filetype:txt
intext:"HTB{" writeup

# Finding exposed CTF infrastructure
inurl:ctf intitle:"login"
intitle:"CTF" inurl:admin
site:*.ctf.* intitle:"index of"

# Finding CTF tools and wordlists
site:github.com "CTF" "wordlist" filetype:txt
site:github.com "CTF toolkit" "pentest"
```

---

## Part 3 — Recon Dork Methodology for Penetration Testing

This section is for authorized penetration testers conducting reconnaissance
within the scope of an engagement. All of this is passive — no traffic is
sent to the target, no systems are touched.

---

### Phase 1 — Scope Mapping

Before running any dorks, map exactly what is in scope.
```
# Identify all domains associated with a company
"company name" site:crunchbase.com
"company name" inurl:whois
"company name" site:linkedin.com "website"

# Find all subdomains in scope
site:*.targetdomain.com
site:*.targetdomain.com -site:www.targetdomain.com -site:mail.targetdomain.com

# Find IP ranges (look for ASN ownership)
"targetcompany" site:bgp.he.net
"targetcompany" site:arin.net
```

---

### Phase 2 — Technology Fingerprinting
```
# Identify web frameworks
inurl:targetdomain.com "powered by WordPress"
inurl:targetdomain.com "powered by Drupal"
inurl:targetdomain.com "Joomla"
site:targetdomain.com intext:"React" OR intext:"Angular" OR intext:"Vue"

# Identify server software from error pages
site:targetdomain.com intext:"Apache" intext:"Server at"
site:targetdomain.com intext:"nginx" intext:"Welcome to"
site:targetdomain.com intext:"IIS" intext:"Windows Server"

# Identify cloud provider
site:targetdomain.com inurl:".s3.amazonaws.com"
site:targetdomain.com inurl:"azurewebsites.net"
site:targetdomain.com inurl:"herokuapp.com"
site:targetdomain.com inurl:"cloudfront.net"

# Identify third party services in use
site:targetdomain.com inurl:zendesk
site:targetdomain.com inurl:salesforce
site:targetdomain.com inurl:workday
```

---

### Phase 3 — Attack Surface Discovery
```
# Find all login points
site:targetdomain.com inurl:login OR inurl:signin OR inurl:auth
site:targetdomain.com intitle:"login" OR intitle:"sign in"

# Find all upload functionality
site:targetdomain.com inurl:upload OR inurl:file OR inurl:attach

# Find all API endpoints
site:targetdomain.com inurl:/api/
site:targetdomain.com inurl:/rest/
site:targetdomain.com inurl:/graphql

# Find all admin interfaces
site:targetdomain.com inurl:admin OR inurl:administrator OR inurl:manage

# Find all search functionality (SQLi surface)
site:targetdomain.com inurl:search OR inurl:query OR inurl:find

# Find all parameter-driven URLs (injection surfaces)
site:targetdomain.com inurl:"?id=" OR inurl:"?page=" OR inurl:"?file="
site:targetdomain.com inurl:"?user=" OR inurl:"?cat=" OR inurl:"?item="
```

**Why parameter-driven URLs matter:** URLs with parameters like `?id=1` or
`?file=document.pdf` are places where the application takes user input and
does something with it. These are the surfaces most commonly vulnerable to
injection attacks — SQL injection, path traversal, local file inclusion,
and others. Finding them through Google before testing lets you build a
comprehensive map of potential injection points on a target.

---

### Phase 4 — Historical and Archived Intelligence
```
# Find old versions of the site through Google cache
cache:targetdomain.com
cache:targetdomain.com/admin
cache:targetdomain.com/login

# Find mentions in security advisories and CVE reports
"targetdomain.com" site:nvd.nist.gov
"targetcompany" site:cve.mitre.org
"targetcompany" "vulnerability" site:securityfocus.com

# Find previous breach mentions
"targetdomain.com" site:haveibeenpwned.com
"targetcompany" "data breach" OR "security incident"
"targetcompany" "breach" site:bleepingcomputer.com

# Find previous pentest reports accidentally made public
"targetdomain.com" filetype:pdf "penetration test" OR "pentest report"
"targetcompany" filetype:pdf "vulnerability assessment"
```

---

## Part 4 — Dorking Automation and Tooling

Running dorks manually is fine for small investigations. For comprehensive
recon, automation matters.

---

### Tools That Automate Google Dorking

| Tool | What It Does | URL |
|---|---|---|
| **GooFuzz** | Automated Google dorking for file discovery | github.com/m3n0sd0n4ld/GooFuzz |
| **Pagodo** | Automates GHDB dorks against a target | github.com/opsdisk/pagodo |
| **DorkScout** | Automated dork runner with proxy support | github.com/R4yGM/dorkscout |
| **GHDB** | Google Hacking Database — library of proven dorks | exploit-db.com/google-hacking-database |
| **Dorkbot** | Batch dork runner | github.com/utiso/dorkbot |

**The Google Hacking Database (GHDB)** maintained by Exploit-DB is the
authoritative collection of proven dorks organized by category. It has
thousands of entries added by the security community since Johnny Long
started it in the early 2000s. Before building custom dorks for a specific
use case, always check the GHDB — someone has probably already built and
tested what you need.

---

### Avoiding Google Captcha and Rate Limiting

Google rate limits aggressive search activity. When you run many searches
quickly, you will start seeing CAPTCHA challenges. Techniques to minimize this:

- **Slow down.** Run searches manually with a few seconds between them.
  Automated tools should have rate limiting built in — configure it.
- **Use different search engines in rotation.** Bing, Yandex, and DuckDuckGo
  for the same dorks while Google recovers.
- **Avoid scraping.** Tools that scrape Google results at high speed will
  get your IP blocked quickly. Use the official
  [Google Custom Search API](https://developers.google.com/custom-search/v1/overview)
  for programmatic access — it has a free tier.
- **Use a VPN or residential proxy.** Changes your IP when you hit a block.
  Note that VPN IPs are often already flagged by Google.

---

## Part 5 — Building Your Own Dork Library

Professional OSINT investigators and red teamers maintain personal dork
libraries — collections of tested, effective dorks organized by use case.

**Recommended structure:**
```
dork-library/
  recon/
    subdomain-discovery.txt
    technology-fingerprinting.txt
    login-panels.txt
  credentials/
    env-files.txt
    config-files.txt
    database-files.txt
  intelligence/
    employee-enumeration.txt
    document-discovery.txt
    breach-mentions.txt
  ctf/
    platform-specific.txt
    flag-hunting.txt
```

**Format for each dork in your library:**
```
# Target: Login panels - PHP
# Last tested: 2026
# Works: Yes
# Notes: Combine with site: for targeted recon
inurl:login filetype:php intitle:"login"

# Target: Exposed .env files with DB credentials
# Last tested: 2026
# Works: Yes
# Notes: Remove -github.com to include GitHub results
filetype:env intext:"DB_PASSWORD" -github.com
```

Tracking when you last tested a dork and whether it still works is important
because Google changes its indexing and operator behavior over time. A dork
that worked in 2022 may produce different results in 2026.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*