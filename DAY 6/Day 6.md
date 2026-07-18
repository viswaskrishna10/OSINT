# **GitHub Mini Investigation** 

**Target:** GitHub, Inc.

---

## **Who owns it?**

GitHub is owned by **Microsoft** — acquired in 2018\. It still operates as its own legal entity, **GitHub, Inc.**, headquartered in San Francisco, CA, but Microsoft is the parent.

**Technical evidence:**

* GitHub's Terms of Service explicitly define Microsoft as an "Affiliate" under a \>50%-ownership control clause.  
* Microsoft's published affiliate list includes GitHub, Inc. alongside LinkedIn Corporation, Nuance Communications, and Microsoft Licensing GP — each with **separate registered agents**, so legal service/OSINT on one doesn't carry over to another.

---

## **Servers / tech setup**

GitHub's site resolves to one main server, and their DNS is deliberately split across two providers for resilience.

**Technical detail:**

| Record | Value |
| ----- | :---- |
| A | `140.82.114.4` |
| AAAA | none returned |
| MX | `0 github-com.mail.protection.outlook.com` (Microsoft Exchange Online) |
| NS | Dual-provider: `p08.nsone.net` (NS1) \+ `awsdns-*` (AWS Route 53\) simultaneously authoritative |
| SOA | `dns1.p08.nsone.net` |
| www | CNAME → `github.com` |
| TXT | Not retrieved — sandbox egress blocked (SPF/DKIM/DMARC unconfirmed) |

The MX pointing to `mail.protection.outlook.com` is direct evidence of Microsoft-managed corporate email infrastructure — not just a business relationship, but shared technical stack.

---

## **Domain registration (WHOIS)**

| Field | Value |
| ----- | :---- |
| Registrant Org | GitHub, Inc. |
| Registrant State/Country | CA, US |
| Registrar | MarkMonitor, Inc. |
| Created | 2007-10-09 |
| Last Updated | 2024-09-07 |
| Expires | 2026-10-09 |
| Status | clientDeleteProhibited / clientTransferProhibited / clientUpdateProhibited |

MarkMonitor is a corporate-defensive registrar used almost exclusively by large brands — it shields registrant contact PII behind a web-form gateway and layers on anti-hijacking transfer locks. No direct contact PII was exposed.

---

## **Security / bug bounty**

GitHub publishes a machine-readable `/.well-known/security.txt` file (RFC 9116 compliant):

Contact: https://hackerone.com/github  
Acknowledgments: https://hackerone.com/github/hacktivity  
Preferred-Languages: en  
Policy: https://bounty.github.com  
Hiring: https://github.careers

They run their vulnerability disclosure program through **HackerOne**, with a public bounty policy and researcher acknowledgments page — a standard, low-sensitivity structured source for gauging an org's VDP maturity.

---

## **What their job listings reveal**

Job boards are consistently the richest passive-collection source — same pattern as the Mistral exercise. Sampled listings on `github.careers` show:

* **Identity Core** team (Software Engineer II & Staff roles) — internal identity/auth platform group  
* **Git Systems** team (Staff Software Engineer) — core Git protocol/backend engineering  
* **Data Engineering** team (Senior Software Engineer)  
* Org structure spans: Engineering/Product/Design, Revenue (Sales/CS), Marketing & Communications, **CELA** (Corporate, External & Legal Affairs), Security & IT

Named internal teams like "Identity Core" give a passive read on platform architecture divisions without needing any direct access.

---

## **Website footprint (Certificate Transparency)**

Public CT logs (crt.sh) show GitHub's certificate history spans **800+ logged hostnames**. Rather than pull a full enumeration (which would exceed minimization for a mini exercise), a representative sample illustrates the pattern:

* `api.github.com` — REST/GraphQL API  
* `codespaces.github.com` — cloud dev environments  
* `classroom.github.com` — education product  
* `assets-cdn.github.com` — static asset delivery  
* `cloud.github.com`

The subdomain naming maps cleanly onto GitHub's public product lineup — useful for understanding scope without needing the full asset list.

---

## **Methodology limitations**

* TXT record lookup failed against both the default resolver and 1.1.1.1 — sandbox network egress timed out the query. Same class of limitation hit during the Mistral AI exercise. Logged as a **tooling constraint**, not a target-side finding.  
* Full CT log enumeration intentionally not pulled (minimization principle).

---

## **Bottom line**

Nothing sensitive or private was touched or reproduced. All findings came from public, structured sources: WHOIS, DNS, `security.txt`, job postings, ToS language, and certificate transparency logs.

