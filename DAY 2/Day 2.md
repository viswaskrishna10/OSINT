## **OSINT Investigation  Wikipedia**

---

### **1\. Planning & Scope**

**Target**: Wikimedia Foundation, Inc. (the nonprofit operating Wikipedia and sister projects)

 **Objective**: Build an organizational \+ technical infrastructure profile using passive OSINT techniques

 **Method**: Corporate records, official Wikimedia/Meta-wiki documentation, technical blog posts, news coverage

---

### **2\. Organizational Profile**

| Attribute | Finding |
| ----- | ----- |
| **Legal entity** | Wikimedia Foundation, Inc. — American 501(c)(3) nonprofit |
| **Founded** | June 20, 2003, in St. Petersburg, Florida, by Jimmy Wales |
| **HQ** | One Sansome Street, 18th floor, San Francisco — the fifth HQ location and fourth in San Francisco, under a lease running 2024–2028  |
| **Staff size** | Over 700 staff and contractors as of 2023, [Wikipedia](https://en.wikipedia.org/wiki/Wikimedia_Foundation) with roughly 650 managed across engineering, legal, and community liaison functions under the executive team  |
| **Finances** | Net assets of $255 million and an endowment surpassing $100 million as of 2023, funded mainly through millions of small donor donations plus grants and Wikimedia Enterprise services income  |
| **Leadership (current)** | Bernadette Meehan became CEO in January 2026, [Wikimedia](https://meta.wikimedia.org/wiki/Wikimedia_Foundation/Organizational_chart) succeeding Maryana Iskander, who announced in May 2025 her plan to depart by January 2026  |
| **Board** | 12-member Board of Trustees, including one permanent founder seat held by Jimmy Wales, five appointed expert seats, and six community/affiliate-elected seats |

**Notable current event**: The U.S. Congressional Oversight and Government Reform Committee opened an investigation into Wikipedia's operations in September 2025, initiated by two Republican representatives, examining alleged foreign-influence efforts within academically-funded institutions. This is the kind of finding a real OSINT report would flag as a live reputational/regulatory risk factor. 

---

### **3\. Technical Infrastructure (Digital Footprint)**

**Hosting model**: Unlike most large sites, Wikimedia does **not** rely on major cloud providers — it runs its own data centers with thousands of self-owned servers, a deliberate choice that let it stay online during the October 2025 AWS outage that took down countless cloud-dependent sites. 

**Data center footprint**: Servers are spread across seven data centers — three in the US (Virginia/Ashburn, Texas/Dallas, San Francisco), two in Europe (Amsterdam, Marseille), one in Asia (Singapore), and one in South America, with Brazil noted as the most recent addition.  

**DNS/routing**: Wikimedia uses gdnsd for geographic DNS to route requests to the nearest of its data centers, Linux Virtual Server (LVS) for load balancing, and an in-house failover system called PyBal. 

**Software stack** (classic LAMP-adjacent, fully open source): Varnish and Apache Traffic Server as caching proxies in front of Apache HTTP Server, all running on Debian GNU/Linux. Databases run on MySQL and MariaDB. Even AI/ML infrastructure uses AMD GPUs specifically because AMD ships an open-source software stack, avoiding proprietary Nvidia drivers. 

**CDN architecture**: The Wikimedia CDN (maintained by the SRE Traffic team) handles TLS/HTTP2 termination, rate limiting, and two-tier caching — an in-memory frontend layer and an on-disk backend layer. Server hostnames encode their location — e.g. "cp4043" identifies a caching node in San Francisco, "mw2393" an application server in Dallas — which is itself a small OSINT artifact if you ever see raw hostnames in headers or logs. 

**Multi-datacenter write/read split**: Wikimedia spent roughly seven years migrating to true multi-datacenter concurrency, finally achieved in 2022 — write (POST) requests always route to the primary datacenter while read (GET) requests route to whichever datacenter is geographically closest. 

---

### **4\. What This Teaches About OSINT Methodology**

1. **Self-disclosed technical documentation is a goldmine.** Most companies don't publish this much about their own infrastructure. Wikimedia does, via Wikitech — a reminder to always check if a target maintains public engineering blogs, status pages, or wikis before resorting to active recon.  
2. **Non-standard hosting is itself an OSINT finding.** Most modern orgs are AWS/GCP/Azure-hosted, so "which AS/cloud provider" is usually your first pivot. Wikimedia running its own colocation facilities is atypical and would change your investigative approach (no single cloud-provider subpoena target, no shared cloud misconfig risk class, etc.).  
3. **Hostname naming conventions leak topology.** The `cp4043` / `mw2393` example is a textbook case of "reverse-engineering internal structure from something the org never meant to be a public data point."

### **1\. DNS Report** 

I looked up Wikipedia's DNS records. Here's what each one means in plain terms:

| What I checked | What I found | In simple words |
| ----- | ----- | ----- |
| IP address | `208.80.153.224` | This is Wikipedia's "street address" on the internet |
| Nameservers | `ns0/ns1/ns2.wikimedia.org` | Wikipedia runs its own phonebook service instead of using someone else's (like GoDaddy or Cloudflare) |
| Mail servers | `mx-in1001` & `mx-in2001.wikimedia.org` | They also run their own email servers, not Gmail or Outlook |
| Ownership proof | Google \+ Yandex verification codes | Confirms Wikipedia has officially claimed this domain on both Google and the Russian search engine Yandex |
| `en.wikipedia.org` and `www.wikipedia.org` | Same IP as the main domain | All versions of Wikipedia point to the same front door — there's no separate server just for the English version |

---

### **2\. URL Breakdown (simple)**

Example link: `https://en.wikipedia.org/wiki/Open-source_intelligence#History`

| Piece | Value | Meaning |
| ----- | ----- | ----- |
| `https` | Secure connection | Data is encrypted between you and Wikipedia |
| `en` | Subdomain | Tells you it's the English version |
| `wikipedia.org` | Main domain | The actual website |
| `/wiki/Open-source_intelligence` | Path | The specific article — Wikipedia's software (MediaWiki) always uses `/wiki/` for articles, so seeing that pattern instantly tells you what software runs the site |
| `#History` | Fragment | Jumps straight to the "History" section on the page — this part never gets sent to Wikipedia's server, it only works in your browser |

---

### **3\. Website Architecture** 

* Wikipedia doesn't use Amazon, Google, or Microsoft's cloud — they run and own their **own servers** in data centers across the US, Europe, Asia, and now South America.  
* All traffic goes through one shared "front door" (that IP address), which then figures out where to send it based on which Wikipedia version you asked for.  
* Their servers have readable names like `text-lb.codfw` — you can literally decode what a server does and where it is just from its name.  
* Reading/browsing requests go to whichever data center is closest to you. Editing/saving requests always go to one main data center, to keep everything in sync.

