# **Digital Footprint Report: Mistral AI**

---

## **1\. Executive Summary**

Mistral AI is a French artificial intelligence company headquartered in Paris, founded in April 2023 by three former Google DeepMind / Meta researchers. It has grown into one of the most heavily funded AI startups outside the US, with a reported valuation in the $13–14B range as of late 2025/2026. This report documents its footprint across four categories: organizational/corporate, technical infrastructure, personnel, and online identity coherence.

---

## **2\. Organization Footprint**

### **2.1 Business / Administrative Footprint**

| Attribute | Finding | Source |
| ----- | ----- | ----- |
| Legal name | Mistral AI (Société par actions simplifiée / SAS) | Legal notice, verif.com |
| Registration number (SIREN) | 952 418 325 | French RCS / Annuaire des Entreprises |
| Registered address | 15 Rue des Halles, 75001 Paris, France | Legal notice, ToS |
| Alternate address (LEI record) | 21 Rue Tandou, 75019 Paris | LEI Lookup, job boards |
| LEI | 894500Q9JNDV9YXSOL64 | LEI Lookup |
| NAF/APE code | 8211Z (Combined office administrative service activities) | verif.com |
| Founded | April 2023 | Wikipedia, multiple press sources |
| Founders | Arthur Mensch (CEO), Guillaume Lample (Chief Science Officer), Timothée Lacroix (CTO) | Company "About" page |
| Founding background | \<cite index="4-1"\>Mensch is a former Google DeepMind employee, while Lample and Lacroix previously worked on large-scale AI models at Meta Platforms; all three met as students at École Polytechnique\</cite\> | Wikipedia |
| Valuation trajectory | \<cite index="4-1"\>Roughly $2B in December 2023, rising to $6.2B by June 2024, reportedly in talks for a $10B valuation by August 2025, then a €2B raise reported in September 2025 valuing the company near $14B\</cite\> | Wikipedia (aggregating FT/Bloomberg reporting) |
| Total funding | $3.05B across 8 rounds (per Tracxn) | Tracxn |
| Notable shareholders | Founders retain \~39% economic equity but majority voting control via dual-class shares; ASML holds the largest single external stake (\~11%) following a late-2025 round; Bpifrance (French state investment bank) has held a stake since seed | LegalClarity |
| Recent M\&A | Acquired Koyeb (Paris-based infra startup) in Feb 2026; acquired Emmi AI (Austria, industrial simulation AI) in May 2026 | Wikipedia |
| Notable partnerships | Microsoft (Azure), CMA CGM (€100M, April 2025), Accenture (enterprise AI, Feb 2026\) | Wikipedia |
| EU lobbying registration | Registered as an interest representative on the EU Transparency Register, tracking AI Act implementation and related digital policy dossiers; discloses public-affairs staff allocations | LobbyFacts.eu |

### **Methodology note:** Corporate registration data was cross-validated across three independent sources (official legal notice, LEI registry, and the Verif business directory) — all three agree on the SIREN number and core registered address, which increases confidence in the finding. The LEI record's differing address (Rue Tandou vs. Rue des Halles) likely reflects a secondary office/campus rather than a discrepancy, since job postings also reference the Rue Tandou address as a work location.

### **2.2 Technical Footprint (Live DNS Investigation)**

Conducted live DNS queries against `mistral.ai` and common subdomains, consistent with prior methodology used in the Wikipedia infrastructure exercise:

**Nameservers:** `ada.ns.cloudflare.com`, `ivan.ns.cloudflare.com` — domain and DNS management sit behind Cloudflare.

**Root domain (`mistral.ai`):**

* A records: `162.159.142.207`, `172.66.2.203` (Cloudflare edge IP ranges)  
* AAAA records: `2606:4700:7::2c3`, `2a06:98c1:58::2c3` (also Cloudflare)  
* Reverse DNS on both IPs returned NXDOMAIN — expected behavior when a domain sits behind a CDN/reverse proxy, since the resolvable IP belongs to Cloudflare's edge network rather than Mistral's own infrastructure.

**MX records (mail routing):** A mixed setup —

* `aspmx.l.google.com` and related Google MX hosts (Google Workspace for primary mail)  
* `mailstream-*.mxrecord.io` entries at higher priority (lower preference number \= higher priority), suggesting a third-party mail security/filtering gateway sits in front of or alongside Google Workspace mail flow.

**Subdomain enumeration:**

| Subdomain | Result | Interpretation |
| ----- | ----- | ----- |
| `api.mistral.ai` | Same Cloudflare IPs as root | API traffic also fronted by Cloudflare |
| `docs.mistral.ai` | `216.150.16.129`, `216.150.1.129` | Different IP range — likely a separate CDN/host for documentation (possibly a docs-platform vendor) |
| `console.mistral.ai` | `141.101.90.104–107` | Cloudflare range, distinct pool from root domain |
| `chat.mistral.ai` | Same 141.101.90.x pool as console | Le Chat / "Vibe" product likely shares infra with the console |
| `www.mistral.ai` | Same as root | — |
| `status.mistral.ai` | Same as root | Status page resolves but may be a placeholder/redirect rather than a dedicated status service |
| `app.mistral.ai` | No A/CNAME found | Not in active use, or uses a different resolution method not tested |

**Methodology note:** A direct HTTP header fetch (to inspect server banners, CMS fingerprints, or security headers) was attempted but blocked by this environment's network egress policy (`host_not_allowed`) — this is an environment limitation on my end, not a finding about Mistral's infrastructure, and is logged here per standard practice of documenting blocked queries.

### **2.3 Human / Personnel Footprint**

* **Leadership team** is publicly named on the company's own "About" page: Arthur Mensch (CEO), Guillaume Lample (Chief Science Officer), Timothée Lacroix (CTO) — all three maintain active, public LinkedIn presences.  
* Arthur Mensch's LinkedIn activity shows a pattern typical of a scaling startup CEO: government/diplomatic engagement (e.g., partnership announcements involving Luxembourg's government), product launch announcements (Agents API, reasoning models), and enterprise partnership signaling (Azure, Accenture).  
* **Data broker exposure:** A commercial contact-data aggregator (ContactOut) has indexed a likely personal and a likely corporate email address format for the CEO, illustrating how executive contact information propagates to third-party data brokers independent of what the company itself publishes — a classic example of "passive footprint" distinct from what the subject actively discloses.  
* **Job postings as personnel/org-structure intelligence:** Public job boards (Welcome to the Jungle, Built In, ZipRecruiter) collectively reveal:  
  * Specific internal team/product names: **"Mistral Cloud"**, **"Mistral Compute"**, **"La Plateforme"**, **"Mistral Code"**  
  * A dedicated **cybersecurity function** with distinct sub-roles: Offensive Security, Incident Response Lead, SOC Analyst, DevSecOps — indicating an internally staffed security team rather than fully outsourced security operations  
  * Technology stack signals: **Golang** explicitly named for a "Backend Software Engineer (Mistral Cloud, Golang)" role; broader postings reference Kubernetes/Docker for production AI deployment  
  * Organizational scale signal: one aggregator listed **176 open roles** as of a July 2026 snapshot, spanning research, infra, GTM, HR, and legal/compliance (e.g., a dedicated GDPR compliance role) — indicating a company past early-stage headcount and building out full corporate functions

---

## **3\. Online Identity Coherence**

Mistral AI's online identity is unusually consolidated and low-ambiguity for OSINT purposes:

* **Single canonical domain** (`mistral.ai`) used consistently across legal notices, product hosting, and marketing — no evidence of parallel or historical domains needing disambiguation.  
* **Leadership identity is unified**: the three founders use consistent, real-name LinkedIn identities tied directly to the company's own "About" page — no sock puppet or anonymized-founder pattern, consistent with a company courting institutional investors and government partnerships where identity transparency is an asset rather than a liability.  
* **Brand naming consistency**: product names (Le Chat, recently rebranded "Vibe"; Mistral Compute; Mistral Code) are consistently referenced across the company's own site, press coverage, and job postings — no naming drift that would complicate tracking the product ecosystem.

---

## **4\. Metadata**

* Hosting/CDN metadata via DNS functions as a form of infrastructure metadata even without direct header access.  
* Legal notices on the site itself (a required disclosure under French law, per Article 6(I)(1) of LCEN 2004-575) function as a structured metadata source unique to French-registered companies — this is a useful category to remember for future investigations of French entities, as it reliably surfaces registration number, capital, and named legal representative without needing a paid registry lookup.

---

## **5\. Ethical / Legal Notes**

* All data in this report was drawn from sources the subject (Mistral AI, its founders, and job-board operators) either published directly or knowingly submitted to public registries.  
* The CEO's data-broker-indexed contact information is noted only at the category level (i.e., that such exposure exists) rather than reproduced in full, consistent with minimization principles — the finding is the *pattern* (corporate data brokers aggregate executive contact info independent of official channels), not the specific data point.  
* No attempt was made to access non-public systems, authenticate to any service, or circumvent access controls (the blocked HTTP header fetch was a passive, non-adversarial request that failed due to this environment's own allowlist, not an attempt to bypass Mistral's security).

---

