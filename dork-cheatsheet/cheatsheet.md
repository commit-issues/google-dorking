# Dork Cheatsheet

> One page. Everything. Bookmark it.

---

## Google Operators

| Operator | Syntax | What It Does |
|---|---|---|
| `site:` | `site:example.com` | Restrict to domain |
| `filetype:` | `filetype:pdf` | Specific file type |
| `ext:` | `ext:sql` | Same as filetype |
| `inurl:` | `inurl:admin` | Word in URL |
| `intitle:` | `intitle:"index of"` | Word in page title |
| `intext:` | `intext:"api_key"` | Word in page body |
| `allintitle:` | `allintitle:admin login` | All words in title |
| `allinurl:` | `allinurl:admin login` | All words in URL |
| `cache:` | `cache:example.com` | Cached version |
| `related:` | `related:github.com` | Similar sites |
| `"phrase"` | `"internal use only"` | Exact phrase |
| `-word` | `-inurl:login` | Exclude word |
| `OR` | `pdf OR doc` | Either term |
| `*` | `"password * reset"` | Wildcard |
| `..` | `2020..2024` | Number range |

---

## Bing-Only Operators

| Operator | Syntax | What It Does |
|---|---|---|
| `ip:` | `ip:X.X.X.X` | All sites on IP |
| `inbody:` | `inbody:"password"` | Body text only |
| `contains:` | `contains:pdf` | Page links to filetype |
| `feed:` | `feed:example.com` | RSS feeds |
| `loc:` | `loc:ru` | Country filter |
| `language:` | `language:ru` | Language filter |

---

## Yandex-Only Operators

| Operator | Syntax | What It Does |
|---|---|---|
| `url:` | `url:admin` | Word in URL |
| `title:` | `title:"index of"` | Word in title |
| `mime:` | `mime:pdf` | File type |
| `lang:` | `lang:ru` | Language filter |
| `date:` | `date:20240101..20241231` | Date range |
| `host:` | `host:sub.example.com` | Exact hostname |
| `inlink:` | `inlink:example.com` | Pages linking to URL |

---

## Golden Rules
```
No space after colon     site:example.com ✅    site: example.com ❌
Quotes for phrases       "exact phrase"   ✅    exact phrase      ❌
Operators lowercase      filetype:pdf     ✅    FileType:PDF      ❌
Combine with space       site:x.com filetype:pdf (no AND needed)
Exclude with minus       -inurl:login
```

---

## High Value File Types
```
filetype:env        filetype:sql        filetype:log
filetype:bak        filetype:conf       filetype:yml
filetype:xml        filetype:json       filetype:pem
filetype:key        filetype:csv        filetype:xlsx
```

---

## Top Dork Recipes
```bash
# Open directories
intitle:"index of" site:target.com

# Exposed env files
filetype:env intext:"DB_PASSWORD"

# Private keys
"BEGIN RSA PRIVATE KEY" site:github.com

# Admin panels
inurl:admin intitle:"login" site:target.com

# Exposed cameras
intitle:"Live View / - AXIS"
has_screenshot:true port:554

# SQL dumps
filetype:sql intext:"INSERT INTO" intext:"password"

# Full target recon
site:*.target.com -site:www.target.com

# Exposed cloud storage
site:s3.amazonaws.com "company name"

# Shadow IT
"company" site:notion.so OR site:trello.com OR site:airtable.com

# Paste site breach check
site:pastebin.com "target.com" "password"

# GitHub credential hunt
site:github.com filetype:env "AWS_SECRET_ACCESS_KEY"

# Job posting tech stack intel
site:linkedin.com/jobs "company" "engineer"

# Error pages revealing tech
site:target.com intext:"Fatal error" OR intext:"stack trace"
```

---

## GitHub Search
```bash
filename:.env DB_PASSWORD
filename:id_rsa
extension:pem "PRIVATE KEY"
org:targetcompany password
language:python "password ="
"BEGIN RSA PRIVATE KEY"
pushed:>2026-01-01 filename:.env
```

---

## Shodan Filters
```bash
port:3389 has_screenshot:true     # Exposed RDP
port:27017 -authentication        # Open MongoDB
port:9200 elasticsearch           # Exposed Elasticsearch
port:6379 -authentication         # Open Redis
port:554 has_screenshot:true      # Exposed cameras
port:502 Modbus                   # Industrial systems
org:"Company" port:3389           # Target RDP exposure
ssl:"targetdomain.com"            # Certificate search
vuln:CVE-2021-44228               # Log4Shell (paid)
```

---

## People Search — Quick Reference

| Seed | First Stop | Second Stop | Third Stop |
|---|---|---|---|
| Name | FastPeopleSearch | TruePeopleSearch | ThatsThem |
| Email | ThatsThem reverse | Skymem | Holehe |
| Phone | SpyDialer | Sync.me | PhoneInfoga |
| Username | WhatsMyName | Maigret | Namechk |
| Photo | Yandex Images | PimEyes | TinEye |
| Address | TruePeopleSearch | County Assessor | Judyrecords |
| Domain | SecurityTrails | crt.sh | DNSDumpster |
| IP | Shodan | ViewDNS | VirusTotal |

---

## Search Engine Stack

| Engine | Best For |
|---|---|
| Google | Primary — broadest coverage |
| Bing | `ip:` operator, different index |
| Yandex | Facial recognition, Eastern Europe, VK |
| Baidu | China-connected targets |
| Startpage | Unfiltered Google, anonymous browsing |
| SearXNG | Aggregate multiple engines at once |
| DuckDuckGo | Bang operators — `!gh` `!so` `!wayback` |

---

## Dark Web Quick Reference

| Tool | URL | Use |
|---|---|---|
| Ahmia | ahmia.fi | Dark web search — start here |
| IntelX | intelx.io | Dark web without Tor |
| Ransomware.live | ransomware.live | Leak site monitoring |
| Tor Browser | torproject.org | Tor access |
| Tails OS | tails.boum.org | Maximum anonymity |

---

## Pivot Quick Reference
```
Found domain     → crt.sh, DNSDumpster, Shodan, Wayback
Found IP         → Bing ip:, ViewDNS reverse, Shodan host, VirusTotal
Found email      → ThatsThem, Holehe, HIBP, Epieos
Found username   → WhatsMyName, Maigret, Sherlock
Found photo      → Yandex, PimEyes, TinEye, FaceCheck.ID
Found credential → Revoke immediately, check logs, scrub git history
Found breach     → HIBP, IntelX, BreachDirectory, notify
```

---

## Operators That Are Dead (2026)
```
link:        ❌ Dead — use Ahrefs
inanchor:    ❌ Dead
daterange:   ❌ Dead — use Tools → time filter
phonebook:   ❌ Dead
info:        ⚠️ Unreliable
cache:       ⚠️ Reduced — use Wayback Machine or Bing cache
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*