# **Google Hacking Database (GHDB) Structured Dorking**

The Google Hacking Database is a curated library of pre-tested Google dork queries, maintained by Exploit-DB (Offensive Security). Each entry pairs a generic search-operator pattern with a category describing the type of exposure it surfaces (credentials, open directories, login portals, etc.).

### **Why It Matters for OSINT Work**

* Turns ad-hoc dork chaining (Module 9\) into a **repeatable, categorized methodology** instead of one-off guesses.  
* Because entries are public and widely known, a hit is a strong signal — if a target is exposed via a cataloged GHDB pattern, it's exposed to everyone searching the same way, not just to this exercise.  
* Gives a natural **taxonomy for logging findings**: category → pattern → adapted query → result → classification (Confirmed/Inferred  
*   
*   
*   
* Every dork run gets logged with:  
1. **GHDB category** (from the official taxonomy)  
2. **Original GHDB pattern** (generic form)  
3. **Adapted query** (target-scoped, using `site:`)  
4. **Result count** (order of magnitude is fine; exact count not required)  
5. **Classification** — Confirmed (directly observed) vs Inferred (indirect evidence, e.g. cached snippet without full page access)  
6. **No enumeration of specific PII, credentials, or file contents** — pattern-level only, per the minimization principle already applied in Mistral AI / Hugging Face / GitHub exercises.  
7. **Tooling constraints** (egress blocks, JS-rendered pages Google can't index, etc.) logged separately from findings — never conflated with a target-side result.

---

## **2\. Worked Example (Illustrative — Hugging Face)**

This reuses the target already covered in Module 9 to keep the example grounded rather than introducing a new live target.

### **Methodology**

GHDB categories selected for relevance to a corporate/ML-platform target. Patterns adapted by inserting `site:huggingface.co`. Queries run manually via standard Google Search (no API/automation — consistent with the sandbox egress constraint documented in prior modules).

### **Ethics Notes**

* **Necessity:** categories chosen map directly to credential/config exposure risk, which is the stated objective of this exercise.  
* **Proportionality:** search scope limited to Google's public index only; no attempt to access, download, or use any discovered file.  
* **Minimization:** findings below are stated at pattern/category level; no file paths, usernames, or credential fragments are reproduced.

### **Query Log**

| \# | GHDB Category | Original GHDB Pattern | Adapted Query | Result Count | Classification |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | Files Containing Juicy Info | `filetype:env "DB_PASSWORD"` | `site:huggingface.co filetype:env` | Multiple | Confirmed (pattern present) |
| 2 | Pages Containing Login Portals | `intitle:"login" inurl:admin` | `site:huggingface.co intitle:"login" inurl:admin` | Low/none | Inferred (no clear public admin portal indexed) |
| 3 | Sensitive Directories | `intitle:"index of" "parent directory"` | `site:huggingface.co intitle:"index of"` | Low | Inferred (isolated hits, likely doc/asset dirs) |

### **Findings (Pattern-Level Only)**

* **Files Containing Juicy Info:** indexed `.env`\-type files present at scale — consistent with the exposure pattern already logged in Module 9\. Confirmed as a pattern; individual file instances not enumerated.  
* **Pages Containing Login Portals:** no significant public-facing admin login surface indexed under this pattern. Inferred absence, not proof of absence (Google indexing gaps possible).  
* **Sensitive Directories:** minimal signal; likely benign (static asset directories) rather than a genuine misconfiguration. Inferred, low confidence.

### **Tooling Constraints**

No live HTTP fetch performed against any discovered `.env` path (sandbox egress restriction \+ ethics boundary — indexed existence is sufficient for the exercise; content retrieval is out of scope). This is logged as a methodology limitation, not a target-side finding.

### **Next Steps**

* Cross-reference "Files Containing Juicy Info" hits against HackerOne VDP disclosures to see if this pattern was previously reported.  
* Extend the same GHDB category set to a second target for comparison.

