# 🔍 google-dorking

> You have been using 1% of what search engines can actually do.
> This is the other 99%. A complete Google dorking and search engine
> intelligence reference for OSINT, security research, investigation,
> and professional reconnaissance — built for everyone from complete
> beginners to seasoned professionals.
>
> Part of the **SudoCode Pentesting Methodology Guide** — by SudoChef.

![Documentation](https://img.shields.io/badge/Documentation-Complete-C9667A?style=flat-square)
![OSINT](https://img.shields.io/badge/OSINT-Reference-9B8EC4?style=flat-square)
![Free Tools](https://img.shields.io/badge/Free_Tools-Included-7ABFB0?style=flat-square)
![Docs Only](https://img.shields.io/badge/Docs_Only-No_Code-2B2D3A?style=flat-square)

---

## 🧭 What Is This?

Most people type a few words into Google and scroll the first page.
That is not searching. That is guessing.

**Dorking** is the practice of using advanced search operators —
built-in commands that every major search engine supports — to filter
billions of pages with surgical precision. The same techniques used
by security researchers, law enforcement analysts, journalists, and
OSINT investigators are available to anyone who knows the syntax.

This guide covers everything. What dorking is and why it works. Every
operator across Google, Bing, and Yandex. Real working examples.
Cybersecurity and OSINT applications. Social media intelligence.
GitHub credential hunting. Shodan. The dark web. Free research tools.
How to pivot from what you find into a full investigation. Legal and
ethical boundaries. And a one-page cheatsheet to bookmark.

No gatekeeping. No basic fluff. Built for real use.

---

## 📁 Repository Structure
```
google-dorking/
├── what-is-dorking/
│   └── intro.md
├── google/
│   ├── operators.md
│   ├── cybersecurity-dorks.md
│   ├── social-media-dorks.md
│   └── advanced-combinations.md
├── bing/
│   └── bing.md
├── yandex/
│   └── yandex.md
├── other-search-engines/
│   └── alternatives.md
├── free-research-sites/
│   └── sites.md
├── github-dorking/
│   └── github.md
├── shodan-dorking/
│   └── shodan.md
├── tor-and-dark-web/
│   └── dark-web-search.md
├── what-next/
│   └── pivoting.md
├── dork-cheatsheet/
│   └── cheatsheet.md
└── legal-and-ethics/
    └── ethics.md
```

---

## 🗺️ Navigation

### 🟣 Foundation

| File | What It Covers |
|---|---|
| [What Is Dorking](./what-is-dorking/intro.md) | Hacking → web hacking → Google hacking → dorking. History, how search engines index everything, why most people use 1% of what is available. Start here. |

---

### 🔴 Google

| File | What It Covers |
|---|---|
| [Operators](./google/operators.md) | Every Google operator with syntax, real examples, what works in 2026, common mistakes and fixes, and the dead operators to stop wasting time on. |
| [Cybersecurity Dorks](./google/cybersecurity-dorks.md) | Finding exposed systems, login panels, databases, config files, credentials, log files, and backups. Full OSINT methodology. Tools professionals actually use. Techniques most guides never cover. |
| [Social Media Dorks](./google/social-media-dorks.md) | Reverse image search, photo metadata, geolocation from background details, license plate intelligence, Facebook, Instagram, LinkedIn, Twitter/X, Twitch, DevTools, cross-platform identity correlation. |
| [Advanced Combinations](./google/advanced-combinations.md) | Chaining operators into real-world recipes. Full target recon workflow. CTF techniques. Penetration testing recon methodology. Automation tools. Building your own dork library. |

---

### 🟡 Search Engines

| File | What It Covers |
|---|---|
| [Bing](./bing/bing.md) | What Bing indexes differently, the `ip:` operator (no Google equivalent), `inbody:`, `loc:`, `language:`, Bing cache, image OSINT, when to use Bing over Google. |
| [Yandex](./yandex/yandex.md) | The most underestimated OSINT engine. Superior facial recognition, Eastern European content, VK and Russian social platform intelligence, unique operators, Yandex Maps for geospatial intelligence. Deep dive. |
| [Alternative Engines](./other-search-engines/alternatives.md) | DuckDuckGo bang operators, Brave Search Goggles, Baidu for China-connected targets, Startpage anonymous proxy, SearXNG aggregation, MetaGer for European targets, complete decision guide. |

---

### 🟢 Research Tools

| File | What It Covers |
|---|---|
| [Free Research Sites](./free-research-sites/sites.md) | The full investigator toolkit. People search (FastPeopleSearch, TruePeopleSearch, ThatsThem, Radaris), reverse phone lookup, email investigation, username tracking, business and corporate records, property records, dark web exposure checking, image and face search. All free. All real. |

---

### 🔵 Advanced Surfaces

| File | What It Covers |
|---|---|
| [GitHub Dorking](./github-dorking/github.md) | Why GitHub is the most dangerous public search surface. Exposed credentials, private keys, API keys, commit history attacks, organization intelligence, deleted content that is not gone. Google dorks and GitHub native search. Trufflehog and Gitleaks. The full security audit checklist. |
| [Shodan](./shodan-dorking/shodan.md) | The search engine for everything connected to the internet. Free vs paid tiers explained. Exposed cameras, databases, industrial systems, medical devices. What bad actors use it for. What defenders use it for. Core filters and search recipes. |
| [Tor and Dark Web](./tor-and-dark-web/dark-web-search.md) | What the dark web actually is. OPSEC before you open Tor Browser. Dark web search engines. How to actually investigate — forum intelligence, username correlation, Bitcoin tracking, PGP key analysis, stylometric analysis. Ransomware leak monitoring without Tor. |

---

### ⚪ Reference

| File | What It Covers |
|---|---|
| [Pivoting](./what-next/pivoting.md) | Once you have information — what to do with it. Full pivot workflows from every finding type. Maltego, Spiderfoot, theHarvester, Recon-ng. Common issues and fixes. When to stop. |
| [Cheatsheet](./dork-cheatsheet/cheatsheet.md) | One page. Every operator. Top dork recipes. People search quick reference. Search engine stack. Pivot reference. Bookmark this. |
| [Legal and Ethics](./legal-and-ethics/ethics.md) | What is legal, what is not, the passive vs active line, CFAA, GDPR, stalking laws, responsible disclosure, how law enforcement uses these same techniques, the ethics beyond the law. |

---

## ⚡ Quick Start

New to dorking? Start here in order:
```
1. what-is-dorking/intro.md       — understand what you are doing and why
2. google/operators.md            — learn the syntax
3. google/cybersecurity-dorks.md  — see it applied to real security research
4. dork-cheatsheet/cheatsheet.md  — bookmark this and keep it open
```

Already know the basics and want to go deeper?
```
→ google/advanced-combinations.md     — real world recipes and recon methodology
→ github-dorking/github.md            — the most dangerous surface most people ignore
→ yandex/yandex.md                    — the engine most professionals underuse
→ free-research-sites/sites.md        — the full people intelligence toolkit
→ what-next/pivoting.md               — turning findings into intelligence
```

---

## 🔑 The Golden Rules
```
No space after the colon      site:example.com     ✅
                              site: example.com    ❌

Quotes for exact phrases      "internal use only"  ✅
                              internal use only    ❌

Combine with a space          site:example.com filetype:pdf "confidential"

Exclude with minus            site:example.com -inurl:login

Either/or with OR             filetype:pdf OR filetype:docx
```

---

## 🛠️ What You Will Need

Everything in this guide is free. No paid tools required to get started.

| Tool | Purpose | Cost |
|---|---|---|
| A browser | Running all dorks | Free |
| Tor Browser | Dark web investigation | Free |
| Shodan account | Device and infrastructure search | Free tier available |
| Maltego CE | Visual relationship mapping | Free community edition |
| Spiderfoot | Automated OSINT aggregation | Free and open source |
| theHarvester | Email and subdomain harvesting | Free and open source |
| Maigret | Username cross-platform tracking | Free and open source |
| ExifTool | Photo metadata extraction | Free and open source |
| Trufflehog | GitHub secret scanning | Free and open source |

---

## ⚠️ Legal and Ethical Use

This guide documents passive reconnaissance techniques using publicly
available information. Everything here is legal when used as documented —
reading information that search engines have already indexed from publicly
accessible sources.

**The line:** Passive OSINT is legal. Using found credentials, accessing
exposed systems, or interacting with infrastructure you do not own
without authorization is not.

Read the [Legal and Ethics](./legal-and-ethics/ethics.md) section in full.
It is not a disclaimer — it is a real conversation about where the lines
are and why they exist.

---

## 📬 Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to submit corrections
and additions. See [SECURITY.md](./SECURITY.md) for the security policy.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*