# **Google Dorks Library — Document & File Discovery**

### **| Search Engine Mastery Module**

**Purpose**: A reference library of 50 Google dork queries for locating publicly indexed PDFs, spreadsheets, research papers, and government documents during OSINT investigations.

**How to use this library**: Replace bracketed placeholders (e.g., `[TARGET]`, `[TOPIC]`, `[ORG]`) with your investigation's actual entity, keyword, or domain. Document which queries you run, against which target, and what they surface — consistent with the reproducibility standard from the Search Strategies module.

**Ethical note**: Every query below only surfaces content Google has already indexed and made publicly retrievable. Locating a document via these dorks is passive reconnaissance. What you do with a document once found (redistribution, use of any personal/sensitive data inside it) is still governed by the necessity, proportionality, and minimization principles from your ethics module.

## **Category 1: PDFs (Queries 1–13)**

1. `filetype:pdf "[TARGET]"`  
2. `site:[TARGET DOMAIN] filetype:pdf`  
3. `filetype:pdf intitle:"[TOPIC]"`  
4. `filetype:pdf "confidential" "[ORG]"`  
5. `filetype:pdf "internal use only"`  
6. `filetype:pdf "do not distribute" [TOPIC]`  
7. `filetype:pdf inurl:report [ORG]`  
8. `filetype:pdf "annual report" [ORG] 2023..2025`  
9. `filetype:pdf "org chart" [ORG]`  
10. `filetype:pdf "employee handbook"`  
11. `filetype:pdf "meeting minutes" [ORG]`  
12. `filetype:pdf site:linkedin.com "[TARGET]"`   
13. `filetype:pdf "press release" [ORG] site:[ORG DOMAIN]`

## **Category 2: Excel Sheets & Spreadsheets (Queries 14–25)**

14. `filetype:xlsx "[TARGET]"`  
15. `filetype:xls "budget" [ORG]`  
16. `filetype:xlsx "contact list"`  
17. `filetype:xlsx "employee list" [ORG]`  
18. `filetype:csv "[TARGET]" email`  
19. `filetype:xlsx inurl:finance [ORG]`  
20. `filetype:xls "salary" OR "compensation" [ORG]`  
21. `filetype:xlsx "password" -site:github.com`  
22. `filetype:xlsx "inventory" [ORG]`  
23. `filetype:csv "database export"`  
24. `filetype:xls "vendor list" [ORG]`  
25. `filetype:xlsx "project tracker" [ORG]`

## **Category 3: Research Papers & Academic Content (Queries 26–37)**

26. `filetype:pdf site:scholar.google.com "[TOPIC]"`  
27. `"[TOPIC]" filetype:pdf site:researchgate.net`  
28. `filetype:pdf intitle:"abstract" "[TOPIC]"`  
29. `filetype:pdf "working paper" [TOPIC]`  
30. `site:arxiv.org "[TOPIC]"`  
31. `filetype:pdf "peer reviewed" [TOPIC]`  
32. `filetype:pdf site:ncbi.nlm.nih.gov "[TOPIC]"`  
33. `"[AUTHOR NAME]" filetype:pdf "abstract"`  
34. `filetype:pdf "literature review" [TOPIC]`  
35. `filetype:pdf intitle:"thesis" OR intitle:"dissertation" [TOPIC]`  
36. `site:*.edu filetype:pdf "[TOPIC]"`  
37. `filetype:pdf "conference proceedings" [TOPIC] 2020..2026`

## **Category 4: Government Documents (Queries 38–50)**

38. `site:*.gov filetype:pdf "[TOPIC]"`  
39. `site:*.gov.uk filetype:pdf "[TOPIC]"`  
40. `filetype:pdf site:*.gov "budget" [YEAR]`  
41. `filetype:pdf site:*.gov "request for proposal" OR "RFP"`  
42. `filetype:pdf site:*.mil "[TOPIC]"`  
43. `site:*.gov filetype:xlsx "[TOPIC]"`  
44. `filetype:pdf site:*.gov "inspector general" [ORG]`  
45. `filetype:pdf site:*.gov "audit report"`  
46. `site:*.gov intitle:"index of" "reports"`  
47. `filetype:pdf site:*.gov "public comment" [TOPIC]`  
48. `filetype:pdf site:*.gov "memorandum" [TOPIC]`  
49. `site:*.europa.eu filetype:pdf "[TOPIC]"`  
50. `filetype:pdf site:*.gov "FOIA" [TOPIC]`

---

## **Cross-Reference Reminders**

* Run high-value queries across **2–3 engines** (Google, Bing, DuckDuckGo) per the multi-engine diversification principle — operator support and results will differ.  
* For any `site:*.gov` result that looks stale, cross-check against the **Wayback Machine** to see if it was live/updated recently, or whether you've found an orphaned/deindexed artifact.  
* A single indexed document is a *lead*. Corroborate names, figures, or claims inside it against at least one independent source before treating it as validated.

