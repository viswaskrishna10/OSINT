# **OSINT Investigation Report: Comparative Web Archive Analysis** 

---

## **1\. Executive Summary**

This investigation compares the Wayback Machine archival footprint of three organizations, chosen to represent three different points on the "archive density" spectrum: a young startup, an established company, and a massive, ancient nonprofit. The goal is to demonstrate how archive coverage differs by organization age, popularity, and crawl priority — and to practice the CDX API query patterns and historical-analysis techniques from this module.

Programmatic execution was blocked by sandbox/tooling constraints (logged in §7). This report documents the full methodology, target rationale, query design, ethics framework, and a structured data-collection template so the investigation can be completed and the findings section filled in once the data is gathered.

---

## **2\. Objective**

Compare capture volume, capture date range, and content-change frequency across three targets, and draw conclusions about what drives archive coverage differences.

| Target | Category | Founded | Why chosen |
| ----- | ----- | ----- | ----- |
| mistral.ai | Young AI startup | 2023 | Short history → tests how quickly a new domain gets picked up by crawlers |
| openai.com | Established, high-profile AI lab | 2015 | Mid-length history, very high public/media attention → likely dense capture record |
| wikipedia.org | Large, ancient, heavily-mirrored nonprofit | 2001 | Longest possible history among the three, extremely high traffic → stress-tests capture volume |

This spread lets us test a hypothesis: **archive density correlates with age × public attention × inbound links**, rather than any single factor alone.

---

## **3\. Background: How the Wayback Machine Decides What to Crawl**

Relevant context carried over from the theory module, restated here because it directly explains expected differences between the three targets:

* Crawl frequency depends on **popularity, submissions, and partnerships** — not a fixed schedule.  
* Sites well-linked from other sites are found and crawled more often; obscure or brand-new domains may go uncrawled for long stretches.  
* `robots.txt` and in-page meta directives can block or historically restrict crawling.  
* Because openai.com and wikipedia.org are old, high-profile, and heavily linked, they are expected to show dense, near-continuous capture history. mistral.ai, being younger and more niche (2023 launch), is expected to show a shorter history with possibly sparser gaps in the early months.

---

## **4\. Methodology**

### **4.1 Primary technique: CDX API sweep**

Query the Wayback Machine's CDX API for each target, with:

* `collapse=digest` — return only captures where content actually changed (compresses a long capture history down to the moments that matter)  
* `filter=statuscode:200` — exclude broken/error captures  
* `output=json` — machine-readable for counting and parsing

https://web.archive.org/cdx/search/cdx?url=mistral.ai\&output=json\&collapse=digest\&filter=statuscode:200  
https://web.archive.org/cdx/search/cdx?url=openai.com\&output=json\&collapse=digest\&filter=statuscode:200  
https://web.archive.org/cdx/search/cdx?url=wikipedia.org\&output=json\&collapse=digest\&filter=statuscode:200

### **4.2 Secondary technique: Calendar view (visual density check)**

For a quick visual cross-check against the CDX numbers:

https://web.archive.org/web/\*/mistral.ai  
https://web.archive.org/web/\*/openai.com  
https://web.archive.org/web/\*/wikipedia.org

Darker/denser clusters on the calendar heatmap \= more frequent captures in that period.

### **4.3 Tertiary technique: Manual diff**

For each target, open two snapshots several years apart and compare:

* Homepage messaging/positioning  
* Visual design/branding  
* Any product or leadership changes reflected in page content

### **4.4 Planned analysis metrics**

| Metric | What it tells us |
| ----- | ----- |
| Earliest capture date | How quickly the domain was discovered/crawled after launch |
| Most recent capture date | Whether the site is still actively crawled |
| Total digest-collapsed captures | Real change frequency (not just crawl frequency) |
| Gaps in capture history | Possible crawl deprioritization or robots.txt changes |
| Qualitative content shifts | Ties archive data to real-world company events |

---

## **5\. Ethics Framework Applied**

* **Necessity**: all three targets are public corporate/organizational homepages; no personal accounts, private data, or non-public pages are in scope. The research question (archive density comparison) specifically requires historical public homepage data — nothing broader is pulled.  
* **Proportionality**: scope is limited to homepage-level capture metadata and high-level content evolution. This is a much lighter-touch inquiry than, e.g., reconstructing a specific individual's deleted personal posts would be.  
* **Minimization**: if any archived snapshot incidentally surfaces PII (old staff names on a historical "About"/"Team" page), this report will note the *category* of exposure only — consistent with the standard set in the Mistral AI Digital Footprints investigation. No names, emails, or other identifiers will be reproduced here.

---

---

## **6\. Methodology Limitation (logged, not a target-side finding)**

Direct programmatic access to `archive.org` was not possible from the assistant's session:

* **Sandbox egress restriction** — `archive.org` is not in the assistant's allowed outbound domain list, blocking scripted CDX calls from the bash environment.  
* **Fetch-tool restriction** — the assistant's web-fetch tool only retrieves URLs that have already surfaced via a search result; it cannot fetch an arbitrarily-constructed API query string directly, even though the CDX syntax is fully public and requires no authentication.

Consistent with the standard set in the earlier Mistral AI exercise: this is recorded as a **tooling/environment limitation**, not a finding about any target. The queries in §4 are valid and can be run from any unrestricted browser or script.

---

## **7\. Incidental Finding: Archive Coverage Is Actively Degrading (2025–2026)**

Relevant, current context for the "Deleted Websites" and "Historical Analysis" topics in this module, surfaced during research around the above constraint:

* Multiple major news publishers have moved to **restrict or block Internet Archive crawlers**, over concerns that the Wayback Machine gives AI companies an unauthorized backdoor to scrape their content.  
* The **New York Times** confirmed it is "hard blocking" Internet Archive crawlers and added `archive.org_bot` to its `robots.txt` at the end of 2025\.  
* **The Guardian** has also moved to limit Internet Archive access, reportedly working directly with the Internet Archive on the change, though stopping short of a full block.  
* This contributed to an **87% drop in Wayback Machine captures of news publisher homepages** between May and October 2025\.  
* **Reddit** separately blocked the Internet Archive from crawling its forums in 2025\.  
* Internet Archive founder Brewster Kahle has publicly pushed back, arguing that publisher restrictions reduce public access to the historical record.

**Analytical implication for this curriculum:** when archived content for a news/media target can't be found, this should now be weighed as a plausible *publisher-side access restriction* — not automatically assumed to mean "never crawled" or "target deleted it." This is a good working example of why deleted-content investigations need to distinguish between three different causes:

1. Target removed the content  
2. Archive never captured it  
3. Archive was blocked from capturing/serving it

This finding does **not** directly affect mistral.ai, openai.com, or wikipedia.org (none are news publishers), but it's a relevant caveat to carry into any future exercise involving news-site archives.

---

