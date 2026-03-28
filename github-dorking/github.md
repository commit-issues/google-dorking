# GitHub Dorking — The Most Dangerous Search Engine You Are Not Using

> Let me be direct with you. GitHub is a landmine. Not because it is
> poorly built — it is actually an incredibly powerful platform. It is a
> landmine because developers treat it like a private workspace when it is,
> by default, a public one. Every day, credentials, API keys, passwords,
> private certificates, internal infrastructure maps, and proprietary source
> code get pushed to public GitHub repositories by people who did not stop
> to think about what they were actually making public.
>
> I know this firsthand. I have spent serious time hardening my own GitHub
> presence — SSH commit signing, branch rulesets, no-reply emails, scrubbed
> commit history, Dependabot, secret scanning, CODEOWNERS, private
> vulnerability reporting. I did all of that because I understand exactly
> what an unprotected GitHub exposes. This section is going to show you
> the attack side of that picture in full detail.
>
> This section covers OSINT and beyond — GitHub dorking crosses the line
> from passive intelligence gathering into active attack surface discovery.
> Everything here is documented for defensive awareness and authorized
> security research. Finding your own organization's exposure before an
> attacker does is one of the highest-value security exercises available.
> Using these techniques against systems you do not own or have explicit
> written permission to test is illegal.
>
> A dedicated repository covering how to properly secure your GitHub
> presence — commit signing, branch protection, secret scanning, history
> scrubbing, and full security posture hardening — is coming as part of
> this series. This section is the problem. That repository will be the
> solution.

---

## Why GitHub Is the Most Dangerous Public Search Surface

Every other search engine in this guide indexes content that was put on
the web — websites, documents, pages. GitHub is different. GitHub indexes
content that developers put there while thinking about something else
entirely. They were thinking about their code. They were not thinking about
the API key three lines above it.

Here is what makes GitHub uniquely dangerous as an exposed surface:

**Commit history is permanent and searchable.**
When a developer commits a file containing a password, then realizes the
mistake and deletes it in the next commit, most people assume the password
is gone. It is not. It is in the commit history forever — and that history
is publicly searchable. GitHub's search indexes commit history. So does
Google. Deleting a file in a new commit does not remove it from the past.

**Developers work fast and make mistakes.**
The pressure to ship code quickly means developers frequently commit
things they should not. A `.env` file accidentally included. A hardcoded
API key for "just testing." A config file with real database credentials
pushed because the developer forgot to add it to `.gitignore`. These
mistakes happen hundreds of times per day across GitHub.

**One exposed credential can compromise an entire organization.**
A single AWS access key committed to a public repository gives an attacker
full access to that organization's cloud infrastructure — potentially
every server, every database, every stored file. This has happened to
Fortune 500 companies. It has happened to government agencies. It happens
constantly to startups who move fast and do not think about security posture.

**Gists are forgotten.**
GitHub Gists are small code snippets that developers share — little
pastes of code for quick reference. Developers frequently paste config
files, scripts with embedded credentials, and sensitive code into public
Gists without thinking twice. Gists are indexed by Google. They are
searchable. They are almost never reviewed for security.

**Issues and pull requests contain intelligence.**
The discussion threads on GitHub repositories reveal internal
architecture decisions, vendor relationships, internal tool names,
employee usernames, and sometimes credentials shared carelessly in
comments. All of it is public and indexed.

**Organizations reveal their entire technology stack.**
A company's GitHub organization page lists every public repository.
Each repository reveals what technologies they use, how their code is
structured, who their developers are, what third-party services they
integrate with, and sometimes what their internal infrastructure looks
like. This is technology fingerprinting without touching a single system.

---

## Part 1 — Google Dorking for GitHub

Before using GitHub's own search, Google dorking against GitHub surfaces
content that GitHub's own search sometimes misses — particularly older
indexed content and content from repositories that have since been made
private or deleted but remain in Google's cache.

---

### Finding Credentials and Secrets
```
# Environment files with credentials
site:github.com filetype:env "DB_PASSWORD"
site:github.com filetype:env "AWS_SECRET_ACCESS_KEY"
site:github.com filetype:env "API_KEY"
site:github.com filetype:env "SECRET_KEY"
site:github.com filetype:env "STRIPE_SECRET"
site:github.com filetype:env "TWILIO_AUTH_TOKEN"
site:github.com filetype:env "SENDGRID_API_KEY"

# Configuration files with passwords
site:github.com filetype:yml "password:" intext:production
site:github.com filetype:yaml "password:" intext:database
site:github.com filetype:json "api_key" "secret"
site:github.com filetype:xml "password" "username"
site:github.com filetype:properties "password="
site:github.com filetype:ini "password="
site:github.com filetype:conf "password="

# Private keys and certificates
site:github.com "BEGIN RSA PRIVATE KEY"
site:github.com "BEGIN DSA PRIVATE KEY"
site:github.com "BEGIN EC PRIVATE KEY"
site:github.com "BEGIN OPENSSH PRIVATE KEY"
site:github.com filetype:pem "PRIVATE KEY"
site:github.com filetype:ppk intext:"private"

# Cloud provider credentials
site:github.com "aws_access_key_id"
site:github.com "aws_secret_access_key"
site:github.com intext:"AKIA" filetype:env
site:github.com "azure_client_secret"
site:github.com "google_application_credentials"
site:github.com filetype:json "type: service_account"

# Database connection strings
site:github.com intext:"mongodb+srv://" filetype:env
site:github.com intext:"postgres://" filetype:env
site:github.com intext:"mysql://" filetype:env
site:github.com "jdbc:mysql" intext:"password"
site:github.com "connection_string" intext:"password"

# Payment and financial API keys
site:github.com "sk_live_" intext:stripe
site:github.com "pk_live_" intext:stripe
site:github.com "access_token" intext:paypal filetype:env
site:github.com "braintree" "private_key" filetype:env

# Communication service credentials
site:github.com "TWILIO" "AUTH_TOKEN" filetype:env
site:github.com "SENDGRID" "API_KEY" filetype:env
site:github.com "MAILGUN_API_KEY" filetype:env
site:github.com "SLACK_TOKEN" filetype:env
site:github.com "DISCORD_TOKEN" filetype:env
site:github.com "TELEGRAM_BOT_TOKEN" filetype:env
```

---

### Finding Internal Infrastructure Details
```
# Internal hostnames and server names
site:github.com "internal.company.com" filetype:yml
site:github.com "staging.company.com" filetype:env
site:github.com "dev.company.com" filetype:conf

# Exposed network configuration
site:github.com filetype:yml "host:" "port:" "password:"
site:github.com filetype:xml "connectionString" "password"
site:github.com "vpn" filetype:conf intext:"password"

# Exposed SSH configuration
site:github.com filetype:cfg "ssh" "password"
site:github.com "id_rsa" site:github.com/gist

# Internal IP addresses
site:github.com intext:"192.168." filetype:conf
site:github.com intext:"10.0." filetype:yml
site:github.com intext:"172.16." filetype:conf

# Docker and container secrets
site:github.com filetype:yml "docker" "password:"
site:github.com "docker-compose" filetype:yml "password:"
site:github.com filetype:env "DOCKER_PASSWORD"

# Kubernetes secrets
site:github.com filetype:yml "kind: Secret"
site:github.com "kubectl" "secret" filetype:sh
```

---

### Finding Sensitive Code and Logic
```
# Hardcoded credentials in source code
site:github.com "password=" filetype:py
site:github.com "password=" filetype:php
site:github.com "password=" filetype:js
site:github.com "api_key=" filetype:py
site:github.com "secret_key=" filetype:rb

# Admin and backdoor code
site:github.com "admin password" filetype:php
site:github.com "backdoor" filetype:php
site:github.com "eval(base64_decode" filetype:php

# Vulnerable code patterns
site:github.com "mysql_query" filetype:php intext:"$_GET"
site:github.com "exec(" filetype:py intext:"request"
site:github.com "system(" filetype:php intext:"$_POST"
```

---

### Finding Target-Specific Repositories
```
# Find all public repos for a company
site:github.com/companyname
site:github.com "companyname" filetype:env
site:github.com "companyname.com" "password"

# Find internal tools made public accidentally
site:github.com "companyname" "internal" filetype:md
site:github.com "companyname" "do not share" OR "confidential"
site:github.com "companyname" "proprietary"

# Find employee personal repos with company code
site:github.com "companyname.com" intext:"api_key"
site:github.com "@companyname.com" filetype:env
```

---

## Part 2 — GitHub Native Search

GitHub has its own search engine that is separate from Google. It searches
the live current state of repositories — including content that Google has
not yet indexed. For finding fresh exposures, GitHub's native search is
more current than Google dorking.

**Access GitHub search:**
```
https://github.com/search
```

GitHub's search supports its own operator syntax. Learning it gives you
access to the most current indexed state of every public repository,
gist, issue, pull request, commit, and user on the platform.

---

### GitHub Search Operators

| Operator | What It Does | Example |
|---|---|---|
| `in:file` | Search within file contents | `password in:file` |
| `in:path` | Search within file path | `config in:path` |
| `in:readme` | Search within README files | `api_key in:readme` |
| `filename:` | Search by exact filename | `filename:.env` |
| `extension:` | Search by file extension | `extension:pem` |
| `language:` | Filter by programming language | `language:python` |
| `org:` | Search within an organization | `org:targetcompany` |
| `user:` | Search within a user's repos | `user:targetusername` |
| `repo:` | Search within a specific repo | `repo:owner/reponame` |
| `path:` | Search within a specific path | `path:config/` |
| `size:` | Filter by file size | `size:>1000` |
| `fork:` | Include or exclude forks | `fork:false` |
| `is:public` | Public repos only | `password is:public` |
| `pushed:` | Filter by last push date | `pushed:>2025-01-01` |
| `created:` | Filter by creation date | `created:>2024-01-01` |

---

### GitHub Native Search Recipes
```
# Find exposed .env files across all public repos
filename:.env DB_PASSWORD
filename:.env AWS_SECRET
filename:.env "API_KEY"

# Find private keys
filename:id_rsa
filename:id_dsa
extension:pem "PRIVATE KEY"
extension:ppk "private"

# Find exposed configuration files
filename:config.yml password
filename:database.yml password
filename:settings.py SECRET_KEY
filename:wp-config.php DB_PASSWORD
filename:.htpasswd
filename:shadow
filename:passwd

# Find credentials in specific languages
language:python "password ="
language:javascript "api_key ="
language:php "mysql_connect"
language:ruby "password ="

# Find secrets within a specific organization
org:targetcompany password
org:targetcompany api_key
org:targetcompany "internal use only"
org:targetcompany filename:.env

# Find recently pushed sensitive files
filename:.env pushed:>2026-01-01
filename:config.yml password pushed:>2026-01-01

# Find exposed WordPress configurations
filename:wp-config.php
filename:wp-config.php DB_PASSWORD

# Find exposed SSH configurations
filename:sshd_config
filename:authorized_keys
filename:known_hosts

# Find exposed database files
extension:sql "INSERT INTO" "password"
extension:sql "CREATE TABLE users"
extension:db
extension:sqlite

# Find CI/CD pipeline secrets
filename:.travis.yml password
filename:.circleci/config.yml password
filename:Jenkinsfile password
filename:.github/workflows secret
```

---

### Searching GitHub Issues and Pull Requests

Issues and pull requests are discussion threads — and people say things
in discussion threads that they would never put in code. Credentials shared
in comments, internal architecture debates, references to internal systems,
and sensitive decisions made in public.
```
# Search issues for credential mentions
is:issue "password" org:targetcompany
is:issue "api_key" org:targetcompany
is:issue "credentials" org:targetcompany

# Search pull requests for sensitive content
is:pr "password" org:targetcompany
is:pr "secret" org:targetcompany
is:pr "internal" org:targetcompany

# Find issues mentioning specific systems
is:issue "database" "password" org:targetcompany
is:issue "production" "credentials" org:targetcompany
```

---

### Searching GitHub Gists

Gists are GitHub's pastebin equivalent — small code snippets shared
for quick reference. They are almost never reviewed for security and
are frequently the source of exposed credentials.
```
# Google dork for gists
site:gist.github.com "password"
site:gist.github.com "api_key"
site:gist.github.com "AWS_SECRET"
site:gist.github.com "BEGIN RSA PRIVATE KEY"
site:gist.github.com "targetcompany"
site:gist.github.com "@targetcompany.com"

# GitHub native gist search
# Navigate to: gist.github.com/search?q=SEARCHTERM
```

**The gist problem in detail:**
A developer needs to share a script with a colleague. They paste it into
a Gist. The script has a database connection string hardcoded — they were
going to clean that up before sharing but forgot. The Gist is public by
default. Google indexes it within hours. That connection string is now
searchable by anyone who knows the right query.

This exact scenario happens constantly. Gist searches are one of the
highest-yield credential hunting techniques in bug bounty programs.

---

## Part 3 — The Commit History Attack

This is the attack vector that surprises people most. Even when developers
fix a mistake by removing a credential in a new commit, the old commit
containing the credential remains in the repository history permanently —
and it is searchable.

---

### How Commit History Exposure Works

**Plain English explanation:**

Think of Git commit history like a Google Doc with version history turned
on. Every version of every file you have ever saved is stored. If you typed
your password into a document, then deleted it, then shared the document —
anyone with access can click "See version history" and find the version where
the password was there.

GitHub's public repositories work exactly the same way. Every commit is a
saved version. The history is public. The history is searchable.

**The three ways credentials end up in commit history:**

1. **Direct commit:** Developer commits a file with credentials, realizes
   the mistake, removes the file in the next commit. The file with credentials
   is in commit N. The removal is in commit N+1. Anyone viewing the repo
   history can see commit N.

2. **Revert without history rewrite:** Developer reverts a bad commit using
   `git revert` instead of `git rebase` or `git filter-branch`. `git revert`
   adds a new commit that undoes the changes — but the original bad commit
   remains in history.

3. **Force push myth:** Many developers believe that force pushing over a
   bad commit removes it. It does not remove it from GitHub immediately.
   GitHub caches commits. And if Google indexed the commit before the force
   push, it remains in Google's cache even after the force push.

---

### Finding Credentials in Commit History
```
# Google dork for specific commit content
site:github.com "password" inurl:commit
site:github.com "api_key" inurl:commit
site:github.com "AWS_SECRET" inurl:commit/

# Finding deleted files that remain in history
site:github.com inurl:"/blob/" ".env"
site:github.com inurl:"/commit/" "password"
site:github.com inurl:"/commit/" "api_key"

# GitHub native search for commit content
# GitHub search: hash:COMMITHASH
# Or browse: github.com/owner/repo/commits/main
```

**The manual commit history method:**

When you have identified a target repository, browse its commit history
directly:
```
https://github.com/owner/repository/commits/main
```

Look for commits with messages like:
- "remove credentials"
- "fix password"
- "oops"
- "cleanup"
- "remove sensitive"
- "delete env file"
- "remove api key"

These commit messages are developers documenting their own mistakes.
Click the commit *before* the cleanup commit to see the version of the
file that contained the credential.

---

### Tools for Automated Commit History Analysis

Manual commit history browsing works for small investigations. For
comprehensive credential hunting across entire organizations, these
tools automate the process:

| Tool | What It Does | URL |
|---|---|---|
| **Trufflehog** | Scans git history for high-entropy strings (likely secrets) | github.com/trufflesecurity/trufflehog |
| **Gitleaks** | Scans repos and commit history for secrets using regex patterns | github.com/gitleaks/gitleaks |
| **GitAllSecrets** | Combines multiple tools for comprehensive secret scanning | github.com/anshumanbh/git-all-secrets |
| **Gitrob** | Maps organization repos and flags sensitive files | github.com/michenriksen/gitrob |
| **Repo-supervisor** | Scans for secrets in repository files | github.com/auth0/repo-supervisor |

**Trufflehog in practice:**

Trufflehog is the most widely used tool for this. It identifies secrets
by looking for high-entropy strings — sequences of characters that are
statistically random enough to likely be cryptographic keys or passwords
rather than regular text.
```bash
# Install
pip3 install trufflehog

# Scan a public repository including full history
trufflehog git https://github.com/owner/repository

# Scan an organization's repos
trufflehog github --org=targetorganization

# Scan with verified secrets only (reduces false positives)
trufflehog git https://github.com/owner/repo --only-verified

# Output to JSON for further analysis
trufflehog git https://github.com/owner/repo --json
```

**Gitleaks for pattern-based scanning:**
```bash
# Install
brew install gitleaks

# Scan a repo
gitleaks detect --source /path/to/repo

# Scan including git history
gitleaks detect --source /path/to/repo --log-opts="--all"

# Scan a remote repo directly
gitleaks detect --source https://github.com/owner/repo
```

---

## Part 4 — GitHub Organization Intelligence

When a company has a GitHub organization, that organization is a goldmine
of intelligence about the company's technology, people, and internal
infrastructure.

---

### What a GitHub Organization Reveals

**Repository list:**
Every public repository reveals a technology decision. React frontend means
JavaScript. Terraform repos mean infrastructure-as-code on a cloud provider.
A Kubernetes configuration repo maps their container orchestration setup.
A repository named `internal-tools` that was accidentally made public is
exactly what it sounds like.

**Contributor list:**
Every contributor to every public repository is a named developer who works
or worked at that organization. Their GitHub profiles link to their personal
repositories, their social media, and their other projects. This is employee
enumeration without touching a single internal system.

**Dependency files:**
`package.json`, `requirements.txt`, `Gemfile`, `pom.xml` — dependency files
reveal every third-party library and service the application uses. Known
vulnerabilities in those dependencies become known attack vectors.

**CI/CD configuration:**
`.github/workflows/`, `.travis.yml`, `Jenkinsfile` — CI/CD configuration
files reveal deployment processes, testing infrastructure, environment
variable names (even when values are hidden), and sometimes credentials
that were hardcoded during debugging.

---

### Mapping a GitHub Organization
```
# Find the organization page
github.com/targetorganization

# Google dorks for organization intelligence
site:github.com/targetorganization
site:github.com/targetorganization filetype:env
site:github.com/targetorganization "password"
site:github.com/targetorganization "internal"
site:github.com/targetorganization "do not share"

# GitHub native search within org
org:targetorganization filename:.env
org:targetorganization password
org:targetorganization api_key
org:targetorganization "internal use only"
org:targetorganization filename:docker-compose.yml password

# Finding employees through org contributions
# Navigate to: github.com/orgs/targetorganization/people
# (if public membership is enabled)
```

---

### Finding Personal Repos With Company Code

Developers frequently work on company code in personal repositories —
side projects that use company APIs, personal forks of internal tools,
or experimental code that should have stayed internal.
```
# Find personal repos mentioning company domain
site:github.com "@targetcompany.com" filetype:env
site:github.com "targetcompany.com" "api_key"
site:github.com "targetcompany" "internal" -org:targetcompany

# Find former employees' repos with company code
site:github.com "targetcompany" "former" filetype:md
```

---

## Part 5 — The GitHub Footprint of a Real Person

GitHub profiles reveal more about a person than almost any other public
platform — because what people build in their spare time, the tools they
use, the communities they contribute to, and the code they write all paint
a detailed picture of who they are professionally and sometimes personally.

---

### What a GitHub Profile Reveals
```
# Full profile including bio, location, company, website
github.com/username

# All public repositories
github.com/username?tab=repositories

# Contribution history (activity calendar)
github.com/username?tab=overview

# Stars (what they find interesting)
github.com/username?tab=stars

# Followers and following (professional network)
github.com/username?tab=followers
```

**The contribution graph:**
The green contribution graph on every GitHub profile shows exactly when
a person is active on GitHub — day by day for the past year. This reveals
work patterns, timezone (when do their commits cluster), whether they work
weekends, and when they take vacations. For OSINT this is behavioral
intelligence derived from public coding activity.

**Commit email exposure:**
Every git commit contains the committer's email address. When commits are
pushed to a public repository, that email is publicly accessible in the
commit metadata. This is how developers accidentally expose their personal
email, work email, or previously used email addresses.
```bash
# Extract emails from a repository's commit history
git clone https://github.com/username/repository
cd repository
git log --format="%ae" | sort -u
```

This command clones the repository and extracts every unique email address
from its commit history. One command. All emails. Permanently embedded in
the public repository.

---

### Google Dorks for Individual Developer Intelligence
```
# Find a developer's exposed credentials
site:github.com/username filetype:env
site:github.com/username "password"
site:github.com/username "api_key"

# Find their work email from commit history
site:github.com/username inurl:commit
"@targetcompany.com" site:github.com/username

# Find what companies they have worked with
site:github.com/username "targetcompany"
site:github.com/username inurl:README "worked at"
```

---

## Part 6 — Deleted Content That Is Not Gone

The hardest thing for developers to accept is that deleting something on
GitHub does not mean it is gone. Here is every place deleted content lives:

---

### Where Deleted GitHub Content Survives

**Google Cache:**
If Google crawled a repository, file, or commit before it was deleted or
made private, that content remains in Google's cache. The repository may
be gone but the cached version is still accessible.
```
cache:github.com/owner/repository
cache:raw.githubusercontent.com/owner/repository/main/filename
```

**Wayback Machine:**
The Internet Archive crawls GitHub. Public repositories, public gists,
and public profile pages are archived. Content made private or deleted
after being archived remains accessible through the Wayback Machine.
```
https://web.archive.org/web/*/github.com/owner/repository
```

**Raw Content URLs:**
GitHub serves raw file content at `raw.githubusercontent.com`. These
URLs are sometimes indexed separately from the main repository page.
Even after a repository is made private, cached raw content URLs may
remain accessible temporarily.
```
# Raw content URL format
https://raw.githubusercontent.com/owner/repository/branch/filename

# Search for cached raw content
site:raw.githubusercontent.com "targetcompany" "password"
```

**Forked Repositories:**
When a public repository is forked before being made private or deleted,
the fork retains all the content from the original — including sensitive
files. Forks are independent copies. Making the original private does not
affect forks.
```
# Find forks of a repository
github.com/owner/repository/forks
site:github.com "forked from owner/repository"
```

---

## Part 7 — The Wake-Up Call

If you maintain any public GitHub repositories and you have never audited
them for exposed credentials, commit history leaks, or sensitive content —
do it today. Not tomorrow. Today.

Here is the audit checklist professionals use:

**Repository audit:**
- [ ] Run Trufflehog against every public repo including full history
- [ ] Run Gitleaks against every public repo
- [ ] Check every `.env` file — are any committed?
- [ ] Check `.gitignore` — does it exclude `.env`, `*.pem`, `*.key`?
- [ ] Review commit history for messages like "remove password" or "fix credentials"
- [ ] Check Gists — review every public Gist for sensitive content

**Profile audit:**
- [ ] What does your email in commit history reveal?
- [ ] Are any work email addresses exposed in commit metadata?
- [ ] Do your personal repos contain code from an employer?
- [ ] Does your contribution graph reveal work patterns you want private?

**Organization audit (if applicable):**
- [ ] Are any repositories accidentally public that should be private?
- [ ] Do CI/CD configuration files contain hardcoded credentials?
- [ ] Are dependency files exposing vulnerable library versions?
- [ ] Is organization membership public when it should not be?

**The tools to run this audit yourself:**
```bash
# Trufflehog — scan all your public repos
trufflehog github --org=yourusername --only-verified

# Gitleaks — scan a specific repo
gitleaks detect --source /path/to/your/repo --log-opts="--all"

# Extract all emails from your commit history
git log --format="%ae" | sort -u
```

A dedicated repository covering the complete GitHub security hardening
process — commit signing setup, branch ruleset configuration, secret
scanning, history scrubbing with `git filter-repo`, `.gitignore` best
practices, and full security posture from day one — is coming as the
next entry in this series. Watch this space.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*