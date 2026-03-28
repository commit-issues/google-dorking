# Social Media Dorks & People Intelligence

> This section goes far beyond "search someone's name on Google." We are
> talking about reverse image analysis, metadata extraction from photos,
> geolocation from background details, cross-platform identity correlation,
> and techniques that professional investigators and law enforcement actually
> use. If you follow me for cybersecurity content, this is the section where
> things get real.
>
> Everything here is passive. Everything here uses publicly available
> information. The ethics of what you do with it are entirely yours to own.
> Read the [Legal and Ethics](../legal-and-ethics/ethics.md) section. No
> vigilantism. No stalking. No harassment. Full stop.

---

## Why Social Media Is the Richest OSINT Source Available

Every time someone posts online they are potentially revealing:

- Their location (directly tagged, or embedded in photo metadata)
- Their daily routine (posting times, check-ins, recurring locations)
- Their relationships (tagged people, comments, mutual followers)
- Their devices (photo metadata, browser fingerprints in some cases)
- Their real identity (even behind anonymous accounts)
- Their physical environment (background details in photos and videos)

Most people understand that social media is public. Almost nobody understands
*how much* of their life can be reconstructed from what they post. This
section shows you the full picture.

---

## Part 1 — Reverse Image Search (The Most Underestimated Tool in OSINT)

Most people know you can drag a photo into Google Images. Almost nobody uses
the full depth of what reverse image search can actually do.

**What reverse image search does:** It takes an image — or part of an image —
and searches for visually similar images across the web. This means it can
find other places the same photo has been posted, other accounts using the
same profile picture, and sometimes the original source of a photo that has
been cropped, filtered, or modified.

---

### The Multi-Engine Approach

No single reverse image search engine finds everything. Professionals run
every image through multiple engines because each one indexes differently.

| Engine | Strength | URL |
|---|---|---|
| **Google Images** | Broadest web coverage | images.google.com |
| **TinEye** | Finds oldest and most exact matches, tracks image history | tineye.com |
| **Yandex Images** | Best facial recognition of any free engine, finds faces Google misses | yandex.com/images |
| **Bing Visual Search** | Good for product and landmark identification | bing.com/visualsearch |
| **PimEyes** | Facial recognition search across the public web (free tier limited) | pimeyes.com |
| **FaceCheck.ID** | Face search across social media specifically | facecheck.id |
| **Search4Faces** | Indexes VK (Russian social network) heavily, finds Eastern European profiles | search4faces.com |

**The Yandex advantage:** Yandex's facial recognition is significantly more
powerful than Google's for finding faces across different photos, different
angles, different lighting. This is well documented in the OSINT community.
If Google Images finds nothing for a face photo, Yandex frequently does.
This is why Yandex gets its own full section in this guide.

---

### Reverse Image Search Techniques That Actually Work

**Technique 1 — Crop to the face only.** When searching for a person, crop
the image so only the face fills the frame before uploading. Engines perform
better with isolated subjects than full scene photos.

**Technique 2 — Remove filters and restoration.** If you have a filtered
Instagram photo, run it through a filter removal tool first. Filters confuse
reverse image search engines. Tools like `Image Cleaner` or simply adjusting
contrast and saturation in Preview can help normalize heavily filtered photos.

**Technique 3 — Screenshot instead of download.** Some platforms embed
invisible tracking watermarks in downloaded images. Taking a screenshot
instead of downloading bypasses this in some cases.

**Technique 4 — Search by URL not upload.** If an image is hosted publicly,
paste the direct image URL instead of downloading and re-uploading. This
sometimes returns different results.

**Technique 5 — The profile picture history method.** People change profile
pictures but old ones stay indexed. TinEye is the best tool for finding
earlier versions of a profile picture — sometimes finding the original photo
from years before on a completely different platform with a completely
different name attached.

---

### What You Can Find With Reverse Image Search

- **Same person, different accounts:** A profile picture used on Instagram
  also appears on a dating site, a forum, and a LinkedIn under a different name
- **Location identification:** A photo posted without geotag can be
  reverse searched to find the same location tagged by someone else
- **Stock photo catfishing:** Profile pictures that are stolen stock photos
  or images from other people's accounts
- **Original source of modified images:** Cropped, filtered, or slightly
  edited images traced back to their original unmodified source
- **Cross-platform account linking:** The same photo appearing across multiple
  platforms links those accounts together even if the usernames are different

---

## Part 2 — Photo Metadata and EXIF Data

Every photo taken on a smartphone or camera contains hidden data called
**EXIF data** (Exchangeable Image File Format). This is metadata — information
*about* the photo rather than the photo itself.

EXIF data can contain:

- **GPS coordinates** — exact latitude and longitude where the photo was taken
- **Timestamp** — exact date and time, sometimes with timezone
- **Device make and model** — iPhone 15 Pro, Samsung Galaxy S24, etc.
- **Camera settings** — aperture, shutter speed, ISO (useful for identifying
  professional vs consumer equipment)
- **Software** — what app was used to take or edit the photo
- **Unique device identifiers** in some cases

**The GPS coordinate problem:** When someone takes a photo at home on their
smartphone with location services enabled and posts it without stripping
metadata, that photo contains the GPS coordinates of their home. Most major
platforms (Instagram, Twitter, Facebook) strip EXIF data server-side before
displaying images. But many do not — direct image shares, email attachments,
images posted to forums, images hosted on personal websites, and images shared
through many messaging apps retain their full EXIF data.

---

### How to Extract EXIF Data

**Free tools:**

| Tool | How to Use | URL |
|---|---|---|
| **ExifTool** | Command line, most powerful, handles every format | exiftool.org |
| **Jeffrey's Exif Viewer** | Web-based, paste image URL or upload | exifdata.com |
| **Jimpl** | Web-based EXIF viewer | jimpl.com |
| **Metadata2Go** | Web-based, handles many file types beyond images | metadata2go.com |
| **ExifPurge** | Shows EXIF then lets you strip it (for your own images) | exifpurge.com |

**ExifTool command line:**
```bash
# View all metadata from a file
exiftool filename.jpg

# View GPS data specifically
exiftool -GPS* filename.jpg

# View all metadata from all images in a folder
exiftool /path/to/folder/

# Strip all metadata from an image (for your own privacy)
exiftool -all= filename.jpg
```

**Converting GPS coordinates to a real address:**

ExifTool gives you coordinates in degrees/minutes/seconds format like:
```
GPS Latitude  : 40 deg 44' 54.36" N
GPS Longitude : 73 deg 59' 8.52" W
```

Convert to decimal format for Google Maps:
- Degrees + (Minutes/60) + (Seconds/3600)
- North and East are positive, South and West are negative

Then paste into Google Maps or:
```
https://maps.google.com/?q=40.748433,-73.985700
```

---

## Part 3 — Geolocation From Photo Content (No Metadata Required)

This is where OSINT becomes genuinely impressive. Even when EXIF data has
been stripped, photographs contain visual clues that can identify location
with surprising precision. This technique is called **geoint** —
geospatial intelligence from imagery.

Investigators, journalists, and intelligence analysts use this to verify
the location of photos from conflict zones, identify where crimes occurred,
and confirm or deny claims about where something happened.

---

### What to Look for in Background Details

**Street signs and road markings:**
- Font, color, and shape of road signs vary by country and region
- Highway markers are country-specific
- Lane marking styles differ internationally
- Utility pole styles vary by region

**Architecture:**
- Roof styles vary dramatically by region and climate
- Building materials (brick types, concrete styles) are regionally specific
- Window styles, balcony designs, and facade details
- Religious buildings indicate likely country and sometimes region

**Vegetation:**
- Palm trees narrow location to tropical/subtropical regions
- Specific species of trees are regionally unique
- Agricultural crops in backgrounds narrow geography significantly

**Vehicles:**
- License plate shape, color, and format — every country and most regions
  have unique plate designs
- Vehicle models sold in specific markets (some cars are not sold in certain
  countries at all)
- Steering wheel position visible through windows — left vs right hand traffic

**Sky and shadows:**
- Sun angle and shadow direction can confirm approximate latitude and time of day
- Cloud formations and weather patterns are somewhat regionally characteristic

**Language:**
- Text on signs, storefronts, menus in backgrounds
- Script type (Latin, Cyrillic, Arabic, Chinese, etc.) immediately narrows region

---

### License Plate Intelligence

License plates are one of the most information-dense objects in any photo
that includes a vehicle. Even a partially visible plate reveals significant
information.

**What plate design tells you:**
Every country has unique plate formats. Within the United States, every state
has a distinctive plate design. Most European countries have country codes on
the left edge of the plate. Some countries include regional codes within the
plate number format itself.

**Free plate identification resources:**

| Resource | What It Does | URL |
|---|---|---|
| **Platesmania** | Global license plate format reference with photos | platesmania.com |
| **License Plates of the World** | Comprehensive format database | licenseplates.tv |
| **WorldLicensePlates.com** | Country and regional format guide | worldlicenseplates.com |

**What a full plate number can potentially reveal (with legal access):**
- Registered owner name and address (law enforcement only in most countries)
- Vehicle make, model, year, and color
- Registration status and expiry
- Insurance status
- Whether the vehicle has been reported stolen
- In some countries, toll road usage history

**What civilians can legally look up with a plate number:**
This varies dramatically by country. In the United States, the Driver's
Privacy Protection Act (DPPA) restricts most plate lookups. However, some
states make vehicle registration records partially public. Commercial services
like BeenVerified and Spokeo aggregate some of this data. Legitimate use cases
include buying a used vehicle, verifying a delivery vehicle, or documenting
a hit-and-run.

> ⚠️ Using plate numbers to locate or track individuals without authorization
> is illegal in most jurisdictions. This information is documented for
> awareness and legitimate investigative use only.

---

### Cross-Referencing Vehicle Type With State/Region

This technique combines plate format, vehicle type, and environmental clues
to narrow location far beyond what any single data point provides.

**Example workflow:**
1. Photo shows a pickup truck with a partial plate
2. Plate shape is American — rectangular, white background
3. Plate color pattern matches Texas registration
4. Truck model is a Ford F-250 with a brush guard — common in rural Texas
5. Background shows cedar trees and limestone rock formations — consistent
   with Texas Hill Country
6. Road is unpaved caliche — characteristic of rural Central Texas

No single detail confirms the location. All five together paint a very specific
picture. This is the core methodology of geolocation — stacking clues until
the intersection of all of them points to one place.

**Tools for vehicle identification:**

| Tool | What It Does | URL |
|---|---|---|
| **CarNetAI** | AI vehicle identification from photos | carnet.ai |
| **What Car Is This** | Community vehicle identification | whatcaristhis.com |
| **Auto Identify** | Image-based vehicle make/model identification | autoidentify.ai |

---

## Part 4 — Platform-Specific Dorks and Techniques

---

### Google Dorks for Social Media
```
# Find profiles by name
"firstname lastname" site:linkedin.com
"firstname lastname" site:twitter.com
"firstname lastname" site:instagram.com

# Find profiles with specific info
site:linkedin.com/in "company name" "job title"
site:linkedin.com "company name" "email" "@gmail.com"

# Find cached or indexed social posts
site:twitter.com "username" "keyword"
site:reddit.com/u/"username"

# Find people mentioning a target company
site:twitter.com "targetcompany" "insider" OR "employee" OR "fired"

# Find public Facebook profiles
site:facebook.com "firstname lastname"
site:facebook.com/public "firstname lastname"

# Find Instagram posts indexed by Google
site:instagram.com "username"
site:instagram.com "location name"
```

---

### Facebook and Meta Intelligence

Facebook has the most data of any social platform on earth. Most of it is
locked behind login walls — but more is public than users realize.

**Facebook-specific dorks:**
```
site:facebook.com "targetname" "works at"
site:facebook.com "targetname" "lives in"
site:facebook.com "targetname" "phone"
site:facebook.com/public/posts "keyword"
```

**Facebook Graph Search is officially dead** — Meta removed it in 2019. But
the underlying URL structure of Facebook still allows some direct queries.

**The Facebook ID technique:**
Every Facebook account has a unique numeric ID. The profile URL format is
`facebook.com/profile.php?id=XXXXXXXXXX`. This ID does not change even if
the username changes. Finding someone's Facebook ID lets you track profile
changes over time.

Tools:
- **Find My FB ID** — paste any Facebook profile URL, get the numeric ID
- **Lookup-ID.com** — Facebook and Instagram ID lookup

**Facebook photos through Google:**
```
site:facebook.com/photo "targetname"
site:facebook.com/media/set "targetname"
```

Photos posted publicly on Facebook are indexed by Google. Even if the person
later restricted their profile, archived Google results and Wayback Machine
snapshots may retain indexed photos.

---

### Instagram Intelligence

Instagram strips EXIF data from uploaded photos — but the photos themselves
still contain enormous amounts of intelligence.

**Instagram-specific dorks:**
```
site:instagram.com "targetusername"
site:instagram.com "location name"
"instagram.com/p/" "targetusername"
```

**The location tag intelligence method:**
When someone tags a location on Instagram, that post is added to a public
location page — visible to anyone, no account required. Searching a specific
location page shows every public post ever tagged there. This lets you:

- Map everyone who regularly visits a specific location
- Identify who was present at a specific place at a specific time
- Find photos of a location's interior from multiple angles

**Instagram location URL format:**
```
https://www.instagram.com/explore/locations/LOCATION_ID/location-name/
```

Location IDs can be found by clicking any location tag. Searching the
location ID across Google:
```
site:instagram.com/explore/locations/ "location name"
```

**The highlight archive:** Instagram Stories disappear after 24 hours — unless
they are saved to Highlights. Public Highlights are permanently accessible
and indexable. Many people forget this and leave sensitive location-tagged
stories in their Highlights indefinitely.

---

### LinkedIn Intelligence

LinkedIn is the most information-dense professional intelligence source
available. People voluntarily publish their employer history, job titles,
skills, education, certifications, connections, and contact information.

**LinkedIn-specific dorks:**
```
site:linkedin.com/in "company name"
site:linkedin.com/in "job title" "company name"
site:linkedin.com/in "company name" "email"
site:linkedin.com "company name" employees
site:linkedin.com/pub "firstname lastname"

# Find employees at a specific company with specific skills
site:linkedin.com "works at targetcompany" "python" OR "aws" OR "kubernetes"

# Find people who recently left a company
site:linkedin.com "former" "targetcompany" "security engineer"

# Find contractors and freelancers connected to a company
site:linkedin.com "consultant" "targetcompany"
```

**The employee enumeration technique:**
LinkedIn company pages show employee counts and list employees. Combining
LinkedIn data with Hunter.io email format discovery lets you construct
corporate email addresses for any employee you can find. This is a standard
technique in red team engagements and phishing simulations.

**The tech stack intelligence method:**
Search for current and former employees with technical skills:
```
site:linkedin.com "targetcompany" "AWS" "Kubernetes" "Terraform"
site:linkedin.com "targetcompany" "Oracle" "SAP"
site:linkedin.com "targetcompany" "Splunk" "CrowdStrike"
```

Every skill listed by employees is a data point about what technology the
company runs internally.

---

### Twitter/X Intelligence

Twitter/X has one of the largest archives of real-time public posts on the
internet. Even with recent API restrictions, significant intelligence is
available through Google dorking.

**Twitter-specific dorks:**
```
site:twitter.com "targetusername"
site:twitter.com "keyword" "targetname"
site:twitter.com "from:username" "keyword"

# Find tweets mentioning a company from specific account types
site:twitter.com "targetcompany" "password" OR "credentials" OR "breach"

# Find deleted tweets that Google cached
cache:twitter.com/username/status/TWEETID

# Find tweets with location info
site:twitter.com "targetname" "from" "cityname"
```

**Advanced Twitter OSINT tools:**

| Tool | What It Does | URL |
|---|---|---|
| **Nitter** | Privacy-friendly Twitter viewer, no account needed | nitter.net |
| **Social Bearing** | Twitter analytics and search without API | socialbearing.com |
| **Twint** | Twitter scraping tool (run locally, no API) | github.com/twintproject/twint |
| **Twitter Advanced Search** | Native — underused by most | twitter.com/search-advanced |

**Twitter Advanced Search** is built into the platform but almost nobody uses
it. You can filter by account, date range, engagement level, replies vs
original posts, and language. Pair it with Google dorking for targets that
have restricted their profile but have older indexed tweets.

---

### Twitch Intelligence

Twitch is an OSINT goldmine that almost no guide covers seriously. Streamers
reveal enormous amounts of personal information over time — often without
realizing it — because streaming is a long-form, real-time format that rewards
authenticity and personal connection with the audience.

**What streamers accidentally reveal:**
- Real name (Twitch requires legal name for affiliate/partner payout — sometimes
  mentioned on stream accidentally, appears in DMCA notices, or leaked through
  PayPal/Venmo donation screens)
- Physical location (background details, accent, timezone from streaming hours,
  local news on in background, local sports teams discussed)
- Daily routine (consistent stream schedule reveals when they are home)
- Device specifications (Twitch's stream info panel often shows PC specs)
- Connected accounts (Twitch panels show linked social media, Discord,
  Amazon/Prime Gaming connections)

**Twitch-specific dorks:**
```
site:twitch.tv "username"
site:twitch.tv/username/about
"twitch.tv/username" site:twitter.com
"twitch.tv/username" site:instagram.com
"username" "twitch" site:linkedin.com
```

**The Twitch → Amazon Gaming connection:**
Twitch is owned by Amazon. Streamers who subscribe to Amazon Prime get Prime
Gaming included. Many streamers publicly link their Amazon wish lists to their
Twitch channel panels for viewer gifts. **Amazon wish lists are public by
default** and until 2023 displayed the registered name and shipping address
city of the list owner in some cases. Even now, publicly linked wish lists
reveal:
- The registered name on the Amazon account
- Sometimes location (city/state visible on some list configurations)
- Purchasing patterns and interests
- Sometimes connected social accounts

**Finding Twitch → Amazon links:**
```
site:twitch.tv "amazon.com/wishlist" "username"
site:twitch.tv "amzn.to" "username"
"twitch.tv/username" "wish list"
```

**The VOD archive method:**
Twitch VODs (past broadcasts) are archived for 14-60 days depending on
account tier. But streamers who upload highlights to YouTube create a
permanent archive. Searching YouTube for old stream content often surfaces
things that were said or shown on stream years ago that the streamer has
long forgotten about.
```
site:youtube.com "twitchusername" stream highlights
site:youtube.com "twitchusername" clip
```

**The Discord connection:**
Nearly every active Twitch streamer has a Discord server linked from their
Twitch channel. Discord servers frequently contain:
- The streamer's real name in verified roles or welcome messages
- Timezone information in scheduled events
- Location hints in regional channels (EU/NA server divisions, local weather
  discussions)
- Connected social media in introduction channels

---

## Part 5 — Browser DevTools for Social Media Intelligence

> **Note:** This section covers passive observation using your browser's
> built-in developer tools against your own session and publicly accessible
> pages. The advanced application of this — cookies, session tokens, API
> calls, and what you can do once you have certain data — is covered in depth
> in [Advanced Combinations](./advanced-combinations.md). What follows here
> is the foundation.

Every modern browser has built-in developer tools (DevTools). On any platform:
- **Chrome/Brave/Edge:** F12 or Cmd+Option+I (Mac)
- **Firefox:** F12 or Cmd+Option+I (Mac)
- **Safari:** Enable in Preferences → Advanced → Show Develop menu, then
  Cmd+Option+I

DevTools lets you look underneath the surface of any webpage you can access.

---

### What DevTools Reveals on Social Platforms

**The Network tab** shows every request and response your browser makes when
loading a page. On a social media profile you can see:

- API calls the platform makes to fetch user data
- The exact data returned by those API calls — often including fields not
  displayed in the UI
- Image source URLs (which can be reverse searched)
- Request headers including session tokens and authentication

**Finding non-displayed profile data:**
Many platforms return more data in their API responses than they display in
the UI. Opening DevTools → Network → filtering for `fetch` or `xhr` requests
on a profile page and examining the JSON responses sometimes reveals:

- Account creation date
- User ID numbers
- Email address (partially masked)
- Connected account flags
- Last active timestamps
- Geographic data from the account's IP history in some cases

**The Instagram example:**
When you view a public Instagram profile, the page makes an API call to
`/api/v1/users/web_profile_info/`. The JSON response contains significantly
more data than what Instagram displays — including full name, biography,
follower counts, media counts, and in some cases account verification status
and business category information.

To view it:
1. Open DevTools → Network tab
2. Navigate to a public Instagram profile
3. Filter requests for `web_profile_info`
4. Click the request → Preview tab
5. Expand the JSON tree

This is not exploiting anything. This is reading data that your browser
already received — you are just looking at the raw version.

---

### Finding Hidden or Obfuscated User IDs

Most platforms use internal numeric IDs that are different from display
usernames. These IDs are stable — they do not change when a username changes.
Finding a user's internal ID is valuable because:

- It lets you track account changes over time
- It enables more precise API queries
- It sometimes enables cross-platform correlation when the same ID format
  is used across related services

**Twitter/X User ID:**
```
https://tweeterid.com
```
Enter any Twitter username, get the stable numeric ID.

**Instagram User ID:**
Found in the API response method above, or through tools like:
```
https://www.instagramid.com
```

**Facebook User ID:**
```
https://lookup-id.com
```

---

## Part 6 — Cross-Platform Identity Correlation

This is the synthesis of everything above. Individual data points are
interesting. A complete cross-platform identity map is intelligence.

**The correlation methodology:**

**Step 1 — Start with what you have.**
Even a single username, email, or profile photo is enough.

**Step 2 — Expand across platforms.**
Run the username through Sherlock/Maigret. Run the email through Holehe.
Run the profile photo through all reverse image engines.

**Step 3 — Extract usernames from each platform found.**
Different platforms may show slightly different usernames — all variants
become new search seeds.

**Step 4 — Cross-reference metadata.**
Posting times across platforms reveal timezone → region. Writing style and
vocabulary can confirm identity across accounts. Profile photo history through
TinEye reveals when accounts were created.

**Step 5 — Map the network.**
Who does this person interact with consistently? Mutual connections across
platforms narrow real-world social circles.

**Step 6 — Ground truth.**
Find one piece of confirmed real-world data — a real name, a city, a
workplace — and use it to verify the entire picture.

**Free tools for this workflow:**

| Tool | Role in Correlation | URL |
|---|---|---|
| **Maltego CE** | Visual relationship mapping, free community edition | maltego.com |
| **Spiderfoot** | Automated multi-source OSINT aggregation | spiderfoot.net |
| **OSINT Framework** | Curated tool directory by category | osintframework.com |
| **Obsidian** | Intelligence note-taking and relationship mapping | obsidian.md |
| **Hunchly** | Web investigation capture and logging (paid, worth it) | hunch.ly |

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*