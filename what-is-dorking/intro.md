# What Is Dorking?

> Before we talk about Google dorking, we need to back up and talk about three
> things first: what hacking actually means, what web hacking is, and what
> Google hacking is. Most people skip straight to the syntax and miss the whole
> picture. We are not doing that here.

---

## Part 1 — What Does "Hacking" Actually Mean?

The word "hacking" has been ruined by movies.

If you have ever watched a movie where someone types furiously for 10 seconds,
green text flies across a black screen, and they say "I'm in" — that is not
hacking. That is Hollywood.

In the real world, **hacking simply means finding a way to use something
differently than it was intended to be used.** That is it. That is the whole
definition.

Here is a non-tech example most people can relate to:

> Imagine a theme park with a strict "no re-entry" policy. You buy a ticket,
> go in, and if you leave, you cannot come back. A hacker mindset looks at
> that rule and asks — *what if I never fully leave?* They prop the exit door
> open with a rock, step out, and step back in. The rule still technically
> exists. The system still thinks it is working. But the outcome is completely
> different from what was intended.

That is hacking. Finding the gap between how something is **supposed** to work
and how it **actually** works — and using that gap on purpose.

Hacking is not always illegal. It is not always malicious. A huge portion of
the security industry is made up of people who hack things professionally —
legally, ethically, and with permission — to find those gaps before the bad
guys do. They are called **penetration testers** (or pentesters), **red
teamers**, **security researchers**, and **ethical hackers**.

The tool does not make something hacking. The mindset does.

---

## Part 2 — What Is Web Hacking?

Now take that same mindset and point it at the internet.

The internet is made up of websites, web applications, APIs, servers, and
databases — all talking to each other constantly. Every single one of those
systems was built by humans. Humans make mistakes. Systems have gaps.

**Web hacking means finding and using those gaps in internet-based systems.**

This could be:

- A login page that lets you bypass the password entirely
- A web form that accepts commands it was never supposed to accept
- A server that exposes files it was never supposed to expose
- A search engine that reveals information the owner forgot was publicly visible

That last one — a search engine revealing things people forgot were public —
is exactly where Google dorking lives.

---

## Part 3 — What Is Google Hacking?

Google hacking is a term coined in the early 2000s by a security researcher
named **Johnny Long**. He noticed something that most people had completely
overlooked:

**Google is not just a search engine. It is the world's largest accidental
database of exposed information.**

Here is how that happens. When you build a website, Google sends out what are
called **crawlers** — also called spiders or bots. These are automated programs
that browse the internet constantly, reading every page they can find, and
indexing the content. Indexing means storing a reference to it so Google can
return it in search results.

The problem is that Google does not know what was *meant* to be public and
what was *accidentally* left public. It just indexes what it can reach.

So if a company accidentally leaves a folder of internal documents on their
web server without any password protection, Google will find it, index it, and
make it searchable. The company may have no idea. But anyone who knows how to
search for it can find it instantly.

Johnny Long started collecting these searches. He built a public database of
them called the **Google Hacking Database (GHDB)** — and the security world
has been using and building on it ever since.

> **Google hacking is the practice of using advanced Google search techniques
> to find information that is publicly exposed but not meant to be easily
> found.**

No breaking in. No malware. No exploits. Just search.

---

## Part 4 — What Is Dorking?

**Dorking is the hands-on practice of Google hacking.** The search strings you
build and run are called **dorks**.

The term comes from the slang word "dork" — originally used dismissively, as
in someone who does something clueless. The irony is that the people who got
called dorks for spending all their time figuring out how search engines work
turned out to be building one of the most powerful passive intelligence
techniques in the security industry.

A dork is a search query that uses **advanced operators** — special commands
built into search engines — to filter results with surgical precision.

Most people use Google like this:
```
best pizza near me
```

A person who knows dorking uses it like this:
```
site:companyname.com filetype:pdf "internal use only"
```

That second search is not magic. It is just a more precise instruction to
Google. It says: *only show me results from this specific website, only show
me PDF files, and only show me ones that contain the phrase "internal use
only."*

If that company accidentally uploaded internal documents to their public web
server, that search will find them. Google already indexed them. You are just
asking the right question.

---

## Why This Works — How Search Engines Index Everything

To really understand why dorking is so powerful, you need to understand what
search engine crawlers actually do.

Think of the internet like an enormous library — except nobody organized it.
Books are stacked everywhere, in every language, with no labels, no card
catalog, and new books appearing every second.

Google's job is to walk through that library constantly, read every book it
can find, and write down what is in each one so people can find things later.

The crawlers do not discriminate. They do not check whether the book was
*meant* to be in the library. If the door is open and the book is on a shelf,
they read it and index it.

This means that:

- Misconfigured web servers expose directory listings — Google indexes them
- Publicly accessible databases show their login pages — Google indexes them
- Documents uploaded to the wrong folder on a company server — Google indexes them
- Camera feeds, control panels, network devices left open — Google indexes them
- Internal wikis, staging environments, test pages left public — Google indexes them

None of this requires any technical skill to find once it has been indexed.
It just requires knowing how to ask Google the right question.

---

## The 1% Problem — Why Most People Use Almost None of What Is Available

Here is something that might surprise you.

The average person uses maybe **1% of what Google can actually do.**

Most people type a few words and scroll through the first page of results.
They do not know about advanced operators. They do not know that you can filter
by file type, by website, by date, by location, by words that appear in the
URL, by words that appear in the page title, or by dozens of other parameters.

This is not a criticism. Google deliberately keeps the basic search bar simple
because that is what most people need most of the time. But underneath that
simple bar is an extremely powerful filtering and retrieval system — and almost
nobody uses it.

Security researchers, OSINT investigators, law enforcement analysts, and
journalists who cover data breaches use these tools every day. They are not
doing anything the average person *cannot* do. They just know how to ask.

That is what this guide teaches.

---

## A Note Before You Continue

Everything in this guide is **passive reconnaissance**. That means we are
not touching any systems. We are not sending any traffic to any server. We are
not breaking in anywhere.

We are asking a search engine — which has already indexed publicly available
information — to show us what it found.

**Passive reconnaissance is legal.** Looking at information that is publicly
accessible is not a crime. However, what you *do* with that information can
absolutely cross legal and ethical lines. The
[Legal and Ethics](../legal-and-ethics/ethics.md) section of this guide covers
that in full — read it.

This guide exists to teach. The same techniques used to find exposed company
files are used by security teams to audit their own exposure, by journalists
to investigate stories in the public interest, and by law enforcement to locate
evidence in open investigations.

Knowledge is neutral. Use it responsibly.

---

## What Comes Next

Now that you understand what dorking is and why it works, here is where we go:

| Section | What You Will Learn |
|---|---|
| [Google Operators](../google/operators.md) | Every operator, real syntax, what works in 2026 |
| [Cybersecurity Dorks](../google/cybersecurity-dorks.md) | Finding exposed systems, files, credentials |
| [Social Media Dorks](../google/social-media-dorks.md) | People finding, email enumeration, profiles |
| [Advanced Combinations](../google/advanced-combinations.md) | Chaining operators, real world recipes |
| [Bing](../bing/bing.md) | What Bing finds that Google misses |
| [Yandex](../yandex/yandex.md) | Why OSINT professionals take Yandex seriously |
| [GitHub Dorking](../github-dorking/github.md) | Leaked credentials, API keys, internal tools |
| [Shodan](../shodan-dorking/shodan.md) | The search engine for exposed devices |
| [Pivoting](../what-next/pivoting.md) | What to do once you have information |
| [Cheat Sheet](../dork-cheatsheet/cheatsheet.md) | Everything on one page |
| [Legal and Ethics](../legal-and-ethics/ethics.md) | What is legal, what is not, responsible use |

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*