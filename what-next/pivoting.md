# What Next — Pivoting From Dorking Into a Full Investigation

> You ran your dorks. You found something. Now what? Here is 
where a lot
>of people get lost. Finding information is step one. Knowing what
> to do with it — how to pivot from a Google search result into a full
> intelligence picture, how to feed what you found into the right tools,
> how to connect the dots — is the actual skill. This section covers the
> complete pivot workflow from every type of dorking result into the next
> phase of investigation. Some of it is repeated, as you will repeat steps to get answers oftentimes.

---

## The Core Pivot Principle

Every piece of information you find through dorking is a seed. A seed
has no value sitting by itself. Its value is in what grows from it when
you plant it in the right tool, run it against the right database, and
cross-reference it against independent sources.

**The pivot mindset:**
- Every domain leads to IP addresses, certificates, subdomains, and history
- Every IP address leads to other domains, organization data, and geolocation
- Every email leads to breach data, platform registrations, and identity
- Every username leads to cross-platform accounts and behavioral patterns
- Every document leads to metadata, author identity, and organizational data
- Every credential leads to access verification and lateral movement paths
- Every exposed service leads to vulnerability research and version intelligence

Nothing you find through dorking is an endpoint. Everything is a doorway.

---

## Part 1 — Pivoting From Domain and Infrastructure Findings

### You Found: A Target Domain

**Immediate pivots:**
```
# Step 1 — DNS enumeration
# Find every subdomain associated with the domain
# Tools: DNSDumpster, SecurityTrails, crt.sh, Amass, Subfinder

# crt.sh — certificate transparency logs
https://crt.sh/?q=%.targetdomain.com

# DNSDumpster — visual DNS map
https://dnsdumpster.com → enter domain

# SecurityTrails — DNS history and subdomains
https://securitytrails.com/domain/targetdomain.com/dns

# Amass (local tool — most comprehensive)
amass enum -d targetdomain.com
amass enum -d targetdomain.com -passive

# Subfinder (local tool — fast passive enumeration)
subfinder -d targetdomain.com
subfinder -d targetdomain.com -all
```

**Step 2 — Technology fingerprinting:**
```
# What is the site built with?
# Tools: BuiltWith, Wappalyzer, WhatWeb

# BuiltWith — full technology stack
https://builtwith.com/targetdomain.com

# WhatWeb (local tool)
whatweb targetdomain.com
whatweb -v targetdomain.com

# Wappalyzer browser extension — live fingerprinting
# Install from browser extension store
# Navigate to target site — stack appears in toolbar
```

**Step 3 — Shodan lookup:**
```bash
# What does Shodan see on this domain?
shodan search hostname:targetdomain.com

# CLI lookup
shodan search hostname:targetdomain.com --fields ip_str,port,org,hostnames
```

**Step 4 — Historical intelligence:**
```
# What did this domain look like in the past?
# Wayback Machine
https://web.archive.org/web/*/targetdomain.com

# Old DNS records — what IPs did this domain point to?
https://securitytrails.com/domain/targetdomain.com/history/a

# Old subdomains from certificate history
https://crt.sh/?q=targetdomain.com&deduplicate=Y
```

---

### You Found: An IP Address

**Immediate pivots:**
```
# Step 1 — Reverse DNS — what domains point to this IP?
# Tools: ViewDNS, SecurityTrails, Shodan

https://viewdns.info/reverseip/?host=X.X.X.X

# Bing ip: operator — sites hosted on this IP
# In Bing search bar:
ip:X.X.X.X

# Step 2 — Shodan host lookup
shodan host X.X.X.X

# Step 3 — ASN and organization lookup
# Who owns this IP range?
https://bgp.he.net/ip/X.X.X.X
https://ipinfo.io/X.X.X.X

# Step 4 — Geolocation
https://ipinfo.io/X.X.X.X
https://www.maxmind.com/en/geoip-demo

# Step 5 — Threat intelligence — is this IP malicious?
https://virustotal.com/gui/ip-address/X.X.X.X
https://www.abuseipdb.com/check/X.X.X.X
https://greynoise.io/viz/ip/X.X.X.X

# Step 6 — Historical IP data
# What domains have pointed to this IP historically?
https://securitytrails.com/list/ip/X.X.X.X
```

---

### You Found: An Exposed Subdomain
```
# Step 1 — What is running on it?
# Direct browser visit + Wappalyzer
# Shodan lookup for the subdomain
shodan search hostname:sub.targetdomain.com

# Step 2 — Is it a forgotten or shadow IT asset?
# Check if it appears in the main site's sitemap
https://targetdomain.com/sitemap.xml

# Step 3 — Certificate analysis
# What else is on the same certificate?
https://crt.sh/?q=sub.targetdomain.com

# Step 4 — Is it a staging or development environment?
# These frequently have weaker security than production
# Look for exposed debug modes, verbose error messages
# Test with basic inputs — staging environments often skip WAF protection
```

---

### You Found: An Exposed Service (Login Panel, Admin Interface)
```
# Step 1 — Identify the software and version
# Check the page source for version strings
# Check HTTP response headers
curl -I https://targetdomain.com/admin

# Step 2 — Search for known vulnerabilities
https://nvd.nist.gov/vuln/search → enter software name and version
https://www.exploit-db.com → search software name
https://cve.mitre.org/cve/search_cve_list.html

# Step 3 — Check for default credentials
# Many exposed admin panels still use defaults
# Default credential databases:
https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials

# Step 4 — Check Shodan for other instances
# How widespread is this exposure?
shodan search http.title:"Software Name" port:8080

# Step 5 — Document and report (for authorized assessments)
# Screenshot the exposed panel
# Note the URL, software, version, and date
# Report through proper channels
```

---

## Part 2 — Pivoting From Credential and Data Findings

### You Found: An Exposed Credential or API Key

This is the pivot that requires the most discipline. Finding a credential
is not a license to use it. For authorized security assessments, the
workflow is verification and reporting. For bug bounty programs, it is
submission. For your own infrastructure, it is immediate remediation.
```
# Step 1 — Identify what the credential is for
# AWS access key format: AKIA followed by 16 characters
# Stripe live key: sk_live_
# GitHub token: ghp_ or github_pat_
# Google API key: AIza
# Slack token: xoxb- or xoxp-

# Step 2 — Assess scope (authorized testing only)
# AWS key — what permissions does it have?
# API key — what endpoints does it access?
# Never use credentials against systems you do not own

# Step 3 — For your own organization — revoke immediately
# AWS: IAM → Users → Security credentials → Make inactive
# GitHub: Settings → Developer settings → Personal access tokens → Revoke
# Stripe: Dashboard → Developers → API keys → Roll key

# Step 4 — Check if the credential has been used maliciously
# AWS CloudTrail logs show API call history
# GitHub token usage visible in security log
# Most APIs have usage logs — check them immediately

# Step 5 — Scrub from repository history
# git filter-repo is the current recommended tool
pip3 install git-filter-repo
git filter-repo --path-glob '*.env' --invert-paths
# Then force push — and immediately rotate all credentials
# that appeared in history regardless
```

---

### You Found: Leaked Database Credentials
```
# Step 1 — Identify the database type and connection format
# MySQL: mysql://user:password@host:3306/database
# PostgreSQL: postgresql://user:password@host:5432/database
# MongoDB: mongodb+srv://user:password@cluster.mongodb.net/database
# Redis: redis://:password@host:6379

# Step 2 — For authorized testing — verify connectivity only
# Do NOT browse, extract, or modify data
# Verification of access without data extraction is the boundary

# Step 3 — For your own infrastructure — remediate immediately
# Rotate credentials
# Audit who has accessed the database
# Check for unauthorized data access in logs
# Notify affected parties if data may have been accessed

# Step 4 — Check exposure scope
# How long has this been indexed by Google?
# site:github.com filetype:env "your_credential" — has anyone else found it?
# Check Shodan for the database port on your infrastructure
```

---

### You Found: A Data Breach or Paste Site Dump
```
# Step 1 — Verify authenticity
# Breach data is frequently fake, exaggerated, or recycled
# Sample check — do any records match known real data?
# Never verify by testing credentials against live systems

# Step 2 — Assess scope
# How many records?
# What data types — email, password hash, plaintext password, PII?
# What time period does the data cover?

# Step 3 — Identify the source
# What service does the email format suggest?
# Do the email domains cluster around a specific organization?

# Step 4 — Notify (if you have standing to do so)
# Organizations have legal obligation to notify affected users
# CISA has reporting mechanisms for critical infrastructure
# FBI IC3 for cybercrime reporting
# NCSC (UK) for UK-based incidents

# Step 5 — Check exposure for your own domains
https://haveibeenpwned.com/DomainSearch
# Enter your domain — see all breached accounts
```

---

## Part 3 — Pivoting From People and Identity Findings

### You Found: A Real Name
```
# Full cross-reference workflow
1. FastPeopleSearch → address history, phone, relatives
2. TruePeopleSearch → cross-reference, reverse address
3. ThatsThem → email address and vehicle
4. Radaris → business associations
5. Webmii → web presence aggregation
6. OpenCorporates → business registrations
7. Judyrecords → court records
8. LinkedIn → professional history
9. Spokeo → social media aggregation
10. Pipl → deep people search (paid for full results)
```

---

### You Found: An Email Address
```
# Full email pivot workflow
1. Thatsthem reverse email → real name and address
2. Hunter.io → other emails on same domain
3. Phonebook.cz → additional email discovery
4. Verifalia → confirm deliverability
5. EmailRep → reputation and breach presence
6. BreachDirectory → breach data check
7. Holehe (local) → platform registrations
8. GHunt (local, Gmail only) → Google account intelligence
9. Epieos → Google and Apple account lookup
10. HIBP → full breach history
```

---

### You Found: A Username
```
# Full username pivot workflow
1. WhatsMyName → fast platform sweep (600+ sites)
2. Maigret (local) → deep profile pull with data extraction
3. Namechk → additional platform coverage
4. Usersearch.org → dating site coverage
5. Social Searcher → live social media activity
6. Google dorks:
   "username" site:reddit.com
   "username" site:github.com
   "username" site:twitter.com
   "username" forum
7. Yandex search → Eastern European platform coverage
8. Dark web username search through Ahmia
```

---

### You Found: A Phone Number
```
# Full phone pivot workflow
1. SpyDialer → name + voicemail playback
2. FreeCarrierLookup → carrier and line type
3. Sync.me → social media connections
4. Mr. Number → community reports
5. ThatsThem reverse phone → full identity record
6. TruePeopleSearch reverse phone → address history
7. PhoneInfoga (local) → comprehensive automated scan
8. NumLookup → international coverage
```

---

### You Found: A Profile Photo
```
# Full reverse image pivot workflow
1. Yandex Images → best facial recognition, VK coverage
2. Google Images → mainstream web coverage
3. Bing Visual Search → additional index
4. TinEye → image history and first appearance date
5. PimEyes → broad web facial recognition
6. FaceCheck.ID → social media specific facial search
7. Search4Faces → VK-specific facial search
```

---

## Part 4 — Pivoting Into Specialist Tools

Once dorking and basic OSINT tools have produced a solid data picture,
these specialist tools take the investigation to the next level.

---

### Maltego — Visual Relationship Mapping

**URL:** maltego.com (Community Edition is free)

Maltego is the industry standard for visual OSINT relationship mapping.
It connects entities — people, domains, IP addresses, email addresses,
social profiles — with visual links showing how they relate to each other.

**What Maltego transforms do:**
A transform is an automated data enrichment step. You add an entity
(a domain, for example) and run transforms against it. Maltego
automatically queries DNS records, Shodan, VirusTotal, HIBP, social
media, and dozens of other sources and adds the results as linked
nodes on your graph.

**Basic Maltego workflow:**
```
1. Open Maltego → New graph
2. Drag a Domain entity onto the canvas
3. Enter your target domain
4. Right-click → Run transforms → All transforms
5. Watch the graph expand with DNS records, subdomains,
   related IPs, SSL certificates, and more
6. Right-click any new node → Run transforms
7. Keep expanding until the graph shows the complete picture
```

**Free community edition limitations:**
- 12 results per transform (paid removes limit)
- No commercial transforms
- Basic entity types only

Even with these limits the Community Edition produces graphs that would
take hours of manual work to build manually.

---

### Spiderfoot — Automated Multi-Source OSINT

**URL:** github.com/smicallef/spiderfoot

Spiderfoot is an open source OSINT automation framework. Give it a seed —
domain, IP, email, username, name — and it automatically queries dozens
of data sources simultaneously and aggregates the results.
```bash
# Install
pip3 install spiderfoot

# Run with web interface
python3 sf.py -l 127.0.0.1:5001

# Open browser to http://127.0.0.1:5001
# New Scan → enter target → select scan profile → run

# CLI scan
python3 sfcli.py -s targetdomain.com -t INTERNET_NAME -m sfp_dns,sfp_shodan,sfp_haveibeenpwned
```

**Spiderfoot scan profiles:**
- **All** — comprehensive, slow, noisy (many queries)
- **Passive** — only passive sources, no direct contact with target
- **Investigate** — balanced approach for most investigations
- **Footprint** — focused on infrastructure mapping

For OSINT investigations where you want zero contact with the target
infrastructure, use the **Passive** profile. It queries third-party
databases and historical sources only.

---

### theHarvester — Email and Domain Intelligence

**URL:** github.com/laramies/theHarvester

theHarvester is specifically designed for email address and subdomain
harvesting from public sources. It queries search engines, certificate
databases, and OSINT sources simultaneously.
```bash
# Install
pip3 install theHarvester

# Basic domain harvest
theHarvester -d targetdomain.com -b all

# Specific sources
theHarvester -d targetdomain.com -b google,bing,linkedin,hunter,certspotter

# Save results
theHarvester -d targetdomain.com -b all -f results.html

# Available sources
theHarvester -d targetdomain.com -b google      # Google search
theHarvester -d targetdomain.com -b bing        # Bing search
theHarvester -d targetdomain.com -b linkedin    # LinkedIn
theHarvester -d targetdomain.com -b hunter      # Hunter.io
theHarvester -d targetdomain.com -b certspotter # Certificate logs
theHarvester -d targetdomain.com -b shodan      # Shodan (requires API key)
theHarvester -d targetdomain.com -b virustotal  # VirusTotal
```

**What theHarvester returns:**
- Email addresses found across all queried sources
- Subdomains and hostnames
- IP addresses associated with the domain
- Employee names found in search results
- Hosts discovered through certificate transparency

---

### Recon-ng — The OSINT Framework

**URL:** github.com/lanmaster53/recon-ng

Recon-ng is a full-featured web reconnaissance framework modeled after
Metasploit. It has a module system where each module performs a specific
OSINT task — domain enumeration, contact harvesting, credential checking,
social media mapping, and more.
```bash
# Install
pip3 install recon-ng

# Start
recon-ng

# Inside recon-ng
[recon-ng] > workspaces create targetinvestigation
[recon-ng][targetinvestigation] > marketplace search
[recon-ng][targetinvestigation] > marketplace install recon/domains-hosts/certificate_transparency
[recon-ng][targetinvestigation] > modules load recon/domains-hosts/certificate_transparency
[recon-ng][targetinvestigation][certificate_transparency] > options set SOURCE targetdomain.com
[recon-ng][targetinvestigation][certificate_transparency] > run

# Key modules for dorking follow-up
recon/domains-hosts/certificate_transparency  # subdomain discovery
recon/domains-contacts/hunter_io              # email harvesting
recon/hosts-hosts/shodan_ip                   # Shodan enrichment
recon/domains-vulnerabilities/xssed           # XSS vulnerability check
recon/contacts-credentials/hibp               # breach checking
```

---

### OSINT Framework — The Tool Directory

**URL:** osintframework.com

Not a tool itself — a comprehensive, regularly maintained directory of
OSINT tools organized by category. When you need a tool for a specific
task and do not know what exists, OSINT Framework is the reference.

Categories include:
- Username search
- Email address search
- Domain name research
- IP address and MAC address research
- Social media
- People search engines
- Public records
- Maps and geospatial
- Images/videos/documents
- Dark web
- Threat intelligence
- Malware analysis

Bookmark this. Use it every investigation.

---

## Part 5 — Common Issues and Fixes

### Problem: Dork Returns No Results
```
Cause 1: Operator syntax error
Fix: Check for spaces after colons — site: example.com → site:example.com

Cause 2: Search too narrow
Fix: Remove one operator at a time until results appear
     Then add them back one by one to find the breaking point

Cause 3: Content not indexed
Fix: Try Bing and Yandex with the same query
     Try without site: to see if the content exists anywhere
     Check Wayback Machine for historical indexed versions

Cause 4: Google rate limiting
Fix: Slow down, use Startpage, rotate to other engines
     Try the search from a different network or VPN
```

---

### Problem: Too Many Results, Cannot Find Signal in Noise
```
Fix 1: Add more specific operators
       Add filetype: to narrow to specific document types
       Add site: to restrict to a specific domain
       Add date filter through Tools → Any time

Fix 2: Add exclusion operators
       -inurl:login to exclude login pages
       -site:wikipedia.org to exclude noise sources
       -"press release" to exclude corporate PR

Fix 3: Use exact phrase matching
       Wrap specific technical strings in quotes
       "exact technical string" is far more precise than loose terms

Fix 4: Use Carrot2 for clustering
       Paste results into Carrot2 to visually cluster by topic
       Helps identify signal categories in large result sets
```

---

### Problem: Found Something But Cannot Verify It
```
Fix 1: Cross-reference across independent sources
       One source is a lead. Two independent sources is intelligence.
       Three is confirmation.

Fix 2: Check historical versions
       Wayback Machine for web content
       Bing cache for recently changed pages
       Google cache for very recent pages

Fix 3: Document and preserve immediately
       Screenshot everything before verifying
       Dark web content and paste site content disappears quickly
       Evidence that exists now may not exist in an hour

Fix 4: Check metadata
       Documents contain creation dates, author information, software
       ExifTool on any downloaded file reveals everything embedded

Fix 5: Use Maltego for relationship mapping
       If you cannot verify a connection directly, map what you can confirm
       and look for indirect connections through shared infrastructure,
       accounts, or associates
```

---

### Problem: Investigation Hit a Dead End
```
Fix 1: Go back to basics
       Re-examine what you have found so far
       Are there data points you have not fully pivoted from?
       Every email, username, IP, and domain is a seed

Fix 2: Try different engines
       Google → Bing → Yandex → Baidu (if applicable)
       Each engine surfaces different content

Fix 3: Try historical sources
       Wayback Machine for archived content
       SecurityTrails for DNS history
       crt.sh for certificate history
       Google cache for recently removed content

Fix 4: Expand the search perimeter
       Look at associates, partners, and related entities
       Former employees on LinkedIn
       Related domains registered by the same registrant
       Co-hosted domains on the same IP (Bing ip: operator)

Fix 5: Dark web check
       Is there content about this target indexed by Ahmia or IntelX?
       Has this organization appeared on ransomware leak sites?
```

---

## Part 6 — Knowing When to Stop

OSINT investigations can expand infinitely. Every data point leads to
more data points. Professional investigators know when to stop and how
to define the boundary between productive investigation and rabbit holes.

**Define your objective before you start.**
A clearly defined objective — "confirm whether this email address belongs
to this person" or "identify what internet-facing services this organization
runs" — gives you a stopping condition. When you have answered the question,
the investigation is done.

**Document as you go, not at the end.**
Investigations that are not documented as they happen produce incomplete
records. Screenshot everything. Note timestamps. Record sources. Write
interim notes. An investigation that produces a mountain of browser tabs
and no documentation has produced nothing.

**Know your legal boundaries.**
Passive OSINT — reading publicly available information — is legal.
Accessing systems, using found credentials, or interacting with
infrastructure you do not own is not. The line is clear. Stay on the
right side of it.

**Know when to refer.**
If an investigation reveals evidence of serious ongoing crime —
particularly crimes involving harm to people — the appropriate action
is referral to law enforcement, not continued personal investigation.
Continued investigation can compromise evidence, create legal liability,
and put investigators at personal risk.

OSINT is a skill. Using it responsibly is the other half of the skill.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*