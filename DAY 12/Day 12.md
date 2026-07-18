## Day 12 — Fortune 500 OSINT Investigation Report

Target: JPMorgan Chase & Co

Scope: Full-spectrum open-source investigation applying every technique from Week 2 (search operator mastery, advanced/GHDB dorking, digital footprint mapping, corporate OSINT, internet fundamentals/DNS reasoning, archive-aware methodology) to a single assigned target.

## 1. Ethics & Scope Statement

This investigation used only publicly available, passively indexed information: search engine results, SEC/regulatory filings, public code repositories, and vendor-published disclosure pages. No active scanning, authentication attempts, or social engineering occurred. No PII, credentials, or non-public data were sought. Consistent with prior modules, any sensitive category (e.g. breach-related PII counts) is reported at the aggregate/category level as published by primary sources — never reproduced in detail beyond what the source itself discloses in summary form. Principles applied throughout: necessity, proportionality, minimization.

## 2. Corporate OSINT

| Attribute | Value | Classification |
| --- | --- | --- |
| Legal name | JPMorgan Chase & Co. | Confirmed |
| SEC CIK | 0000019617 | Confirmed |
| EIN | 13-2624428 | Confirmed |
| SIC code | 6021 (National Commercial Banks) | Confirmed |
| State of incorporation | Delaware | Confirmed |
| Legal Entity Identifier (LEI) | 8I5DZWZKVSZI1NUHU748 | Confirmed |
| Registered agent address | Corporation Trust Center, Wilmington, DE 19801 | Confirmed |
| HQ address | 383 Madison Avenue, New York, NY 10179 Confirmed |   |


| Attribute | Value | Classification |
| --- | --- | --- |
| Related legal entity: JPMorgan Chase Bank, N.A. | LEI 7H6GLXDRUGQFU57RNE97, CIK 0000835271, HQ Columbus, OH | Confirmed |
| Related legal entity: JPMorgan Chase Financial Co. LLC | Separate SEC CIK 1665650, used as issuer for structured products | Confirmed |

Note: LEI/CIK cross-referencing (a technique flagged in prior modules for EU jurisdictional quirks like French legal-notice pages) applies equally well to US financial issuers — the multi- entity CIK structure (parent co. vs. financing subsidiary vs. national bank charter) is itself a useful map of how the group segments legal liability and regulatory reporting.

## 3. Digital Footprint & Domain Ecosystem

Domains and subdomains confirmed via search-engine indexing (not active DNS enumeration, per sandbox constraint — see §7):

- jpmorganchase.com  primary corporate site

- careers.jpmorgan.com  recruiting/careers portal (separate subdomain namespace from main corp domain — worth noting as a common enterprise pattern of outsourcing HR tooling to a distinct host)

- chase.com  consumer banking brand

- jpmorgan.com  commercial/investment banking brand (J.P. Morgan)

- jpmorganindices.com  separate branded property for investable indices products

- jpmorganchaseinstitute.com (referenced via JPMorganChase Institute research arm)

- responsibledisclosure.jpmorganchase.com  security vulnerability disclosure portal, hosted on third-party platform (Synack-powered)

- github.com/jpmorganchase and github.com/jpmorgan-payments  two distinct GitHub organizations

Pattern observation: the brand/subdomain split (Chase = retail, J.P. Morgan = commercial/investment, JPMorganChase = corporate umbrella) mirrors the firm's actual line-of-business structure. This is consistent with the "digital footprint reflects org structure" principle established in the Mistral AI module.

## 4. Search Operator & GHDB-Style Dorking Findings


Full query log for the timed competition portion lives in day12_dork_competition_results.md. Findings expanded here:

| Category Query |   | Result | Classificatio n |
| --- | --- | --- | --- |
| Public PDFs (financial disclosure) | site:jpmorganchase.com filetype:pdf | 10-K filings (2023, 2024), quarterly earnings supplements through 2Q26, shareholder letters back to 2008, Policy Center reports | Confirmed |
| Exposed directory listings | site:jpmorganchase.com intitle:"index of" | None found — no genuine Index of / pages indexed | Confirmed (negative finding) |
| Job postings (tech stack intel) | m software engineer | Confirms tech stack: Python, Java, React, Node.js, Docker, Kubernetes; site:careers.jpmorgan.co confirms AI-assisted internal dev tooling; confirms hiring hubs incl. Jersey City, Plano TX, Columbus OH, Glasgow (UK apprenticeship/tech centre) | Confirmed |
| Vulnerabilit program | y disclosure Direct search | Program hosted at responsibledisclosure.jpmorganchase.c om, third-party managed (Synack), safe- harbor language present, no public bounty payout table (VDP-style, not a paid bug bounty) | Confirmed |

GHDB category mapping (per Module 10 convention): Files Containing Juicy Info → none found beyond expected public financial disclosure; Vulnerable Servers / Sensitive Directories → none found; Advisories and Vulnerabilities → none found on primary domain (as expected — a mature financial institution's production infrastructure is not meaningfully dork-discoverable, which is itself the finding).

## 5. Technology & Infrastructure Signals (via GitHub)

JPMorganChase operates two distinct public GitHub organizations:

- jpmorganchase - general open-source output. Notable projects: salt-ds (React design system, TypeScript, Apache-2.0, 195+ stars), fusion/dataquery-sdk (Python SDKs for internal market-data platform APIs), mosaic, QOKit (quantum computing toolkit), nbcelltests/jupyter-fs (Jupyter tooling), abides-jpmc-public (agent-based


market simulation). The firm is a FINOS platinum member and contributes to/maintains FINOS projects including Perspective (analytics/visualization).

- jpmorgan-payments  payments-specific. Includes pdp-mcp, a reference MCP (Model Context Protocol) server for the JPMorgan Payments Developer Portal, updated within the current month at time of research — a concrete, dated signal that the firm is building AI-agent-facing developer tooling now, not just legacy infrastructure.

Inferred tech stack (cross-referencing job postings + GitHub): Python, Java, TypeScript/React, C++, Node.js, Docker, Kubernetes, cloud-native architecture, quantum computing R&D (with QC Ware), applied AI/LLM tooling (CodeQUEST — LLM-based code quality framework).

## 6. Security Posture & Incident History

|   | Date Incident | Nature | Source Type | Classification |
| --- | --- | --- | --- | --- |
| 2014 | Large-scale customer data breach (~76M households) | Names, emails, addresses, phone numbers exposed; financial/login data reported not compromised | Wikipedia (well- documented, widely cited) | Confirmed |
| 2023 | MOVEit third- party vendor exposure | ~451,000 retirement-plan participants' data exposed via vendor Pension Benefit Information LLC (a widescale, multi-company MOVEit zero-day incident, not a JPM-side network compromise) | Regulatory filing (Maine AG), trade press | Confirmed |
| Jan 2026 | Law firm (Fried Frank) data exposure affecting client | Third-party law-firm vendor incident, disclosed alongside a similar Goldman Sachs disclosure | Trade press (SecurityWeek) | Confirmed |

Pattern: all incidents post-2014 in the public record are third-party/vendor-side exposures, not direct compromises of JPM's own perimeter — consistent with the dork sweep in §4 finding no exposed infrastructure on the primary domain. This vendor-risk pattern is worth carrying forward as a standing observation for future financial-sector targets, alongside the existing "French legal notice" and "archive crawler blocking" standing notes from earlier modules.


Responsible disclosure: JPMorgan Chase is cited by HackerOne as one of the minority of financial-sector Global 2000 firms with a public vulnerability disclosure policy (alongside American Express, Citigroup, ING, TD Ameritrade) — a positive security-maturity signal.

## 7. Methodology Limitations

- No live DNS enumeration (TXT/subdomain brute force) or Certificate Transparency (crt.sh) query was performed directly — this session's sandbox network egress is restricted to package-registry domains only (npm/pip/GitHub code hosting), and crt.sh was not reachable via the fetch tool because no crt.sh URL had already surfaced in-session (fetch is restricted to previously-seen or user-provided URLs). This is a tooling constraint, not a target-side finding — subdomain discovery here relied entirely on search-engine indexing rather than raw CT-log queries, which likely undercounts the true subdomain footprint.

- Wayback Machine / CDX API queries were not run this session; if a full archival comparison is wanted for JPMorgan Chase properties, that would be a natural follow- up mirroring the Module-11-style exercise already partially completed for mistral.ai/openai.com/wikipedia.org.

- All findings are search-engine-mediated rather than raw Google dork syntax against google.com directly; operator support (site:, filetype:, intitle:) was respected by the search backend used, but ranking/result-count behavior may differ from a live Google session.

## 8. Summary

JPMorgan Chase presents a large, well-governed digital footprint: clean primary-domain dork surface (no exposed directories, no loose sensitive files), a mature and long-standing public vulnerability disclosure program, two active GitHub organizations spanning design systems to quantum computing to emerging AI-agent tooling, and a multi-entity corporate/legal registration structure that mirrors its retail/commercial/investment banking lines of business. The one recurring risk pattern across its public incident history is third- party vendor exposure rather than direct infrastructure compromise — a useful standing observation for future financial-sector OSINT targets.
