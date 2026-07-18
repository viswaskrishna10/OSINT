# **Day 7 — Advanced Google Dorking**

## **Target**

**Mistral AI** (mistral.ai) — previously investigated in the Digital Footprints module (Day 3-equivalent dork library exercise) and the Internet Archives module.

---

## **Queries Used \+ Purpose**

| \# | Query | Purpose |
| ----- | ----- | ----- |
| 1 | `site:mistral.ai filetype:pdf intitle:confidential OR intext:"internal use only"` | Surface exposed internal/confidential documents |
| 2 | `site:mistral.ai intitle:"index of" "parent directory" -inurl:blog` | Detect misconfigured/open directory listings, excluding blog noise |
| 3 | `site:mistral.ai inurl:admin OR inurl:login intitle:panel` | Locate login panels — **observe only, no interaction** |
| 4 | `site:mistral.ai filetype:env OR filetype:config OR filetype:log` | Chained filetype dork for exposed configs/secrets |
| 5 | `site:mistral.ai "error" "traceback" OR "stack trace" -intext:blog` | Surface server-side error disclosures |
| 6 | `related:mistral.ai` | Test the `related:` operator for associated sites |
| 7 | `site:mistral.ai filetype:pdf inurl:doc intitle:mistral intext:2025` | Full 5-operator chain (`site:` \+ `filetype:` \+ `inurl:` \+ `intitle:` \+ `intext:`) |
| 8 | `"mistral.ai" AROUND(3) "vulnerability" OR "breach"` | Proximity search to surface security-relevant mentions near the domain name |

---

## **Findings**

### **High-Confidence (Directly Observed)**

* **No open directory listings** on mistral.ai (query \#2). The only "index of" hits were false positives on unrelated sites.  
* **No exposed `.env`/`.config`/`.log` files** on mistral.ai itself (query \#4) — unlike Hugging Face's Spaces-hosted user content, Mistral's own domain doesn't host third-party uploads, so this category came back clean.  
* **No server-side error/stack-trace disclosure** from Mistral's own infrastructure (query \#5) — matches only referenced "stack trace" conceptually in product docs (Vibe Code's debugging feature).  
* **Login/admin surfaces are all standard, documented product features**, not misconfigurations: `console.mistral.ai` (developer console), `admin.mistral.ai` (Admin Panel, requires org/workspace admin role), `chat.mistral.ai` (consumer chat signup). All discovered incidentally via query \#3 — a good example of chained operators surfacing subdomain structure without a dedicated subdomain-enum query.  
* **A dedicated public Trust Center exists** at `trust.mistral.ai`, offering security documentation on request — this is intentional, structured disclosure, not an exposure (GHDB category 11 territory, but the "good" version of it).  
* **One published legal PDF** (French privacy policy, `mistral.ai/static/doc/fr-politique-de-confidentialite.pdf`) was returned by query \#1 — a legitimately public compliance document, not a leak. It happened to contain a visible DocuSign envelope ID in the source metadata, which is a harmless artifact and grants no access.  
* **A real, Mistral-confirmed security incident is publicly documented on their own site**: Mistral's official Security Advisories page (`docs.mistral.ai/resources/security-advisories`) discloses reference MAI-2026-002 — a supply-chain compromise (dubbed "Mini Shai-Hulud" by researchers) where malicious versions of Mistral's own npm and PyPI SDK packages were briefly published in May 2026 after a third-party dependency (TanStack) was compromised. Mistral's advisory states the affected npm packages were live for roughly three hours before removal, and that the incident stemmed from a compromised developer device rather than Mistral's core infrastructure. Multiple independent outlets (Microsoft threat intelligence via several security news sites) corroborate the technical details of the malware's behavior.

### **Inferred / Unverified (Reported But Not Independently Confirmed)**

* Several security news outlets report a threat group ("TeamPCP") claiming to have stolen roughly 450 internal Mistral repositories (\~5GB of source code) and is offering them for sale on a dark-web forum, threatening a free public leak if unsold. **This claim is disputed by Mistral** — reporting notes Mistral has confirmed only the SDK/package contamination described above, not the broader repository theft, and no data samples from the alleged larger breach have been published or verified as of the reporting date. This is flagged as **inferred, not high-confidence** — it's a claim under active dispute, not a directly observable technical finding from this exercise's own queries.

### **Tooling Notes**

* **`related:` (query \#6)** did not return genuinely related/similar sites — it returned a mix of Mistral's own pages and general news coverage, consistent with this operator being largely non-functional in modern search engines since \~2023–24 (noted in the Module 9 write-up).  
* **`AROUND(3)` (query \#8)** did appear to influence relevance/ranking toward proximity-matched results and was the query that surfaced the security-incident finding — the most functionally useful of the "new" operators tested today.  
* **`daterange:` and `cache:`** were not run as separate queries this session — both are effectively deprecated in current Google Search (Julian-date `daterange:` syntax is legacy and rarely honored; `cache:` was formally removed from Google in 2024). Noted here rather than run against a live query, consistent with the standing practice of logging tooling/operator limitations rather than presenting a non-functional query as if it produced a null finding.  
* **Wildcard `*`** was implicitly exercised via the `OR`\-style multi-term queries above but not tested as a standalone mid-phrase wildcard (e.g. `"mistral * breach"`); this is a gap for a future session if precision-phrase wildcarding becomes relevant.

---

## **What I Deliberately Didn't Pursue, and Why**

* **No login attempts** on any discovered panel (`console.mistral.ai`, `admin.mistral.ai`, `chat.mistral.ai`) — observation only, per the passive-OSINT boundary set in Module 9\.  
* **No further enumeration of the security-incident claim** — I did not attempt to locate, access, or verify the alleged stolen-repository data, the dark-web listing, or any of TeamPCP's claimed samples. That would cross from OSINT reconnaissance into interacting with stolen/illicit material, which is out of scope regardless of curiosity value.  
* **No attempt to fingerprint or probe** `console.mistral.ai` or `admin.mistral.ai` beyond noting their existence and stated purpose from documentation — no version detection, no parameter fuzzing.  
* **Category 8/9 dorking (`filetype:env` etc.) was not expanded or repeated** with variant syntax after the first clean pass, since a clean result doesn't warrant escalating query aggressiveness against the same target.

---

Mistral AI's own domain presents a **low-noise, well-maintained footprint**: no misconfigurations, no accidental file exposure, and login surfaces map cleanly to documented product features rather than forgotten legacy paths. This is a meaningfully different exposure profile from Hugging Face's (Day-prior session), where the platform's *user-generated content* (Spaces) created incidental exposure risk despite Hugging Face's own core infrastructure being clean — a useful comparative data point: **exposure risk scales with whether a platform hosts third-party uploads, not just with the maturity of the organization running it.**

The most consequential finding of the day didn't come from a misconfiguration dork at all — it came from a proximity-search query surfacing Mistral's *own* public security-advisory disclosure, plus independent news corroboration of a real supply-chain incident. This is a good illustration of why "advanced dorking" is as much about **finding what an organization has chosen to publish about itself** (trust centers, advisories) as it is about finding what they didn't mean to expose.

