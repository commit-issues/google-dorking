# Tor and Dark Web Search — Investigation and Intelligence

> The dark web has the worst PR problem in technology. Half the internet
> thinks it is an exclusively criminal underworld. The other half thinks
> it is a mythical realm accessible only to elite hackers. Both are wrong.
> This section cuts through the noise — tight explanation of what it
> actually is, how to access it safely, and most importantly how to
> actually use it as an investigative and intelligence tool. That last
> part is what most guides skip entirely. We are not skipping it.

---

## Part 1 — What It Actually Is (The Short Version)

### Surface Web vs Deep Web vs Dark Web

These three terms get confused constantly. They mean completely different
things.

**Surface Web:** Everything indexed by search engines. Google, news sites,
social media, this repository. Roughly 4-5% of all internet content.

**Deep Web:** Everything not indexed by search engines but accessible
through a regular browser with authentication — your email, your bank
account, Netflix, hospital records, corporate intranets. The vast majority
of the internet. Not dangerous. Not hidden. Just not publicly indexed.

**Dark Web:** A specific overlay network accessible only through special
software — primarily Tor. Uses `.onion` addresses. Not indexed by regular
search engines. Small fraction of total internet content.

The deep web is enormous and mostly mundane. The dark web is a small
specific network that requires Tor. Conflating them is the source of most
dark web misinformation.

---

### How Tor Works — Plain English

When you visit a website normally your browser connects directly to that
server. Your IP address is visible. Your ISP can see where you are going.

Tor routes your traffic through three volunteer-operated servers called
relays. Your traffic is encrypted in layers — like an onion. Each relay
peels one layer and passes it forward. No single relay knows both where
the traffic came from and where it is going.

- **Entry node:** Knows your IP, not the destination
- **Middle node:** Knows neither
- **Exit node:** Knows the destination, not your IP

The result — neither the destination nor any relay can identify both who
you are and what you are accessing simultaneously.

**Who uses Tor legitimately:** 
- Journalists
- Whistleblowers
- Activists in authoritarian countries
- Security researchers
- Law enforcement conducting undercover investigations
- Domestic violence survivors, 
- Military 
- Intelligence agencies (Tor was originally built by the US Naval Research Laboratory), 
- Privacy-conscious individuals

---

## Part 2 — Accessing Tor Safely — OPSEC First

### The Tor Browser

Download only from the official source:
```
https://www.torproject.org/download/
```

Never download from any other source. Malicious Tor Browser versions
are a documented deanonymization vector.

**Immediately after installing — set Security Level to Safest:**
Click the shield icon → Advanced Security Settings → Safest.
This disables JavaScript. Some sites break. That is acceptable.

---

### OPSEC Checklist Before Every Session
```
[ ] Dedicated device preferred — not your daily driver
[ ] Not on employer or institutional network — they log everything
[ ] Do NOT log into any personal accounts through Tor — ever
[ ] Do NOT resize the browser window — window size fingerprints you
[ ] Do NOT open downloaded documents while online
[ ] Do NOT torrent over Tor — bypasses Tor entirely, exposes real IP
[ ] Serious investigations → use Tails OS, not just Tor Browser
```

### Tails OS — The Professional Standard

**URL:** tails.boum.org

Boots from USB on any computer. Routes all traffic through Tor. Leaves
zero trace on the host machine when shut down. Used by journalists,
security researchers, and law enforcement for maximum anonymity. If the
investigation matters, use Tails.

---

### What Tor Does NOT Protect Against

Tor is strong. It is not unbreakable. Documented deanonymization vectors:

- **Traffic correlation attacks** — monitoring entry and exit nodes
  simultaneously to correlate timing
- **Malicious exit nodes** — exit nodes controlled by adversaries who
  inspect unencrypted traffic
- **JavaScript exploits** — why Security Level Safest matters
- **Operational security mistakes** — logging into personal accounts,
  reusing usernames, revealing personal details in posts
- **Law enforcement infiltration** — documented in multiple major
  dark web takedowns

The FBI, Europol, and allied agencies have successfully identified and
arrested significant dark web operators. Tor protects against casual
surveillance. It does not protect against well-resourced adversaries
with the ability to monitor significant portions of the network.

---

## Part 3 — What Actually Exists on the Dark Web

### Legitimate Content

- **SecureDrop:** Whistleblower submission system used by the New York
  Times, Washington Post, Guardian, and dozens of other news organizations.
  Sources submit documents through `.onion` addresses without revealing
  identity. One of the most important legitimate uses of the dark web.

- **News organization mirrors:** ProPublica, BBC, Deutsche Welle maintain
  onion mirrors for readers in countries where those sites are blocked or
  monitored.

- **Facebook's official onion site:**
  `facebookwkhpilnemxj7asber7cybohpyxzqd7sbv5lqh5zl7d5z6czyd.onion`
  For users in countries where Facebook is blocked or surveilled.

- **Privacy tools and activist resources:** Secure communication guides,
  resources for people living under authoritarian surveillance.

- **Security research forums:** Researchers discussing vulnerabilities,
  malware behavior, and threat actor activity.

### Criminal Content That Actually Exists

- Drug markets (successors to Silk Road — continuously disrupted and replaced)
- Stolen credential markets and carding forums
- Malware and exploit markets
- Ransomware-as-a-Service operations
- **Ransomware leak sites** — where groups publish stolen data from
  organizations that refused to pay (significant threat intel source)
- Counterfeit document services

### What Is Largely Myth

- "Hitman for hire" services — virtually all documented examples are scams
- Live violence streams — largely fabricated for sensationalism
- Secret government archives freely browsable
- The ability to "hack anything" from dark web resources

The criminal content is real and significant. The mythological content
is fabricated. Knowing the difference makes you a better investigator.

---

## Part 4 — Dark Web Search Engines and Navigation

The dark web has no Google. It is largely un-indexed. Navigation depends
on a combination of search engines, link directories, and community
knowledge. Here is the complete working toolkit.

---

### Ahmia — Start Here

**Surface web URL:** `ahmia.fi`
**Onion URL:** `http://juhanurmihxlp77nkq76byazcldy2hlmovfu2epvl5ankdibsot4csyd.onion`

Ahmia is the most legitimate dark web search engine. Created by a Finnish
security researcher, it explicitly filters child sexual abuse material
from its index and is the recommended entry point for all legitimate
research.

**The key advantage of Ahmia:** The surface web version works without
Tor Browser. You can search onion site content from a regular browser
to determine whether a Tor session is worth opening. Only open Tor
Browser when you have confirmed a target site is worth accessing directly.

**Working search workflow on Ahmia:**
```
# Search for a specific topic
ahmia.fi/search/?q=ransomware+leak

# Search for a specific organization's appearance
ahmia.fi/search/?q="company name"

# Search for credential markets
ahmia.fi/search/?q=credential+database

# Search for specific threat actor groups
ahmia.fi/search/?q="lockbit" OR "alphv" OR "blackcat"
```

**What Ahmia returns:**
- Onion site URL
- Site title and description
- Last indexed date
- Category tags

Use the last indexed date to gauge whether a site is still active.
Dark web sites disappear constantly — a site indexed six months ago
may no longer exist.

---

### Torch

**Onion URL:** `http://xmh57jrknzkhv6y3ls3ubitzfqnkrwxhopf5aygthi7d6rplyvk3noyd.onion`

One of the oldest dark web search engines. Larger and noisier index
than Ahmia — less filtered, more results, more irrelevant content.
Requires Tor Browser. Use as a secondary search after Ahmia when
Ahmia does not return sufficient results.

---

### DuckDuckGo Onion — Surface Web Through Tor

**Onion URL:** `https://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion`

Searches the **surface web** through Tor — not dark web onion sites.
Use this when you want to research surface web content during a dark
web investigation session without leaving the Tor network. Keeps
your entire investigation within Tor.

---

### Haystak

**Onion URL:** `http://haystak5njsmn2hqkewecpaxetahtwhsbsa64jom2k22z5afxhnpxfid.onion`

Claims one of the larger onion indexes. Free tier available with limited
results. Use as a third search engine when Ahmia and Torch have not
produced sufficient results on a specific topic.

---

### Intelligence X — Dark Web Without Tor

**URL:** `intelx.io`

This is the bridge tool. Intelligence X indexes dark web content,
leaked databases, Telegram channels, paste sites, and historical
web archives — all searchable from a regular browser without Tor.

**What IntelX lets you do without opening Tor:**
- Search for an email, domain, or name across dark web indexed content
- Find whether a target appears in dark web breach dumps
- Search Telegram channels that discuss a target
- Access historical snapshots of dark web content

The free tier is limited but real. For most initial investigations
IntelX surfaces enough to determine whether a full Tor-based
investigation is warranted.
```
# IntelX search workflow
1. Go to intelx.io
2. Enter search term — email, domain, username, company name
3. Review results across all indexed sources
4. Note which dark web sources mention the target
5. Use those specific sources as targets for Tor Browser follow-up
```

---

### Ransomware Leak Site Monitoring Without Tor

Ransomware groups operate data leak sites on the dark web to pressure
victims. Security professionals monitor these for early breach warning
and threat intelligence. These surface web services aggregate the
data so you do not need Tor access:

| Service | What It Does | URL |
|---|---|---|
| **Ransomware.live** | Tracks active groups, victims, and leak posts | ransomware.live |
| **ID Ransomware** | Identifies ransomware from files or ransom notes | id-ransomware.malwarehunterteam.com |
| **Darkfeed** | Threat intelligence feed from dark web (paid) | darkfeed.io |
| **DarkOwl** | Dark web monitoring platform (paid) | darkowl.com |
| **Breach Forums Monitor** | Tracks major breach forum activity | breachforums.st (surface) |

**The Ransomware.live workflow for defenders:**
```
1. Go to ransomware.live
2. Search your organization name and domain
3. Search client names and domains
4. Set up monitoring for new posts
5. If a hit appears — incident response immediately, do not wait
   for official notification
```

This technique has given organizations days of advance warning before
public breach disclosure — time that is invaluable for response preparation.

---

## Part 5 — How to Actually Investigate on the Dark Web

This is the section most guides skip. Not this one.

---

### The Dark Web Investigation Framework

Dark web investigations require a structured approach. Random browsing
is ineffective and risky. Professional investigators follow a framework:

**Phase 1 — Surface first.**
Before opening Tor Browser, exhaust surface web resources:
- IntelX for dark web content without Tor
- Ahmia surface web interface for onion site discovery
- Ransomware.live and similar aggregators
- Google dorks targeting paste sites and dark web references

**Phase 2 — Define your objective.**
What specifically are you looking for?
- Evidence a specific organization was breached
- Threat actor infrastructure and identity
- Stolen data from a specific breach
- Threat actor communications about a specific target
- Malware samples or exploit code

Without a defined objective, dark web investigations produce noise.

**Phase 3 — Tails or Tor Browser with full OPSEC.**
Once surface web resources are exhausted and a specific target exists,
open a properly configured Tor session.

**Phase 4 — Document everything.**
Dark web sites disappear. Screenshot everything relevant. Note the
onion URL, the date, and the content. If evidence may be used legally,
follow proper evidence preservation procedures.

**Phase 5 — Cross-reference surface web.**
Every username, handle, Bitcoin address, PGP key, or writing sample
found on the dark web becomes a new OSINT seed for surface web
correlation.

---

### Forum Intelligence — How to Actually Work Dark Web Forums

Dark web forums are where threat actors communicate, advertise, and
recruit. Extracting intelligence from them is a specific skill.

**Passive observation only.**
Never register on dark web forums for investigative purposes unless
you are law enforcement with proper legal authority. Registration
creates an account trail. In some jurisdictions, registering on
criminal forums can itself create legal exposure.

**What to look for without registering:**
Many dark web forums have public-facing sections visible without an
account. These often contain:
- Actor announcements and press releases
- Tool and service advertisements
- Recruitment posts
- Victim announcements from ransomware groups

**The intelligence extraction workflow:**
```
1. Find the forum through Ahmia or Torch search
2. Navigate to public sections only
3. Screenshot all relevant content before proceeding
4. Identify usernames, handles, and aliases in posts
5. Note posting times — timezone intelligence
6. Note writing style — language patterns, vocabulary, errors
7. Extract any Bitcoin addresses, PGP key IDs, or contact information
8. Take those identifiers to surface web correlation tools
```

**Posting time timezone analysis:**
Most forum posts include timestamps. Analyzing when a specific actor
posts — clustering activity in morning hours UTC+3 for example — gives
geographic intelligence. Combined with language patterns this narrows
attribution significantly.

---

### Username and Identity Correlation — Dark Web to Surface Web

This is the technique that has identified more dark web actors than
almost any other. People reuse usernames. Dark web actors are not
immune to this failure.

**The workflow:**
```
# Step 1 — Find username on dark web forum through Ahmia
ahmia.fi/search/?q="targetusername"

# Step 2 — Run username through surface web tools
whatsmyname.app → check 600+ platforms
maigret username → deep profile pull

# Step 3 — Search Google
site:reddit.com "targetusername"
site:github.com "targetusername"
site:twitter.com "targetusername"
"targetusername" forum

# Step 4 — Check for username on other dark web forums
ahmia.fi/search/?q="targetusername" -site:knownforum.onion

# Step 5 — Check paste sites
site:pastebin.com "targetusername"
site:rentry.co "targetusername"
```

Documented cases where dark web actors were identified through username
reuse include major ransomware operators, malware authors, and dark web
market administrators. This technique works because people are creatures
of habit even when they know better.

---

### Bitcoin Address Intelligence

Most dark web transactions use cryptocurrency. Bitcoin is pseudonymous
not anonymous — every transaction is permanently recorded on a public
blockchain. Following Bitcoin addresses from known dark web sources
reveals transaction networks and sometimes connects to exchange accounts
with identity verification.

**The Bitcoin tracking workflow:**
```
# Step 1 — Find Bitcoin address in dark web content
# (market listings, ransom notes, forum posts)

# Step 2 — Look up the address on blockchain explorer
blockchain.com/explorer/addresses/ADDRESS
blockchair.com/bitcoin/address/ADDRESS

# Step 3 — Trace transaction history
# Who sent to this address?
# Who did this address send to?
# Are there high-value clustering transactions?

# Step 4 — Check known address databases
# OXT.me for Bitcoin-specific graph visualization
oxt.me/address/ADDRESS

# Step 5 — Check if address appears in threat intelligence
# VirusTotal, abuse.ch, and crypto threat intel feeds
virustotal.com/gui/search/ADDRESS
```

**Blockchain analysis tools — free:**

| Tool | Strength | URL |
|---|---|---|
| **OXT** | Bitcoin-specific graph visualization | oxt.me |
| **Blockchair** | Multi-chain explorer, advanced search | blockchair.com |
| **Breadcrumbs** | Visual transaction mapping | breadcrumbs.app |
| **Blockchain Explorer** | Simple and reliable | blockchain.com/explorer |
| **Walletexplorer** | Identifies exchange wallets | walletexplorer.com |

**The exchange identification technique:**
When a Bitcoin address sends funds to a known exchange wallet
(Coinbase, Binance, Kraken), that exchange has KYC (Know Your Customer)
records for the account owner. Law enforcement with proper legal authority
can subpoena those records. As an investigator, identifying that dark
web funds flowed to a specific exchange is significant — it gives law
enforcement a clear legal path to identity.

---

### PGP Key Intelligence

Dark web actors sign communications with PGP keys to prove authenticity.
PGP public keys are sometimes uploaded to public key servers — creating
an intelligence opportunity.

**The PGP investigation workflow:**
```
# Step 1 — Find PGP key ID or fingerprint in dark web content
# These appear in forum signatures, market listings, announcements

# Step 2 — Search public key servers
keys.openpgp.org/search?q=KEYID_OR_EMAIL
pgp.mit.edu/pks/lookup?search=KEYID

# Step 3 — Examine key metadata
# Key creation date — when did this actor start?
# Email address used to create the key (sometimes real)
# Name field (sometimes real, often pseudonym)
# Other keys that have signed this key (trust network)

# Step 4 — Search the email or name from key metadata
# Run through all surface web OSINT tools
# Sometimes real email addresses are used and searchable
```

Key creation dates establish actor timeline. Cross-signing between keys
reveals trust relationships and actor networks. Email addresses embedded
in keys have directly identified actors in documented cases.

---

### Stylometric Analysis — Writing Style as Intelligence

Every person has a unique writing style. Vocabulary choices, sentence
structure, punctuation habits, spelling patterns, and characteristic
errors are as identifying as a fingerprint when analyzed systematically.

**What to capture for stylometric analysis:**
- Characteristic phrases and expressions
- Spelling errors — consistent misspellings are highly identifying
- Punctuation patterns — does the actor use Oxford commas? Em dashes?
- Capitalization habits — do they capitalize certain words unusually?
- Sentence length patterns
- Language — native speaker or non-native? Which language patterns?
- Technical vocabulary — what domain knowledge is displayed?

**The correlation workflow:**
```
1. Collect multiple writing samples from the dark web actor
2. Identify consistent stylometric markers
3. Build a search query around unusual phrases or patterns
4. Search surface web for those exact patterns

"characteristic phrase" site:reddit.com
"characteristic phrase" site:github.com
"unusual term combination" forum
```

This technique has identified malware authors through Stack Overflow
posts, ransomware operators through old forum posts, and dark web market
administrators through surface web writing from years before they went dark.

---

### Operational Security Mistake Hunting

Threat actors make mistakes. The most common and most exploitable:

**The persona bleed:**
An actor uses a dark web handle, then uses the same handle somewhere
years earlier on a surface web forum. Search the handle everywhere.

**The accidental metadata:**
A threat actor posts a document or image. The metadata was not stripped.
ExifTool reveals the author name, the software version, the creation
timestamp, and sometimes the computer name. All of these are intelligence.

**The timezone slip:**
An actor claiming to be in one country posts exclusively during business
hours in another timezone. Consistent posting time clustering reveals
real timezone regardless of claimed location.

**The language error:**
A threat actor communicates in English but makes consistent errors
characteristic of a specific native language. Russian speakers have
characteristic English errors. Chinese speakers have different ones.
These patterns narrow geographic attribution significantly.

**The infrastructure reuse:**
Command and control servers, Bitcoin wallets, and PGP keys used across
multiple operations. When two separate attacks share infrastructure,
they share an actor.

---

### Monitoring Without Active Investigation

For security professionals who need ongoing dark web visibility without
conducting active investigations, passive monitoring tools provide
continuous intelligence:

**Free monitoring:**
```
# Set up Google alerts for your organization on surface web
google.com/alerts → "your organization name" breach

# Monitor Ransomware.live for your organization
ransomware.live → search your domain weekly

# Check IntelX periodically
intelx.io → search your domain, email formats, executive names

# Monitor paste sites through Google
site:pastebin.com "yourdomain.com"
site:rentry.co "yourdomain.com"
```

**Paid monitoring (worth knowing for professional context):**
- **Flare** — automated dark web monitoring with alerting
- **Recorded Future** — comprehensive threat intelligence platform
- **DarkOwl** — dark web specific monitoring
- **Cybersixgill** — dark web intelligence feeds

---

## Part 6 — The Limits of Dark Web Investigation

**Most significant criminal activity is invite-only.**
The public-facing dark web represents a fraction of actual criminal
activity. The most significant markets, forums, and communities are
accessible only through vouching from existing members. These are
unreachable through standard investigation techniques.

**Content disappears constantly.**
Dark web sites go offline without warning. Law enforcement takedowns,
exit scams, and simple abandonment mean links from last week may be
dead today. Screenshot and document immediately.

**Misinformation is prevalent.**
Threat actors lie. Claims about victims, capabilities, and data are
frequently exaggerated or fabricated for reputation building. Treat
all dark web intelligence as unverified until cross-referenced against
independent sources.

**Legal boundaries are real.**
In many jurisdictions, accessing certain dark web content — even for
investigative purposes — can create legal exposure. Know the laws
in your jurisdiction before conducting dark web investigations.
When in doubt, law enforcement referral is the correct action.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*