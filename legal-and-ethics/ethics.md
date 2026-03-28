# Legal and Ethics — The Real Conversation

> This is not only a disclaimer, to cover my own butt. This is an actual
> conversation about what is legal, what is not, where the lines are, why
> they exist, and how professionals who use these techniques every day think
> about them. If you have read this entire guide you have learned powerful
> techniques. This section is about what you do with that power.

---

## The Line That Actually Matters

Most people think the legal line in security research is complicated.
It is not. It is actually very simple:

**Did you have permission?**

That is the line. Everything else — the technique, the tool, the
information found, the intent — is secondary to that one question.

- Scanning a network you own: legal
- Scanning a network you have written permission to test: legal
- Scanning a network you do not own without permission: illegal in
  most jurisdictions regardless of what you find or what you intended

- Reading publicly available information through a search engine: legal
- Accessing a system using credentials you found through dorking: illegal
- Downloading data from an exposed database you do not own: illegal
- Reporting an exposed database to its owner: legal and ethical

The techniques in this guide are passive. Reading information that a
search engine has indexed from publicly accessible sources is not
illegal. What you do with that information — and whether you cross
from observation into interaction — determines where you stand legally.

---

## Part 1 — What Is Actually Legal

### Passive Reconnaissance

Passive reconnaissance means gathering information without directly
interacting with the target's systems. Everything in this guide up to
the point of using found credentials or accessing found systems is
passive reconnaissance.

**Legal passive activities:**
- Running Google, Bing, and Yandex dorks against any target
- Reading publicly indexed documents, pages, and files
- Searching WHOIS, certificate transparency logs, and DNS records
- Looking up IP addresses and ASN ownership
- Searching breach databases for email addresses
- Using Shodan to view indexed banner data
- Reading publicly accessible social media profiles
- Reverse image searching publicly available photos
- Reading publicly accessible court records and property records
- Using people search sites that aggregate public record data

None of these activities involve touching any system. They involve
reading information that is already publicly accessible and has been
indexed by third-party services. This is the same activity that
journalists, private investigators, law enforcement, and researchers
do every day.

---

### Authorized Security Testing

Security testing conducted with explicit written authorization from
the system owner is legal regardless of the techniques used.

**What authorization looks like:**
- A signed penetration testing agreement
- A bug bounty program's scope document
- Written authorization from your employer for systems you are
  responsible for
- A statement of work from a client engagement

**What authorization does NOT look like:**
- Verbal permission
- An assumption that the owner would not mind
- The belief that the system should be public anyway
- The fact that the system was easy to find

Written authorization is the difference between a penetration tester
and a criminal using the exact same tools and techniques.

---

### Bug Bounty Programs

Bug bounty programs are formal programs run by companies that invite
security researchers to find vulnerabilities in their systems in
exchange for rewards. They are legal by definition — the company has
explicitly authorized research within defined scope.

**Major bug bounty platforms:**
- HackerOne (hackerone.com)
- Bugcrowd (bugcrowd.com)
- Intigriti (intigriti.com)
- Synack (synack.com)
- Open Bug Bounty (openbugbounty.org)

**Critical rule:** Read the scope document carefully. Every bug bounty
program defines what is in scope and what is out of scope. Testing
out-of-scope assets — even if you find a critical vulnerability —
is a violation of the program rules and potentially illegal.

---

## Part 2 — What Is Not Legal

### The Computer Fraud and Abuse Act (US)

The Computer Fraud and Abuse Act (CFAA) is the primary US federal law
governing computer crime. It is broad, it is aggressively enforced,
and it has been applied in ways that surprised defendants who believed
their actions were harmless.

**What the CFAA prohibits:**
- Accessing a computer without authorization
- Exceeding authorized access to a computer
- Accessing a computer to obtain information, cause damage, or commit fraud
- Trafficking in passwords or access credentials

**The "without authorization" problem:**
The CFAA does not require that you cause damage. Accessing a system
without authorization — even if you do nothing harmful, even if the
system was misconfigured, even if you immediately reported the issue —
can constitute a federal crime under the CFAA.

This has been used to prosecute people who:
- Accessed systems that were publicly accessible but not intended to be
- Used someone else's credentials they found publicly exposed
- Scraped data from websites in ways the site owner did not intend
- Accessed systems they believed they had permission to access but did not

**Penalties:** Up to 10 years federal prison for a first offense on
many CFAA violations. Up to 20 years for violations involving national
security or critical infrastructure.

---

### Computer Misuse Act (UK)

The UK equivalent of the CFAA. Prohibits:
- Unauthorized access to computer material
- Unauthorized access with intent to commit further offenses
- Unauthorized modification of computer material
- Making, supplying, or obtaining articles for use in computer misuse offenses

The last point is significant — creating tools designed for unauthorized
access can itself be an offense in the UK regardless of whether they are
used.

---

### GDPR and Privacy Laws (EU and Global)

The General Data Protection Regulation (GDPR) governs how personal data
about EU citizens can be collected, processed, and stored — regardless
of where the collector is located.

**OSINT and GDPR:**
Collecting personal data about EU citizens through OSINT techniques —
even from publicly available sources — can create GDPR obligations.
Storing, processing, or publishing that data without a lawful basis
can constitute a GDPR violation.

For professional investigators and security researchers operating in or
researching targets in the EU, understanding GDPR obligations is
essential. The Information Commissioner's Office (ICO) in the UK and
equivalent data protection authorities in EU countries have guidance
on lawful OSINT practices.

---

### Stalking and Harassment Laws

The techniques in this guide can locate people. Locating people without
their knowledge and using that information to monitor, follow, contact,
or harm them is stalking under the laws of virtually every jurisdiction.

Stalking laws apply regardless of:
- Whether the information was publicly available
- Whether the investigator believes they have a good reason
- Whether the target is a public figure
- Whether digital rather than physical means are used

Cyberstalking is specifically addressed in US federal law (18 U.S.C.
§ 2261A) and in equivalent laws in most countries. Penalties include
significant prison time.

---

### Vigilantism

Using OSINT techniques to investigate, identify, and publicly expose
individuals — even individuals suspected of crimes — outside of law
enforcement or legal processes is vigilantism. It is dangerous, often
illegal, and consistently produces harm.

**Why vigilante OSINT causes harm:**
- OSINT produces leads, not certainties. Vigilante actions based on
  incorrect identification have resulted in innocent people being
  harassed, threatened, and physically attacked.
- Publishing personal information about individuals without legal
  authority — doxxing — is illegal in many jurisdictions and causes
  real harm regardless of the target's guilt.
- Vigilante investigations can compromise actual law enforcement
  investigations by alerting suspects, contaminating evidence, and
  creating legal complications.

If your OSINT investigation reveals evidence of serious crime, the
correct action is to report it to appropriate law enforcement —
not to act on it yourself.

---

## Part 3 — The Passive vs Active Line

This is the most important practical distinction in this guide.

**Passive reconnaissance:**
You are asking a third party (Google, Shodan, a people search site)
what it knows about a target. You are not touching the target's systems.
The target has no way to know you searched for them. This is legal.

**Active reconnaissance:**
You are directly interacting with the target's systems — sending packets,
making requests, probing ports, attempting logins. The target's systems
receive your traffic. This requires authorization.

**The gray areas:**

*Viewing a public webpage:* Passive. Your browser makes a request but
so does every visitor to any public website. Accessing a public page
is not unauthorized access.

*Viewing an accidentally public page:* Generally passive. If a page
is accessible without authentication and indexed by Google, viewing it
is analogous to reading a document someone left on a park bench.
However, downloading or using the data found there may create liability
depending on jurisdiction and circumstances.

*Using found credentials:* Active. Even if credentials were found
through completely passive means, using them to access a system is
unauthorized access under the CFAA and equivalent laws.

*Accessing an exposed database:* Active. Even if the database has no
authentication and is publicly accessible, connecting to it and
reading data constitutes unauthorized access in most jurisdictions.

*Running automated scanners against a target:* Active. Tools like
Nmap, Nikto, and Burp Suite send traffic directly to target systems.
This requires authorization.

---

## Part 4 — How Law Enforcement Uses These Techniques

The same techniques documented in this guide are used by law enforcement
agencies globally for legitimate investigative purposes. Understanding
this context matters for two reasons: it legitimizes the techniques
for professional use, and it illustrates that investigators are not
operating in a space that law enforcement cannot see.

**Documented law enforcement OSINT uses:**
- Social media monitoring for criminal activity and missing persons
- Open source intelligence for counterterrorism investigations
- Dark web monitoring for drug trafficking and cybercrime
- Geolocation of photos posted by suspects
- Username correlation to identify suspects across platforms
- Reverse image search to identify suspects and victims
- Blockchain analysis to trace cryptocurrency in criminal investigations
- Certificate transparency log analysis to map criminal infrastructure

The FBI, Europol, Interpol, and national law enforcement agencies
globally have dedicated OSINT units. The techniques are the same.
The authorization is different.

**Law enforcement OSINT training resources (public):**
- SANS SEC487: Open-Source Intelligence Gathering and Analysis
- Bellingcat guides (bellingcat.com/resources)
- EUROPOL OSINT guidelines
- ACPO (Association of Chief Police Officers) digital investigation guidelines

---

## Part 5 — Responsible Disclosure

If your dorking and OSINT research reveals a security vulnerability
in a system you do not own and were not authorized to test, responsible
disclosure is the ethical and often legally protective path.

**What responsible disclosure means:**
Reporting a vulnerability directly to the affected organization before
making it public, giving them time to fix it, and working with them
on the timeline for any public disclosure.

**How to report:**
1. Find the organization's security contact —
   `security@organization.com` or their `SECURITY.md` file on GitHub
   or their HackerOne/Bugcrowd profile
2. Report the vulnerability clearly — what you found, where, how
   you found it, what the potential impact is
3. Do not access the system beyond what was necessary to confirm
   the vulnerability exists
4. Do not publish or share the vulnerability before the organization
   has had reasonable time to respond and remediate
5. Document everything — your discovery process, your report,
   their response

**CISA Coordinated Vulnerability Disclosure:**
In the US, CISA (Cybersecurity and Infrastructure Security Agency)
provides a framework for coordinated vulnerability disclosure and
can serve as an intermediary when direct contact with an organization
is not possible or productive.

**CVD resources:**
- CISA: cisa.gov/coordinated-vulnerability-disclosure-process
- FIRST: first.org/cvss (vulnerability scoring)
- Disclose.io: disclose.io (safe harbor framework)

---

## Part 6 — The Ethics Beyond the Law

Legality and ethics are not the same thing. Something can be legal
and still cause harm. Something can be technically illegal and still
be the right thing to do. Professional investigators think about both.

**Questions worth asking before any investigation:**

*Why am I doing this?*
Curiosity is not a sufficient reason to investigate a real person.
Professional need, safety concern, or authorized research are.

*Who could be harmed by this investigation?*
Not just the target — their family members, associates, and others
whose information may surface in the investigation.

*What will I do with what I find?*
If you cannot answer this before you start, you should not start.

*Is this proportionate?*
The depth of investigation should match the seriousness of the need.
Comprehensive personal OSINT on a neighbor because they annoyed you
is not proportionate.

*Am I the right person to be doing this?*
Some investigations should be conducted by law enforcement, licensed
private investigators, or trained professionals — not by individuals
acting alone.

---

## Part 7 — A Note on This Guide

This guide documents techniques that are used by security professionals,
journalists, law enforcement, investigators, and researchers every day.
The same techniques are used by stalkers, harassers, and malicious
actors. The techniques themselves are neutral. The intent and application
are not.

Publishing this guide openly is a deliberate choice based on a belief
that security through obscurity does not work. These techniques are
already documented in law enforcement training materials, security
certifications, and professional OSINT courses. Making them accessible
to people who want to understand them — and to people who want to
protect themselves against them — is more valuable than pretending
they do not exist.

The people most harmed by ignorance of these techniques are the people
who do not know what is publicly available about them and therefore
cannot take steps to protect themselves. Understanding what OSINT can
find is the first step in controlling your own digital footprint.

Use this guide to learn. Use it to protect. Use it professionally and
with authorization. Do not use it to harm people.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*