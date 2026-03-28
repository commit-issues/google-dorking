# Cybersecurity Dorks & OSINT Intelligence

> **A note on this guide:** My background spans from technical engineering, AI, and
> cybersecurity. I write for everyone, like I teach and manage all levels — if you are brand new, every term gets
> explained in plain English. If you are seasoned or a professional, we go deep
> enough that you will still find things interesting, but feel free to skim and jump ahead. 

> **Legal disclaimer:** Everything in this section is passive reconnaissance
> using publicly available information. Do not use these techniques against
> systems or individuals without explicit written authorization. Do not
> engage in vigilantism. Knowing how to find information is not a license to
> act on it. Read the [Legal and Ethics](../legal-and-ethics/ethics.md) section.

---

## What Is OSINT and Why Does It Live Here?

**OSINT stands for Open Source Intelligence.** The word "open source" here
does not mean software — it means *publicly available*. OSINT is the practice
of collecting and analyzing information that anyone can legally access.

Think of it this way: a private investigator in the 1970s had to physically
follow someone, dig through trash, and make phone calls pretending to be
someone else. Today, a skilled OSINT investigator sitting at a laptop can
build a more complete profile of a person or organization in two hours than
that PI could in two weeks — using nothing but publicly available information
and the right tools.

OSINT is a core discipline within cybersecurity for a simple reason: **before
anyone attacks a system, they research it.** Knowing what information is
publicly available about a target is the first step in both attacking and
defending. Defenders use OSINT to understand what attackers can see. Attackers
use it to plan. Law enforcement uses it to investigate. Journalists use it to
report. Researchers use it to discover.

Google dorking is one of the most powerful OSINT techniques available — and
it is completely free.

---

## Part 1 — Finding Exposed Systems and Infrastructure

This is where dorking crosses from "interesting" into "genuinely alarming."
The number of systems sitting on the public internet with zero protection —
visible to anyone who knows the right search — is not small. It is enormous.

---

### Open Directory Listings

An open directory listing is what happens when a web server shows you its
file and folder structure like a filing cabinet with the drawer left open.
The server was configured incorrectly — or not configured at all — and instead
of showing a proper webpage, it shows you everything stored there.
```
intitle:"index of"
intitle:"index of /" "parent directory"
intitle:"index of" site:targetdomain.com
intitle:"index of" "backup"
intitle:"index of" ".sql"
intitle:"index of" ".env"
intitle:"index of" "passwords"
intitle:"index of" "private"
intitle:"index of" inurl:ftp
```

What you are looking for when you find one:
- `.sql` files — database dumps, often with usernames and passwords
- `.env` files — environment configuration, API keys, database credentials
- `.bak` files — backup files of databases or application code
- `config` folders — application configuration, often with credentials
- `logs` folders — system logs, error logs, access logs with user activity

---

### Exposed Login Panels and Admin Interfaces

Every web application has an admin panel somewhere. Most developers leave them
at predictable URLs. Most organizations forget to put them behind a VPN or
firewall. Google finds them.
```
inurl:admin intitle:"login"
inurl:"/admin/login" site:targetdomain.com
inurl:"/wp-admin/" site:targetdomain.com
inurl:"/administrator/" intitle:"login"
inurl:phpmyadmin intitle:"phpMyAdmin"
inurl:"/manager/html" intitle:"Tomcat"
inurl:":8080/manager" intitle:"Tomcat"
inurl:"/jmx-console" intitle:"JBoss"
inurl:cpanel intitle:"login"
inurl:plesk intitle:"login"
inurl:webmin intitle:"login"
intitle:"Grafana" inurl:"/login"
intitle:"Kibana" inurl:"/login"
intitle:"Jenkins" inurl:"/login"
intitle:"GitLab" inurl:"/users/sign_in"
intitle:"Portainer" inurl:"/#/auth"
intitle:"Traefik" inurl:"/dashboard"
```

**Plain English:** These are the back doors of websites. phpMyAdmin is a
visual interface for managing databases. Apache Tomcat and JBoss are server
management panels. Finding one of these exposed means someone left the
back door not just unlocked but with a sign pointing at it.

---

### Exposed Databases

Databases should never be directly accessible from the public internet. They
frequently are.
```
inurl:":3306" intitle:"phpMyAdmin"
inurl:":5432" intitle:"pgAdmin"
inurl:":27017" intitle:"MongoDB"
inurl:":6379" intitle:"Redis"
intitle:"phpMyAdmin" "Welcome to phpMyAdmin"
intitle:"MongoDB" inurl:":28017"
intitle:"Elasticsearch" inurl:":9200"
intitle:"InfluxDB" inurl:":8086"
filetype:sql intext:"INSERT INTO" intext:"password"
filetype:sql "CREATE TABLE" "users"
```

**What these mean:**
- Port `3306` = MySQL database
- Port `5432` = PostgreSQL database
- Port `27017` = MongoDB (NoSQL database)
- Port `6379` = Redis (cache/database, often stores session tokens)
- Port `9200` = Elasticsearch (search database, often contains sensitive records)

Redis and Elasticsearch are especially dangerous when exposed — Redis can
store active session tokens and authentication data, Elasticsearch databases
have been found containing millions of medical records, financial data, and
personal information with zero authentication.

---

### Exposed Configuration and Credential Files

These files should never be on a public web server. They exist there because
developers make mistakes — pushing the wrong folder, misconfiguring the server,
or simply not knowing these files are sitting in a publicly accessible location.
```
filetype:env intext:"DB_PASSWORD"
filetype:env intext:"AWS_SECRET"
filetype:env intext:"API_KEY"
filetype:env "APP_KEY"
filetype:env "SECRET_KEY"
filetype:xml intext:"password" intext:"username" inurl:config
filetype:yml intext:"password"
filetype:yaml intext:"password"
filetype:ini intext:"password"
filetype:conf intext:"password"
filetype:properties intext:"password"
filetype:json intext:"api_key"
filetype:json intext:"secret"
filetype:txt intext:"password" inurl:credentials
ext:cfg intext:"password"
ext:bak intext:"password"
ext:old intext:"password"
```

**Plain English:** A `.env` file is like a keychain that an application uses
to remember all its passwords — database passwords, API keys, secret tokens.
It is supposed to live on the server privately, never exposed to the web.
When it is publicly accessible and indexed by Google, every credential inside
it is visible to anyone who finds it.

---

### Exposed Log Files

Log files record everything that happens on a system. They often contain
usernames, session tokens, error messages with internal system paths, and
sometimes full credentials if an application accidentally logs them.
```
filetype:log intext:"password"
filetype:log intext:"username" intext:"password"
filetype:log intext:"error" inurl:logs
filetype:log "PHP Parse error"
filetype:log "MySQL Error"
filetype:log intext:"api_key"
filetype:log intext:"authorization"
filetype:log intext:"Bearer"
intitle:"index of" "access.log"
intitle:"index of" "error.log"
intitle:"index of" ext:log
```

**The Bearer token issue:** HTTP Bearer tokens are used for authentication
in modern web applications and APIs. If an application logs its HTTP requests
(which many do for debugging) and those logs end up publicly accessible, every
Bearer token in those logs is a valid credential for whoever finds it.

---

### Exposed Backup Files

Backups are copies of databases, application code, or configuration files
saved for recovery purposes. When they end up in a public web directory they
are one of the richest sources of sensitive data available.
```
filetype:bak inurl:config
filetype:bak inurl:database
filetype:sql site:targetdomain.com
ext:dump intext:"INSERT INTO"
ext:gz inurl:backup
ext:zip inurl:backup site:targetdomain.com
ext:tar.gz inurl:backup
intitle:"index of" "database.sql"
intitle:"index of" "backup.zip"
intitle:"index of" ext:bak
```

---

## Part 2 — Network Devices and Physical Systems

This is where dorking connects to the physical world. Cameras, routers,
industrial control systems, building management systems — all of them have
web interfaces. Many of those interfaces are publicly accessible.

---

### Exposed Cameras

Security cameras with web interfaces that are accessible from the internet
and indexed by Google are far more common than most people realize.
```
intitle:"webcam" inurl:view
intitle:"Live View / - AXIS"
intitle:"Network Camera" inurl:axis-cgi
inurl:"/view/viewer_index.shtml"
inurl:CgiStart?page=Single
inurl:view/index.shtml
intitle:"WJ-NT304" inurl:LvAppl
intitle:"Toshiba Network Camera" inurl:live
intitle:"Sony Network Camera" inurl:SNC
intitle:"Panasonic Network Camera" inurl:config
intitle:"HIKVISION" inurl:doc/page/login.asp
intitle:"Dahua" inurl:"/RPC2_Login"
intitle:"ip camera" inurl:"/cgi-bin/viewer"
```

**Important context:** Finding a camera feed is not the same as hacking it.
If a camera is configured to stream publicly with no authentication — which
many are, either intentionally or by mistake — the feed is publicly accessible.
This is a configuration failure by the owner, not a breach by the viewer.
However, accessing private feeds or using camera access to facilitate harm is
illegal.

---

### Exposed Network Devices

Routers, switches, firewalls, and VPNs all have web interfaces. These should
never be accessible from the public internet.
```
intitle:"Router" inurl:setup.cgi
intitle:"ADSL Configuration" inurl:userRpm
intitle:"Cisco" inurl:"/exec/show"
intitle:"DD-WRT" inurl:"/Info.htm"
intitle:"pfSense" inurl:"/index.php"
intitle:"MikroTik RouterOS" inurl:winbox
intitle:"Ubiquiti" inurl:"/login.cgi"
intitle:"Netgear" inurl:"/setup.cgi"
intitle:"Linksys" inurl:"/setup.cgi"
inurl:":8291" intitle:"WinBox"
```

---

### Industrial Control Systems (ICS/SCADA)

This section exists for awareness, not exploitation. Industrial Control Systems
manage physical infrastructure — power grids, water treatment, manufacturing
lines, oil pipelines. They should never be on the public internet. Some are.
```
intitle:"SCADA" inurl:login
intitle:"Modbus" inurl:config
intitle:"Wonderware" inurl:login
intitle:"Ignition Gateway" inurl:web/home
intitle:"InTouch" inurl:config
inurl:"/cgi-bin/main.cgi" intitle:"ICS"
intitle:"Siemens" inurl:"/Portal/Portal.mwsl"
```

> ⚠️ **Critical warning:** Accessing ICS/SCADA systems without authorization
> is a federal crime in most countries and can carry severe penalties. These
> dorks are documented here so security professionals and infrastructure owners
> can audit their own exposure. Never use them against systems you do not own
> or have explicit written permission to test.

---

## Part 3 — Finding People and Personal Information (OSINT)

This is the section that separates OSINT from regular search. Finding
information about people is one of the most common investigative tasks in
security research, law enforcement, journalism, and background investigation.

Done ethically and legally, OSINT person-finding is a powerful professional
skill. Done irresponsibly, it enables stalking, harassment, and harm. The
line is intent and use. Know which side you are on before you start.

---

### The OSINT Methodology for Finding People

Before running a single search, professionals build a collection strategy.
Random searching produces random results. Structured searching produces
intelligence.

**Step 1 — Seed data.** What do you already know? Even one piece of
information — a name, an email address, a username, a phone number, a city —
is enough to start.

**Step 2 — Expand.** Use that seed to find more data points. Each new data
point becomes a new seed.

**Step 3 — Correlate.** Look for information that appears across multiple
sources. If a username appears on three different platforms and each platform
has slightly different profile information, the intersection of all three gives
you a more complete picture than any one source alone.

**Step 4 — Verify.** OSINT produces leads, not facts. Verify before you act
on anything.

---

### Email Address OSINT

An email address is one of the richest seeds in OSINT. It connects to
accounts, registration records, breach databases, and more.
```
"firstname.lastname@company.com"
"@targetcompany.com" filetype:pdf
"@targetcompany.com" filetype:xlsx
"@targetcompany.com" site:linkedin.com
"@targetcompany.com" intext:"contact"
"email" "@targetdomain.com" filetype:csv
```

**Free tools to run alongside Google:**

| Tool | What It Does | URL |
|---|---|---|
| **Hunter.io** | Finds email addresses associated with a domain, shows format patterns | hunter.io |
| **Phonebook.cz** | Email, domain, and URL OSINT search | phonebook.cz |
| **EmailRep** | Reputation and breach history for an email | emailrep.io |
| **Have I Been Pwned** | Check if an email appears in known data breaches | haveibeenpwned.com |
| **Holehe** | Checks which websites an email is registered on (run locally) | github.com/megadose/holehe |
| **GHunt** | Deep Google account OSINT from a Gmail address (run locally) | github.com/mxrch/GHunt |
| **Epieos** | Google and Apple account lookup from email | epieos.com |

**The Hunter.io pattern trick:** Hunter.io shows you the email format a company
uses — `firstname.lastname@company.com` or `f.lastname@company.com` or
`firstname@company.com`. Once you know the format, you can construct email
addresses for any employee whose name you know from LinkedIn. This is a
technique actively used in phishing campaigns and security audits.

**GHunt — the deep Google OSINT tool:** If you have a Gmail address, GHunt can
pull the associated Google account's public name, profile photo, Google Maps
reviews, YouTube activity, and connected services. This level of detail from
a single email address surprises even experienced security professionals.

---

### Username OSINT — Cross-Platform Tracking

People reuse usernames. This is one of the most reliable facts in OSINT.
Someone who uses `shadowhunter42` on Reddit probably uses it on Twitter,
Discord, GitHub, and a dozen other platforms.
```
"username" site:reddit.com
"username" site:twitter.com
"username" site:github.com
"username" site:instagram.com
"username" -site:facebook.com -site:twitter.com
```

**Free tools:**

| Tool | What It Does | URL |
|---|---|---|
| **Sherlock** | Searches 300+ platforms for a username simultaneously (run locally) | github.com/sherlock-project/sherlock |
| **WhatsMyName** | Web-based username search across hundreds of sites | whatsmyname.app |
| **Namechk** | Username availability checker (reveals where name is taken) | namechk.com |
| **Instant Username Search** | Fast cross-platform check | instantusername.com |
| **Maigret** | Advanced username OSINT, pulls profile data not just existence | github.com/soxoj/maigret |

**Maigret vs Sherlock:** Sherlock tells you *where* a username exists. Maigret
goes further — it pulls profile data, photos, bios, and linked accounts from
each platform it finds. For serious OSINT investigations, Maigret is the
upgrade.

---

### Phone Number OSINT

Phone numbers are underrated OSINT seeds. They connect to carrier records,
spam databases, social profiles, and reverse lookup databases.
```
"phone number" site:targetdomain.com
"+1 555" "targetname" site:linkedin.com
"contact" "phone" site:targetdomain.com filetype:pdf
```

**Free tools:**

| Tool | What It Does | URL |
|---|---|---|
| **Truecaller** | Crowdsourced caller ID, often reveals name and location | truecaller.com |
| **NumVerify** | Carrier, line type, country verification (free tier) | numverify.com |
| **PhoneInfoga** | Advanced phone number OSINT framework (run locally) | github.com/sundowndev/phoneinfoga |
| **SpyDialer** | Reverse phone lookup, sometimes reveals voicemail | spydialer.com |
| **Sync.me** | Social media reverse phone lookup | sync.me |

**PhoneInfoga** is the professional tool here. It combines carrier lookup,
reputation databases, Google dorking, and social media search into a single
workflow for a phone number. Running it locally gives you results that paid
commercial tools charge hundreds of dollars for.

---

### IP Address and Domain OSINT

IP addresses and domains reveal infrastructure, hosting history, associated
domains, and sometimes the people behind them.
```
"X.X.X.X" site:targetdomain.com
intext:"IP" intext:"X.X.X.X" filetype:log
"targetdomain.com" intext:"whois"
```

**Free tools:**

| Tool | What It Does | URL |
|---|---|---|
| **Shodan** | Search engine for internet-connected devices (see Shodan section) | shodan.io |
| **Censys** | Similar to Shodan, excellent for certificate and infrastructure analysis | search.censys.io |
| **VirusTotal** | Passive DNS, related domains, reputation for IPs and domains | virustotal.com |
| **SecurityTrails** | DNS history, subdomain enumeration, hosting history | securitytrails.com |
| **Robtex** | IP and domain relationships, reverse DNS, ASN data | robtex.com |
| **DNSDumpster** | Free DNS recon, maps domain infrastructure visually | dnsdumpster.com |
| **ViewDNS** | Reverse IP, whois history, DNS propagation checker | viewdns.info |
| **BuiltWith** | Technology stack fingerprinting for any domain | builtwith.com |
| **Wappalyzer** | Browser extension, identifies tech stack live | wappalyzer.com |

**The SecurityTrails DNS history trick:** When someone tries to hide a website
behind Cloudflare or a VPN service, their real IP is often exposed in DNS
history — records of what IP the domain pointed to *before* they added the
protection. SecurityTrails keeps years of DNS history. This is how security
researchers regularly bypass Cloudflare anonymization to find the real server.

---

### Document and Metadata OSINT

Every document created on a computer carries hidden data called **metadata**.
Metadata is information about the file — who created it, what software they
used, when it was created, sometimes the computer name and the user's real
name.
```
site:targetdomain.com filetype:pdf
site:targetdomain.com filetype:docx
site:targetdomain.com filetype:xlsx
site:targetdomain.com filetype:pptx
"company name" filetype:pdf "author"
```

**Free tools:**

| Tool | What It Does | URL |
|---|---|---|
| **ExifTool** | Extracts full metadata from any file type (run locally) | exiftool.org |
| **FOCA** | Automated document metadata extraction and analysis | github.com/ElevenPaths/FOCA |
| **Metagoofil** | Google dorks for documents then extracts metadata automatically | github.com/laramies/metagoofil |
| **Jeffrey's Exif Viewer** | Web-based EXIF data viewer for images | exifdata.com |

**The Metagoofil workflow:** Metagoofil automates what would take hours
manually. You give it a domain, it uses Google to find all publicly indexed
documents on that domain, downloads them, extracts metadata from each one,
and gives you a report. From a single company domain you can often extract:
internal usernames, real names of employees, software versions used internally,
internal server names and paths, and document creation timestamps.

This data feeds directly into social engineering preparation, phishing campaign
construction, and Active Directory enumeration if you later get internal access.

---

## Part 4 — Finding Leaked Credentials and Breach Data

This section is for security professionals auditing their own organization's
exposure and researchers studying breach patterns. Finding your own
organization's credentials in public breach data is a legitimate and important
security practice. Finding other people's credentials and using them is a crime.

---

### Google Dorks for Leaked Credentials
```
site:pastebin.com "targetdomain.com" "password"
site:pastebin.com "targetdomain.com" "@"
site:paste.ee "targetdomain.com"
site:ghostbin.com "targetdomain.com"
site:controlc.com "targetdomain.com"
filetype:sql intext:"targetdomain.com" intext:"password"
filetype:csv intext:"targetdomain.com" intext:"password"
"targetdomain.com" intext:"password" filetype:txt
```

**Pastebin and paste sites** are where data breach dumps frequently appear
first. When a database is breached, the data is often posted to paste sites
as proof before being sold or distributed further. Google indexes many of
these pastes.

---

### Free Breach Intelligence Tools

| Tool | What It Does | URL |
|---|---|---|
| **Have I Been Pwned** | Email and domain breach checking | haveibeenpwned.com |
| **DeHashed** | Full breach database search, supports email/username/IP/name | dehashed.com |
| **LeakCheck** | Breach lookup with partial password visibility | leakcheck.io |
| **IntelligenceX** | Search engine for leaked data, dark web, and Tor | intelx.io |
| **Breach Directory** | Fast email breach check with password hash preview | breachdirectory.org |
| **PWNDB** | Onion service for breach database search (requires Tor) | pwndb2am4tzkvold.onion |

**Intelligence X (intelx.io)** deserves special mention. It is one of the
few tools that indexes dark web content, leaked databases, Telegram channels,
and historical web archives in a single searchable interface. The free tier
is limited but gives you a real taste of what professional threat intelligence
platforms provide.

---

## Part 5 — Advanced Techniques Professionals Miss

These are techniques that do not appear in most OSINT guides. They require
more thought and creativity than running a standard dork, but they produce
results that standard searches miss entirely.

---

### The Wayback Machine + Google Combination

The Wayback Machine at `web.archive.org` stores snapshots of websites going
back to 1996. Google's cache stores recent versions. Combining both lets you
find content that was briefly public, then removed — but is still permanently
archived.

**Workflow:**
1. Find a page with Google that returns a 404 (not found) or has been changed
2. Run it through `web.archive.org` to see historical versions
3. Check historical versions for data that was present before removal

Exposed documents, old employee directories, old login portals, deprecated
API endpoints — all of them live in the Wayback Machine long after the live
site removes them.

---

### Certificate Transparency Logs for Subdomain Discovery

Every HTTPS certificate issued by a certificate authority is recorded in
public **certificate transparency logs**. These logs exist to prevent
fraudulent certificates — but they also reveal every subdomain a company
has ever created a certificate for.

**Free tools:**

| Tool | What It Does | URL |
|---|---|---|
| **crt.sh** | Searches certificate transparency logs | crt.sh |
| **Certspotter** | Certificate monitoring and history | sslmate.com/certspotter |

**Search syntax on crt.sh:**
```
%.targetdomain.com
```

This returns every subdomain that has ever had a certificate issued for it.
Development environments, staging servers, internal tools, deprecated services
— if it ever had HTTPS, it appears here. Then you dork those subdomains:
```
site:staging.targetdomain.com
site:dev.targetdomain.com
site:internal.targetdomain.com
```

---

### Google Dorks for Job Postings as Intelligence

This technique is used by professional threat intelligence analysts and almost
nobody else talks about it. **Job postings reveal infrastructure.**

When a company posts a job listing for a "Senior AWS Engineer familiar with
our EKS clusters running Terraform," they have just told you their cloud
provider, their container orchestration platform, and their infrastructure
as code tool. When they post for a "Database Administrator experienced with
Oracle 19c and PostgreSQL," they have told you their database stack.
```
site:linkedin.com/jobs "targetcompany" "AWS" "Terraform"
site:indeed.com "targetcompany" "database administrator"
site:greenhouse.io "targetcompany" "security engineer"
site:lever.co "targetcompany" "infrastructure"
"targetcompany" site:jobs.lever.co OR site:greenhouse.io OR site:workday.com
```

This intelligence is used to:
- Build a technology stack profile without touching any systems
- Identify which security tools they use (and therefore what gaps might exist)
- Understand internal team structure
- Identify technology that may have known vulnerabilities

---

### Cached Google Translate Pages

This is a technique known to almost no one outside of advanced OSINT circles.
Google Translate acts as a proxy — when you run a URL through translate.google.com,
Google fetches the page server-side and serves it to you through Google's
infrastructure. This can sometimes bypass IP-based access controls and
geographic restrictions.
```
https://translate.google.com/translate?sl=auto&tl=en&u=TARGET_URL
```

Use cases:
- Accessing pages that block your geographic region
- Viewing a cached version of a page through Google's infrastructure
- Accessing pages that have since gone offline if Google has a cached version

---

### Error Messages as Intelligence

Error messages are accidentally detailed. Developers leave debugging enabled.
Applications crash and show their configuration. Servers reveal their software
versions. All of this gets indexed.
```
intext:"Warning: mysql_connect()" intext:"on line" filetype:php
intext:"Fatal error" intext:"PHP" inurl:".php"
intext:"mysqli_connect()" intext:"failed"
intext:"ORA-00933" intext:"SQL command"
intext:"Microsoft OLE DB Provider for SQL Server"
intext:"ODBC SQL Server Driver"
intext:"JDBC Driver" intext:"Connection refused"
intext:"Django" "DEBUG = True" intext:"Traceback"
intext:"Laravel" intext:"APP_DEBUG=true"
intext:"Stack trace:" intext:"at System."
intext:"Traceback (most recent call last)"
```

**What these reveal:**
- Database type and version
- Internal file paths (reveals server structure)
- Programming language and framework versions
- Configuration settings
- Sometimes credentials if they appear in connection strings in error messages

---

### Finding Forgotten and Shadow IT

Shadow IT refers to technology that employees or teams set up without
official IT approval or awareness. It is one of the biggest security risks
in large organizations and one of the hardest to find through internal audits.
Google finds it.
```
site:*.targetdomain.com -site:www.targetdomain.com
site:*.targetdomain.com intitle:"login"
site:*.targetdomain.com inurl:admin
"targetcompany" site:notion.so
"targetcompany" site:confluence.atlassian.net
"targetcompany" site:trello.com
"targetcompany" site:airtable.com
"targetcompany" site:docs.google.com
"targetcompany" site:sharepoint.com
```

**The Notion/Confluence/Trello problem:** Teams regularly create Notion pages,
Confluence wikis, Trello boards, and Google Docs with company information and
set them to public — sometimes intentionally for external sharing, sometimes
by accident. These pages are indexed by Google. They frequently contain internal
processes, system architecture diagrams, vendor lists, employee directories,
and sometimes credentials.

---

## Part 6 — Building a Professional OSINT Workflow

Running individual dorks is useful. Having a systematic workflow is how
professionals produce reliable intelligence instead of lucky finds.

---

### The Professional OSINT Stack (Free)

| Layer | Tool | Purpose |
|---|---|---|
| **Search** | Google (with operators) | Primary indexed web intelligence |
| **Search** | Bing, Yandex | Different indexing, different results |
| **People** | Sherlock / Maigret | Username cross-platform tracking |
| **Email** | Hunter.io / Holehe / GHunt | Email intelligence |
| **Breach** | HIBP / IntelligenceX | Credential exposure checking |
| **Domain** | SecurityTrails / DNSDumpster / crt.sh | Infrastructure mapping |
| **Devices** | Shodan / Censys | Exposed device discovery |
| **Documents** | Metagoofil / ExifTool | Metadata extraction |
| **Archives** | Wayback Machine | Historical content recovery |
| **Social** | Maltego CE (free tier) | Visual relationship mapping |
| **Aggregation** | Spiderfoot (run locally) | Automated multi-source OSINT |

**Spiderfoot** is the tool that ties everything together. It is an open source
OSINT automation framework that takes a seed (email, domain, IP, name) and
automatically queries dozens of data sources simultaneously — DNS records,
breach databases, social media, certificate logs, Shodan, VirusTotal, and more.
The output is a visual graph of everything it found and how it connects.

Running Spiderfoot against your own organization's domain before an attacker
does is one of the most valuable proactive security exercises available for free.

---

### OSINT Note-Taking — Obsidian for Intelligence

Professional OSINT investigators do not rely on memory or scattered browser
tabs. They use structured note-taking. **Obsidian** (free) with the Canvas
plugin lets you build visual intelligence maps — linking people to usernames
to email addresses to domains to companies — that mirror what expensive
commercial threat intelligence platforms provide.

The workflow:
1. Create a note for each entity (person, domain, IP, organization)
2. Link notes as you find connections
3. Use Canvas to build a visual relationship map
4. Tag notes by confidence level (confirmed, probable, unverified)

This turns a dorking session from a pile of browser tabs into a structured
intelligence report.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*