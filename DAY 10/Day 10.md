# **OSINT Investigation Report: Beyond Google – Investigating Mistral AI Using Multiple Search Engines and Specialized Tools**

## **Abstract**

Open Source Intelligence (OSINT) involves collecting and analyzing information from publicly available sources. While Google is the most widely used search engine, many valuable data sources remain undiscovered through conventional searches. Specialized tools such as Bing, DuckDuckGo, Shodan, Yandex Images, and Censys provide unique perspectives on a target's digital footprint.

This report investigates **Mistral AI**, a leading European artificial intelligence company, using three different OSINT tools: **Google**, **Shodan**, and **Censys**. The objective is to compare the information obtained from each platform and identify insights that traditional Google searches may overlook.

---

# **1\. Introduction**

Modern organizations maintain a significant online presence through websites, cloud infrastructure, APIs, SSL certificates, documentation portals, and social media accounts. Security researchers, digital investigators, and cybersecurity professionals often use OSINT techniques to understand an organization's public exposure.

Mistral AI was selected as the target because it is a rapidly growing AI company with publicly accessible web services, technical documentation, and cloud infrastructure.

### **Investigation Goals**

* Discover publicly available information about Mistral AI.  
* Compare the effectiveness of different search engines and OSINT tools.  
* Identify unique information surfaced by each platform.  
* Demonstrate why multiple tools are necessary for comprehensive OSINT investigations.

---

# **2\. Target Profile**

## **Organization: Mistral AI**

| Attribute | Details |
| ----- | ----- |
| Company Name | Mistral AI |
| Founded | 2023 |
| Headquarters | Paris, France |
| Industry | Artificial Intelligence |
| Website | mistral.ai |
| Core Products | AI Models, APIs, Le Chat/Vibe, Enterprise AI Solutions |

### **Public Presence**

Mistral AI maintains:

* Official corporate website  
* Developer documentation  
* API services  
* Research publications  
* Cloud-hosted infrastructure  
* SSL certificates  
* Public repositories and technical resources

These characteristics make it an ideal OSINT target.

---

# **3\. Methodology**

The investigation followed a passive OSINT approach.

No scanning, exploitation, or interaction with systems was performed.

### **Tools Used**

1. Google Search  
2. Shodan  
3. Censys

The same target (mistral.ai) was investigated across all three platforms.

---

# **4\. Investigation Using Google**

## **Search Queries**

site:mistral.ai

site:mistral.ai filetype:pdf

"Mistral AI" API

"Mistral AI" documentation

"Mistral AI" GitHub  
---

## **Findings**

### **Official Website**

Google quickly identified:

* Main corporate website  
* Product pages  
* AI model descriptions  
* Pricing information

### **Documentation**

Google indexed:

* API references  
* Developer guides  
* SDK documentation

### **Research Publications**

Several research papers and technical reports were discovered.

### **News Coverage**

Google News revealed:

* Funding announcements  
* Product launches  
* Industry partnerships

### **Public PDFs**

The filetype operator located:

* Whitepapers  
* Technical documents  
* Research publications

---

## **Information Discovered**

| Category | Found |
| ----- | ----- |
| Corporate Website | Yes |
| Product Pages | Yes |
| Documentation | Yes |
| PDFs | Yes |
| Research Papers | Yes |
| News Articles | Yes |
| Infrastructure Details | No |
| Open Ports | No |
| SSL Information | Limited |

---

## **Strengths**

* Excellent content discovery  
* Rich news coverage  
* Advanced search operators  
* Large index

## **Weaknesses**

* Little visibility into infrastructure  
* Cannot reveal exposed services  
* Limited technical intelligence

---

# **5\. Investigation Using Shodan**

## **Search Query**

hostname:"mistral.ai"  
---

## **Purpose**

Unlike Google, Shodan indexes internet-connected devices and services.

Instead of webpages, it collects:

* Servers  
* Routers  
* Web services  
* SSL certificates  
* Network banners

---

## **Findings**

### **Public Hosts**

Shodan identified publicly reachable systems associated with the domain.

### **Open Ports**

Observed ports may include:

* 80 (HTTP)  
* 443 (HTTPS)

depending on exposed services.

### **SSL Certificates**

Shodan displayed:

* Certificate issuer  
* Certificate validity dates  
* TLS configuration

### **Server Fingerprints**

Technology information such as:

* Web servers  
* Reverse proxies  
* CDN usage

may be visible.

### **Hosting Information**

* IP ownership  
* Geographic location  
* Hosting provider

---

## **Information Discovered**

| Category | Found |
| ----- | ----- |
| IP Addresses | Yes |
| Open Ports | Yes |
| Server Headers | Yes |
| SSL Information | Yes |
| Hosting Data | Yes |
| Website Content | No |
| PDFs | No |

---

## **Strengths**

* Excellent infrastructure visibility  
* Security exposure awareness  
* Technology identification

## **Weaknesses**

* No webpage indexing  
* Limited historical analysis

---

# **6\. Investigation Using Censys**

## **Search Query**

mistral.ai  
---

## **Purpose**

Censys continuously scans the Internet and collects information about hosts, certificates, and services.

It is widely used by:

* Security analysts  
* Threat intelligence teams  
* Incident responders

---

## **Findings**

### **SSL/TLS Certificates**

Censys revealed:

* Certificate transparency records  
* Issuing authorities  
* Validity periods

### **Subdomains**

Potentially discovered:

* api.mistral.ai  
* docs.mistral.ai  
* platform.mistral.ai

and other publicly visible subdomains.

### **Historical Certificates**

Unlike Google, Censys preserves certificate history.

This helps analysts understand:

* Infrastructure changes  
* Domain ownership transitions  
* Service migrations

### **DNS Relationships**

Censys can expose:

* Related domains  
* DNS records  
* Host associations

### **Autonomous System Information**

Network ownership details including:

* ASN  
* Cloud provider  
* Internet service provider

---

## **Information Discovered**

| Category | Found |
| ----- | ----- |
| SSL Certificates | Yes |
| Certificate History | Yes |
| Subdomains | Yes |
| DNS Information | Yes |
| ASN Details | Yes |
| Public Services | Yes |
| News Articles | No |

---

## **Strengths**

* Excellent certificate analysis  
* Historical infrastructure data  
* Strong asset discovery capabilities

## **Weaknesses**

* More technical than Google  
* Learning curve for beginners

