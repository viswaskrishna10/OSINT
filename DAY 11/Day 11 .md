# **Day 11 — Dork Playbook**

---

## **1\. Purpose**

This playbook consolidates every search operator, dork pattern, and technique validated across Days 3 and 7–10 into one categorized, tag-based reference. Instead of re-deriving queries per target, this file is the lookup point: pick a category, pick a use case, adapt the pattern to the new target domain.

**Source modules folded into this playbook:**

| Day | Module | Contribution |
| ----- | ----- | ----- |
| 3 | Search Engine Mastery | Boolean logic, core Google operators, 50-query library |
| 7 | Advanced Google Dorking | Chained operators (Hugging Face, Mistral AI) |
| 8 | Digital Footprints | Personal/org footprint queries (Mistral AI) |
| 9 | Internet Archives | Wayback/CDX-adjacent discovery queries |
| 10 | GHDB Structured Dorking | GHDB category mapping (Hugging Face) |

---

## **2\. Ethics Note (applies to every query below)**

* **Necessity** — only run a dork if it answers a specific investigation question.  
* **Proportionality** — favor category-level/pattern-level results over deep enumeration.  
* **Minimization** — never harvest, store, or reproduce PII or live credentials. If a query surfaces exposed secrets, log the *category* ("exposed `.env` file found") and stop — do not open, copy, or paste contents.  
* All queries below are **passive** (search-engine indexed data only). None involve scanning, exploitation, or direct interaction with target systems.

---

## **3\. How to use this playbook**

1. Pick a **tag** below matching your current goal (`recon`, `files`, `credentials`, `infra`).  
2. Copy the **pattern**, swap `TARGET` for the domain/org name.  
3. Log the adapted query, result count, and a Confirmed/Inferred label in your module's query log table  
4. If a query returns nothing, note it as a negative finding — that's still signal (e.g., "no indexed `.sql` files" suggests decent server hygiene).

---

## **4\. Playbook by Category**

### **4.1  RECON — General footprint & scope mapping**

*Goal: map what exists before drilling into anything specific.*

| Pattern | Tag | Use Case | Notes |
| ----- | ----- | ----- | ----- |
| `site:TARGET.com` | recon | Full indexed footprint | Baseline — always run first |
| `site:TARGET.com -www` | recon | Non-www subdomains/pages | Surfaces subdomains Google indexed outside the main host |
| `site:*.TARGET.com` | recon | Subdomain enumeration | Passive alternative to Censys subdomain view |
| `site:TARGET.com inurl:careers OR inurl:jobs` | recon | Job postings | **Richest single source** across Mistral AI & GitHub exercises — reveals internal product names, stack, team structure |
| `site:linkedin.com/company/TARGET` | recon | Org structure, headcount, employee roles | Cross-reference with careers page |
| `site:TARGET.com intitle:"about" OR intitle:"team"` | recon | Org/leadership info | Pattern-level only — no PII capture |
| `"TARGET" -site:TARGET.com` | recon | Third-party mentions, press, mirrors | Good for discovering unofficial footprints |
| `site:github.com "TARGET"` | recon | Public repos, forks, leaked configs in repo history | Cross-check against official GitHub org page |

---

### **4.2 FILES — Document & file discovery**

*Goal: surface publicly indexed files that reveal structured intelligence.*

| Pattern | Tag | Use Case | Notes |
| ----- | ----- | ----- | ----- |
| `site:TARGET.com filetype:pdf` | files | Whitepapers, reports, policies | Validated on Mistral AI (Section 4 of prior report) |
| `site:TARGET.com filetype:pdf "internal use only"` | files | Sensitivity-marked docs | Log category only if found — do not open/quote sensitive content |
| `site:TARGET.com filetype:docx OR filetype:pptx` | files | Drafts, internal presentations | Often left indexed accidentally |
| `site:TARGET.com filetype:xlsx OR filetype:csv` | files | Data exports, structured datasets | Check for accidental PII exposure — category-log only |
| `site:TARGET.com filetype:log` | files | Server/application logs | GHDB "Files Containing Juicy Info" category |
| `site:TARGET.com filetype:xml OR filetype:json inurl:config` | files | Exposed config files | Overlaps with credentials category below |
| `site:TARGET.com intitle:"index of"` | files | Open directory listings | Classic GHDB pattern — validated conceptually on Hugging Face module |
| `site:TARGET.com "confidential" filetype:pdf` | files | Explicitly marked sensitive PDFs | High caution — minimization applies |

---

### **4.3  CREDENTIALS — Exposure & sensitive-data patterns**

*Goal: identify whether secrets/credentials are inadvertently indexed. Never enumerate contents — category-level logging only.*

| Pattern | Tag | Use Case | Notes |
| ----- | ----- | ----- | ----- |
| `site:TARGET.com filetype:env` | credentials | Exposed environment files | GHDB "Sensitive Directories" adjacent |
| `site:TARGET.com inurl:wp-config.bak` | credentials | Backup config exposure | Legacy CMS pattern, still worth checking |
| `site:TARGET.com "BEGIN RSA PRIVATE KEY"` | credentials | Exposed private keys | If it hits: log "private key material indexed" — do not view/copy |
| `site:pastebin.com "TARGET.com"` | credentials | Leaked credential dumps / paste exposure | Third-party paste-site pattern from Module 10 |
| `site:TARGET.com inurl:admin intitle:"login"` | credentials | Exposed admin login panels | Confirms panel exists; does not confirm compromise |
| `site:github.com "TARGET" "api_key" OR "secret"` | credentials | Hardcoded secrets in public repos | Common in job-posting/GitHub cross-reference work |
| `site:TARGET.com filetype:sql` | credentials | Exposed database dumps | GHDB "Files Containing Passwords" category |
| `intext:"password" site:TARGET.com filetype:log` | credentials | Credentials leaked into logs | Category-log only, never reproduce values |

---

### **4.4  INFRASTRUCTURE — Technical & hosting intelligence**

*Goal: understand hosting, DNS, and technical stack (Google-side only — full infra work belongs to Shodan/Censys modules).*

| Pattern | Tag | Use Case | Notes |
| ----- | ----- | ----- | ----- |
| `site:TARGET.com inurl:docs OR inurl:developers` | infra | API/developer documentation | Reveals tech stack, SDKs, integration points |
| `site:TARGET.com "powered by"` | infra | CMS/platform fingerprinting | Weak signal alone; corroborate elsewhere |
| `site:TARGET.com inurl:status` | infra | Status pages, uptime dashboards | Sometimes reveals infra provider names |
| `site:status.TARGET.com` | infra | Dedicated status subdomain | Common SaaS pattern |
| `"TARGET.com" site:certificatetransparency.dev` OR CT log search | infra | Certificate transparency records | Passive companion to Censys certificate history work |
| `site:TARGET.com inurl:swagger OR inurl:api-docs` | infra | Exposed API documentation/specs | May reveal internal endpoint naming |
| `site:TARGET.com inurl:.well-known` | infra | Security.txt, standard metadata files | Legit, low-risk recon target |
| `site:TARGET.com "legal notice" OR "mentions légales"` | infra | Jurisdictional/registry metadata | **EU-quirk**: French legal notice pages are reliable structured metadata (validated on Mistral AI — SIREN, LEI-type identifiers) |

---

## **5\. Cross-Tool Complement Map**

Google dorking is one leg of the stool. Per the multi-tool investigation on Mistral AI, here's how dork categories above map to non-Google tools for the same question:

| Question | Google Dork Category | Complementary Tool |
| ----- | ----- | ----- |
| What subdomains exist? | Recon (`site:*.TARGET.com`) | Censys (subdomain \+ DNS relationships) |
| What's exposed on the network? | *(not covered by Google)* | Shodan (open ports, banners, SSL) |
| What's the certificate history? | *(not covered by Google)* | Censys (historical certs, infra changes) |
| What files/docs are public? | Files | Wayback/CDX (historical versions of same files) |
| What's the tech stack? | Infra | Shodan (server fingerprints) \+ job postings |
| Are credentials exposed? | Credentials | GitHub code search, paste-site monitoring |

**Takeaway validated across modules:** Google indexes *content*; Shodan/Censys index *infrastructure*; Archives index *history*. A complete picture requires all three.

---

## **6\. Standard Query Log Format (for future modules)**

When applying any pattern from this playbook to a new target, log it as:

| GHDB Category | Original Pattern | Adapted Query | Result Count | Confirmed/Inferred |
| ----- | ----- | ----- | ----- | ----- |
| e.g. Files Containing Juicy Info | `filetype:pdf` | `site:example.com filetype:pdf` | 12 | Confirmed |

