# Shodan — The Search Engine That Sees Everything Connected

> Google indexes websites. Shodan indexes everything else. Every device
> connected to the internet — every server, every camera, every router,
> every industrial control system, every smart TV, every hospital medical
> device, every power grid controller, every baby monitor — Shodan has
> seen it, fingerprinted it, and made it searchable. If that sentence made
> you uncomfortable, good. It should. This section is going to show you
> exactly what Shodan is, what it finds, what bad actors use it for, and
> what security professionals use it for. Both sides of this are real and
> both sides need to be understood.

---

## What Is Shodan?

Shodan was created in 2009 by computer scientist John Matherly. The name
comes from the villain in the video game System Shock — an all-seeing AI
that monitors and controls everything connected to its network. The name
was chosen deliberately. It fits.

Shodan works by continuously scanning the entire public internet —
all 4.3 billion IPv4 addresses — and connecting to every open port it
finds on every IP address. When it connects, it records the response.
That response is called a **banner** — a fingerprint of whatever is
running on that port. It tells Shodan what software is running, what
version, what operating system, what certificates are installed, and
sometimes what the device is and who owns it.

Shodan stores all of this and makes it searchable.

**Plain English:** Imagine if someone walked up to every single door
in every single building in the entire world, knocked, and wrote down
exactly what answered. That is what Shodan does — except on the internet,
and it does it constantly, every day, for every connected device on earth.

The results are available to anyone with an account.

---

## Free vs Paid — What You Actually Get

This is one of the most important things to understand before using Shodan.
The free tier is genuinely useful but significantly limited. Here is exactly
what you get at each level so you know what you are working with.

---

### Free Account (No API Key)

**To get a free account:** Register at `shodan.io` — email and password,
no payment required.

**What the free tier includes:**
- Basic web search at `shodan.io/search`
- **2 search result pages** per query (approximately 20 results)
- Access to the Shodan Maps feature (limited)
- Access to Shodan Exploits search
- Basic host lookup at `shodan.io/host/IP_ADDRESS`
- Access to some saved searches and shared queries

**What the free tier does NOT include:**
- More than 2 pages of results per search
- API access (requires paid plan or one-time credit purchase)
- Shodan Monitor (alerts when your assets are indexed)
- Full export of results
- Vulnerability data for IP addresses
- Historical data for hosts
- SSL certificate search (limited)
- HTTPS scanning results (limited)

**The honest assessment of free tier:**
For learning, for basic investigations, and for checking single IP
addresses or small result sets, the free tier is legitimately useful.
For professional security audits, bug bounty research, or comprehensive
infrastructure mapping, the free tier runs out quickly.

---

### Membership ($69 one-time purchase as of this writing)

**What it adds:**
- 100 query credits per month
- 100 scan credits per month
- API access (1 result per credit)
- No advertisement
- Unlimited search results in the web interface
- Access to saved searches
- Download up to 1,000 results per query

**The query credit system explained:**
One query credit = one search in the API. Browsing the web interface
at `shodan.io` does not consume credits — credits are for programmatic
API access. The web interface has no credit limit with membership.

**Best for:** Individual security researchers, pentesters, and OSINT
investigators who need regular access without a recurring subscription.

---

### API Plans (Monthly Subscription)

**Developer Plan (~$59/month):**
- 1,000,000 query credits/month
- 100 scan credits/month
- Full API access
- SSL, HTTP, and all protocol scanning results
- Vulnerability data
- Historical host data (last 5 results per host)

**Small Business Plan (~$299/month):**
- Everything in Developer
- 10,000,000 query credits/month
- 10,000 scan credits/month
- Access to Shodan Monitor for asset monitoring
- HTTPS and all extended data

**Enterprise:** Custom pricing, used by large organizations for
continuous monitoring of their entire internet-facing infrastructure.

---

### Getting an API Key

After registering (free) or purchasing membership:

1. Log in at `shodan.io`
2. Click your account name → **My Account**
3. Your API key is displayed under **API Key**
4. Copy it — treat it like a password, do not commit it to GitHub

**Using the API key:**
```bash
# Install the Shodan CLI
pip3 install shodan

# Initialize with your API key
shodan init YOUR_API_KEY

# Test it works
shodan info
```

---

## Part 1 — What Shodan Finds

Understanding what Shodan indexes requires understanding what "open port"
means in plain English.

**Plain English:** Every device connected to the internet has an address
(IP address) and can have up to 65,535 virtual "doors" (ports). Each door
is for a specific type of communication. Port 80 is for websites. Port 22
is for remote login (SSH). Port 3306 is for MySQL databases. When one of
these doors is left open and facing the public internet, Shodan walks
up, knocks, and records what answers.

The terrifying reality is how many doors are left open that should not be.

---

### Cameras and Visual Surveillance

Shodan finds internet-connected cameras that have no password, default
passwords, or web interfaces accessible from the public internet.

These include:
- Home security cameras
- Business CCTV systems
- Nanny cams and baby monitors
- Traffic cameras
- University and school campus cameras
- Hospital and medical facility cameras
- Factory floor and warehouse cameras
- Public transit cameras

**Why this is possible:** Camera manufacturers ship devices with default
usernames and passwords (admin/admin, admin/12345, admin/password). Most
consumers never change them. The camera connects to the internet. Shodan
finds it. The web interface is accessible to anyone who finds it.

This is not a theoretical vulnerability. This is happening right now,
at scale, to millions of cameras globally. It has been happening for
over a decade.

---

### Industrial Control Systems (ICS) and SCADA

This is where Shodan moves from unsettling to genuinely alarming.

**Plain English:** Industrial Control Systems are the computers that
control physical infrastructure — power plants, water treatment facilities,
oil pipelines, manufacturing lines, dam controls, traffic management
systems, building automation (heat, ventilation, elevators, locks).

These systems were designed decades ago for isolated internal networks.
They were never meant to be connected to the internet. Many of them are.
Many of them have no authentication. Many of them are findable and
controllable through Shodan.

The Ukraine power grid attacks of 2015 and 2016. The Oldsmar Florida water
treatment attack of 2021 where an attacker remotely changed sodium hydroxide
levels to potentially poisonous concentrations. The Triton/TRISIS malware
targeting safety systems at a petrochemical plant. All of these attacks
involved industrial systems that should not have been reachable from the
public internet.

Shodan finds these systems. Security researchers document them. Attackers
exploit them.

> ⚠️ **Critical:** Accessing or interacting with ICS/SCADA systems without
> explicit written authorization from the owner is a federal crime in the
> United States under the Computer Fraud and Abuse Act and equivalent laws
> in most countries. Penalties include significant prison time. These systems
> are documented here so infrastructure owners and security professionals
> understand their exposure. Never interact with any ICS system you do not
> own or have explicit written permission to test.

---

### Medical Devices and Healthcare

Shodan finds internet-connected medical devices. This includes:

- Hospital patient monitoring systems
- Medical imaging systems (MRI, CT, X-ray)
- Infusion pumps and medication dispensing systems
- Electronic health record servers
- PACS (Picture Archiving and Communication Systems — medical imaging storage)

Medical devices run old software. They often cannot be patched without
FDA recertification. They run Windows XP and Windows 7 on networks connected
to the internet. They have no authentication on their management interfaces.
Shodan finds all of it.

The exposure of medical imaging systems is particularly significant — PACS
servers exposed to the internet have leaked millions of patient medical
images including X-rays, MRIs, and CTs along with patient names, dates of
birth, and doctor information. This is happening at hospitals globally,
documented by security researchers through Shodan.

---

### Databases

Databases that should never face the public internet frequently do. Shodan
finds:

- MongoDB instances (often with no authentication)
- Elasticsearch clusters (frequently containing millions of records)
- Redis servers (often storing active session tokens)
- MySQL and PostgreSQL databases
- CouchDB instances
- Cassandra clusters
- Memcached servers

The MongoDB and Elasticsearch exposures are historically the largest.
Billions of records — personal information, financial data, medical records,
login credentials — have been exposed through databases left open on the
public internet and indexed by Shodan.

---

### Routers, Switches, and Network Equipment

Every router with a web management interface is a potential Shodan result.
This includes:

- Home routers (Netgear, Linksys, ASUS, TP-Link, D-Link)
- Business-grade routers and firewalls
- Managed switches
- VPN concentrators
- Load balancers

Many of these devices use default credentials that manufacturers never
change and consumers never update. Finding a router through Shodan and
logging in with admin/admin gives an attacker control over the entire
network behind it.

---

### Smart Home and IoT Devices

The Internet of Things has created an enormous attack surface of devices
that were designed for convenience and not for security:

- Smart TVs with exposed interfaces
- Smart refrigerators and appliances
- Voice assistant hardware management interfaces
- Smart locks and access control systems
- Solar panel and home energy management systems
- EV charging stations
- Building automation and HVAC systems

---

## Part 2 — What Bad Actors Use Shodan For

This section exists because understanding the attack perspective is how
defenders know what to protect. These are real techniques used by real
threat actors. Knowing them is not the same as using them maliciously.

---

### Ransomware Reconnaissance

Ransomware groups use Shodan to identify targets before attacking. The
workflow:

1. Search Shodan for specific software versions with known vulnerabilities
2. Identify organizations running unpatched systems
3. Find exposed Remote Desktop Protocol (RDP) servers — these are the
   most common ransomware entry point
4. Purchase access to compromised systems found through Shodan searches
   on dark web markets
5. Deploy ransomware
```
# Exposed RDP servers — the ransomware entry point
port:3389 has_screenshot:true
port:3389 os:"Windows" country:"US"
port:3389 "Windows Server 2008"
```

Shodan even has a screenshot feature — for devices with visual interfaces
it captures screenshots of what is running. Searching for RDP servers with
screenshots shows you login screens of exposed Windows servers in real time.

---

### Credential Stuffing Infrastructure Discovery

Attackers use Shodan to find databases and authentication systems to target
with stolen credential lists:
```
# Finding exposed authentication systems
port:27017 -authentication
port:9200 -authentication
port:6379 -authentication
```

The `-authentication` filter finds services where authentication is
disabled — open databases with no password required.

---

### Botnet Recruitment

IoT botnets like Mirai (which caused the 2016 Dyn DNS attack that took
down Twitter, Netflix, Reddit, and CNN simultaneously) were built by
scanning the internet for devices with default credentials — the exact
capability Shodan provides in searchable form.

Mirai's source code became public. Variants are still actively deployed
by scanning for vulnerable IoT devices, logging in with default credentials,
and enrolling devices in the botnet. Shodan makes finding these devices
trivially easy.

---

### Critical Infrastructure Targeting

Nation-state threat actors use Shodan to map critical infrastructure
exposure before targeted attacks. The documented cases include:

- **Volt Typhoon (China):** Documented use of Shodan to identify
  US critical infrastructure targets including power, water, and
  communications
- **Sandworm (Russia):** Used internet scanning to identify vulnerable
  industrial control systems before the Ukraine grid attacks
- **Iranian threat actors:** Documented Shodan use to identify exposed
  Israeli and US infrastructure targets

This is not theoretical. Shodan data is actively used in nation-state
level offensive operations.

---

### Physical Security Circumvention

Finding exposed building access control systems, IP cameras covering
entry points, and smart lock management interfaces through Shodan gives
attackers pre-attack intelligence about physical security — what cameras
are where, what access control system is in use, and sometimes the ability
to directly interact with building systems.

---

## Part 3 — What Security Professionals Use Shodan For

The same tool that attackers use is used by defenders for exactly the same
purpose — understanding what is exposed. The difference is authorization
and intent.

---

### Asset Discovery

Organizations frequently do not have a complete picture of their own
internet-facing infrastructure. Shadow IT, forgotten servers, old systems
that were supposed to be decommissioned — Shodan finds all of it.
```bash
# Find all assets associated with an organization's IP range
shodan search org:"Target Organization Name"

# Find all assets on a specific IP range
shodan search net:"X.X.X.0/24"

# Find specific services on an organization's IP range
shodan search org:"Target Organization" port:22
shodan search org:"Target Organization" port:3389
shodan search org:"Target Organization" port:27017
```

---

### Vulnerability Management

Shodan's vulnerability data (available on paid plans) maps CVEs to
exposed devices. Finding that a critical vulnerability affects a specific
version of software and then finding all your internet-facing assets
running that version — before an attacker does — is core vulnerability
management work.
```bash
# Find devices with specific CVEs (paid feature)
shodan search vuln:CVE-2021-44228
shodan search vuln:CVE-2019-0708

# Find specific vulnerable versions
shodan search "Apache/2.4.49"
shodan search "Microsoft-IIS/7.5"
```

---

### SSL Certificate Analysis

Every HTTPS server has an SSL certificate. Certificates contain the domain
names they are issued for — including internal hostnames and subdomains
that never appear in DNS. Shodan indexes certificate data.
```bash
# Find all certificates issued to an organization
shodan search ssl.cert.subject.CN:"targetdomain.com"
shodan search ssl.cert.subject.O:"Company Name"

# Find expired certificates (security risk)
shodan search ssl.cert.expired:true org:"Target Organization"

# Find self-signed certificates (potential internal systems)
shodan search ssl.cert.issuer.CN:ssl.cert.subject.CN org:"Target Organization"
```

**Why certificate search is powerful:**
Certificates issued for `internal.company.com`, `vpn.company.com`,
`dev-api.company.com` reveal internal infrastructure that was never
meant to be discovered externally — but the certificate itself is public.
This is one of the most effective techniques for subdomain and
infrastructure discovery.

---

### Exposed Service Monitoring

Security teams use Shodan Monitor (paid feature) to receive alerts when
new assets belonging to their organization appear in Shodan's index —
meaning a new internet-facing service has been detected. This is continuous
passive monitoring of your own external attack surface.

---

## Part 4 — Shodan Search Syntax

---

### Core Filters

| Filter | What It Does | Example |
|---|---|---|
| `port:` | Filter by port number | `port:22` |
| `country:` | Filter by country code | `country:US` |
| `city:` | Filter by city | `city:"New York"` |
| `org:` | Filter by organization/ISP | `org:"Amazon"` |
| `hostname:` | Filter by hostname | `hostname:targetdomain.com` |
| `net:` | Filter by IP range (CIDR) | `net:192.168.1.0/24` |
| `os:` | Filter by operating system | `os:"Windows Server 2019"` |
| `product:` | Filter by software product | `product:"Apache httpd"` |
| `version:` | Filter by software version | `version:"2.4.49"` |
| `ssl:` | Search SSL certificate data | `ssl:"targetdomain.com"` |
| `http.title:` | Search HTTP page titles | `http.title:"Admin Panel"` |
| `http.html:` | Search HTTP page content | `http.html:"password"` |
| `has_screenshot:` | Only results with screenshots | `has_screenshot:true` |
| `vuln:` | Filter by CVE (paid) | `vuln:CVE-2021-44228` |
| `before:` | Results before date | `before:"01/01/2025"` |
| `after:` | Results after date | `after:"01/01/2024"` |
| `asn:` | Filter by ASN number | `asn:AS15169` |

---

### Real Shodan Search Recipes
```
# Find exposed webcams with no authentication
has_screenshot:true port:554
has_screenshot:true port:8080 "camera"

# Find exposed MongoDB databases
port:27017 -authentication
port:27017 MongoDB

# Find exposed Elasticsearch
port:9200 elasticsearch

# Find exposed Redis
port:6379 -authentication

# Find exposed RDP servers
port:3389 has_screenshot:true
port:3389 os:"Windows Server"

# Find exposed industrial systems
port:502 Modbus
port:102 Siemens
port:20000 DNP3

# Find exposed Cisco devices
cisco "last-modified"
cisco ios telnet

# Find default credential devices
"default password"
"admin" "password" port:80

# Find exposed printers
port:9100 "HP"
port:9100 "Xerox"
port:631 ipp

# Find exposed VoIP systems
port:5060 sip
port:5060 Asterisk

# Find exposed FTP with anonymous login
port:21 "anonymous"
port:21 "230 login successful"

# Find exposed MQTT brokers (IoT messaging)
port:1883 MQTT

# Find vulnerable Apache Log4Shell instances
vuln:CVE-2021-44228

# Find exposed Kubernetes dashboards
http.title:"Kubernetes Dashboard"
port:8001 kubernetes

# Find exposed Jenkins
http.title:"Dashboard [Jenkins]"
port:8080 jenkins

# Find exposed Grafana
http.title:"Grafana"
port:3000 grafana

# Find exposed phpMyAdmin
http.title:"phpMyAdmin"
port:80 phpmyadmin

# Find medical imaging systems
"DICOM" port:104
"DICOM" port:11112

# Find exposed building automation
port:47808 bacnet
port:20000 "building"

# Find solar and energy management
port:502 "solar"
port:102 "energy"

# Find specific organization's exposure
org:"Target Company" port:22
org:"Target Company" port:3389
org:"Target Company" port:27017
org:"Target Company" has_screenshot:true

# Find everything on a specific IP range
net:X.X.X.0/24
net:X.X.X.0/24 port:22
net:X.X.X.0/24 -authentication
```

---

## Part 5 — Shodan CLI and API Usage

The Shodan command line interface and API unlock programmatic access —
automating searches, pulling large result sets, and integrating Shodan
data into security workflows.

---

### CLI Commands
```bash
# Search from command line
shodan search "apache port:80"

# Limit results
shodan search --limit 100 "apache port:80"

# Download results to file
shodan download results "apache port:80"
shodan parse results.json.gz

# Look up a specific IP
shodan host X.X.X.X

# Look up your own IP
shodan myip

# Check your API plan status
shodan info

# Count results without consuming credits
shodan count "apache port:80"

# Search for specific organization
shodan search org:"Target Organization"

# Find all open ports for an IP
shodan host X.X.X.X --history
```

---

### Python API Usage
```python
import shodan

# Initialize with API key
api = shodan.Shodan('YOUR_API_KEY')

# Basic search
results = api.search('apache port:80')
print(f"Total results: {results['total']}")

for result in results['matches']:
    print(f"IP: {result['ip_str']}")
    print(f"Port: {result['port']}")
    print(f"Data: {result['data'][:100]}")
    print("---")

# Host lookup
host = api.host('X.X.X.X')
print(f"Organization: {host.get('org', 'Unknown')}")
print(f"OS: {host.get('os', 'Unknown')}")
for item in host['data']:
    print(f"Port: {item['port']}")
    print(f"Banner: {item['data'][:100]}")

# Count without consuming credits
count = api.count('apache port:80')
print(f"Total: {count['total']}")

# Streaming API (large result sets)
api = shodan.Shodan('YOUR_API_KEY')
for banner in api.search_cursor('apache port:80'):
    print(f"IP: {banner['ip_str']}")
```

---

## Part 6 — Shodan for Defensive Use — Auditing Your Own Exposure

If you run any internet-facing infrastructure, here is the audit you
should run against yourself before an attacker runs it against you:
```bash
# Find everything Shodan has on your IP
shodan host YOUR_IP_ADDRESS

# Find everything on your IP range
shodan search net:YOUR_IP_RANGE/24

# Find everything associated with your organization name
shodan search org:"Your Organization Name"

# Find your domains in certificate data
shodan search ssl:"yourdomain.com"

# Check for specific vulnerable services
shodan search org:"Your Org" port:3389
shodan search org:"Your Org" port:27017
shodan search org:"Your Org" port:9200
shodan search org:"Your Org" -authentication

# Check for known CVEs on your assets (paid)
shodan search org:"Your Org" vuln:CVE-2021-44228
```

**What to do when you find exposed services:**

1. **Immediately verify** — confirm it is actually your asset and actually exposed
2. **Take it offline or firewall it** if it should not be public-facing
3. **Change default credentials** immediately on anything with default login
4. **Patch** any identified vulnerable versions
5. **Monitor** — set up Shodan Monitor alerts for your IP ranges so you
   know immediately when new assets appear in their index

---

## Part 7 — Shodan Alternatives Worth Knowing

Shodan is the most well-known internet scanning platform but it is not
the only one. Each has different coverage and capabilities:

| Platform | Strength | URL |
|---|---|---|
| **Censys** | Deeper certificate analysis, academic-grade research | search.censys.io |
| **ZoomEye** | Strong Asian infrastructure coverage | zoomeye.org |
| **FOFA** | Chinese infrastructure focus, used by Chinese security researchers | fofa.info |
| **BinaryEdge** | Real-time scanning, risk scoring | binaryedge.io |
| **GreyNoise** | Identifies scanner and bot traffic vs targeted attacks | greynoise.io |
| **Criminal IP** | Focused on malicious IP identification | criminalip.io |
| **Onyphe** | Combines Shodan-style scanning with threat intelligence | onyphe.io |

**Censys** is the primary professional alternative to Shodan. It has
deeper SSL/TLS certificate analysis, a more academic-grade research
focus, and different indexing priorities that surface results Shodan
misses. Running the same search on both Shodan and Censys is standard
practice for comprehensive infrastructure mapping.

**GreyNoise** serves a different purpose — it identifies which IPs are
actively scanning the internet versus which are targeted attack sources.
When you see unexplained traffic in your logs, GreyNoise tells you
whether it is a mass scanner (not targeting you specifically) or
something more concerning.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*