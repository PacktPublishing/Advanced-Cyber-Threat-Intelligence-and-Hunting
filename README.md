# Advanced Cyber Threat Intelligence and Hunting

<a href="https://www.packtpub.com/en-us/product/advanced-cyber-threat-intelligence-and-hunting-9781806380398"><img src="https://content.packt.com/_/image/xxlarge/B34740/cover_image_small.jpg" alt="Advanced Cyber Threat Intelligence and Hunting" height="256px" align="right"></a>

This is the companion repository for [Advanced Cyber Threat Intelligence and Hunting](https://www.packtpub.com/en-us/product/advanced-cyber-threat-intelligence-and-hunting-9781806380398), published by Packt.

**Detect APTs and zero-day attacks using CTI, behavioral analytics, and AI techniques**

## What is this book about?

This book moves beyond theory and provides a practical, technically grounded approach to detecting advanced threats — including APTs and zero-day activity — through structured analytics, data-driven hunting, and a deep understanding of attacker behavior.

This book covers the following exciting topics:

* Build and operationalize a Cyber Threat Intelligence (CTI) program
* Map intelligence to detection logic using frameworks like MITRE ATT&CK
* Apply advanced hunting techniques across endpoint, network, and cloud telemetry
* Detect stealthy adversary tradecraft including C2, persistence, privilege escalation, and exfiltration
* Use analytics and machine learning to drive real hunting scenarios
* Surface zero-day activity through behavioral clustering and the Time-Terrain-Behavior (TTB) framework
* Profile adversaries, attribute activity, and track campaigns over time

If you feel this book is for you, get your [copy](https://www.amazon.com/Advanced-Cyber-Threat-Intelligence-Hunting/dp/1806380390) today!

## Instructions and Navigation

The practical material used throughout the book is organized into folders, one per chapter.

Most chapter folders contain the hunting queries used in the book. For example, the queries for Chapter 9 live in `chapter-09/`. Every chapter folder contains its own `README.md` listing the files it contains and a brief explanation of what each one does.

Some folders also contain companion intelligence artifacts rather than queries. Chapter 3, for example, includes a standalone APT41 threat profile that demonstrates how adversary profiling can be structured and operationalized into threat hunting hypotheses, telemetry requirements, and detection opportunities.

Queries are written in the native language of the platform they target, and the file extension reflects this:

| Extension | Language | Platform |
| --------- | --------------- | ---------------------------------------------- |
| `.esql` | ES\|QL | Elastic Stack |
| `.eql` | EQL | Elastic Stack (event correlation) |
| `.kql` | KQL | Microsoft Sentinel / Defender XDR |
| `.spl` | SPL | Splunk |
| `.sigma` | Sigma | Generic (convertible to most SIEM backends) |
| `.json` | JSON definition | Elastic ML jobs, transforms, enrich policies |
| `.md` | Markdown | Companion documentation and intelligence artifacts |

The queries are intended to be run directly against the corresponding platform's query interface (Kibana Discover / Sentinel / Splunk Search). Some require enrich policies, lookup tables, or baseline indices to be populated first — those prerequisites are documented in the relevant chapter's `README.md` (see for example Chapter 18, which walks through the full setup of the TTB velocity-extension enrich policies).

**What you need for this book:**

This book is for cyber threat intelligence analysts, threat hunters, detection engineers, SOC analysts, and incident responders looking to move beyond signature-based detection and operationalize behavior-driven hunting at scale. A working knowledge of SIEM tooling, the MITRE ATT&CK framework, and at least one query language (KQL, SPL, or ES\|QL) will help you get the most out of the queries and companion artifacts in this repository.

### Software and Hardware List

| Software covered in the book | Operating system requirements |
| ----------------------------------------------------- | ----------------------------- |
| Elastic Stack (Elasticsearch, Kibana) 9.x | Windows, Linux, or macOS |
| Splunk Enterprise / Splunk Cloud | Windows, Linux, or macOS |
| Microsoft Sentinel / Microsoft Defender XDR | Browser-based |
| Sysmon | Windows |

## Chapters with companion material

| Chapter | Title | Folder | Contents |
| ------- | ----------------------------------------------------------- | ------------- | ------------------------------- |
| 3 | Deep Dive – CTI Collection and Enrichment for APTs | `chapter-03/` | APT41 adversary profile |
| 4 | Core Principles of Proactive Threat Hunting | `chapter-04/` | Hunting queries |
| 5 | Understanding Data Sources for Threat Hunting | `chapter-05/` | Hunting queries |
| 6 | Hunting Zero-Days Through Behavioral Signatures | `chapter-06/` | Hunting queries |
| 7 | Advanced Hunting Techniques and Queries | `chapter-07/` | Hunting queries |
| 8 | Hunting Delivery and Initial Access | `chapter-08/` | Hunting queries |
| 9 | Hunting for Exploitation and Execution | `chapter-09/` | Hunting queries |
| 10 | Hunting for Persistence and Privilege Escalation | `chapter-10/` | Hunting queries |
| 11 | Hunting for Lateral Movement and Discovery | `chapter-11/` | Hunting queries |
| 12 | Hunting for Command and Control | `chapter-12/` | Hunting queries |
| 13 | Hunting for Collection, Exfiltration and Impact | `chapter-13/` | Hunting queries |
| 15 | Behavioral Clustering for Zero-Day Detection | `chapter-15/` | Hunting queries |
| 16 | Hunting in Cloud and Specialized Environments | `chapter-16/` | Hunting queries |
| 18 | Emerging Trends in Threat Hunting and CTI | `chapter-18/` | Hunting queries |

Chapters not listed above are conceptual and do not currently ship with companion material in this repository.

## Get to Know the Authors

**Gianluca Tiepolo** is a cybersecurity researcher specializing in Cyber Threat Intelligence within the telecommunications industry. Over the past 16+ years he has performed security monitoring, threat hunting, incident response, and intelligence analysis as a consultant for dozens of organizations, including several Fortune 100 companies.

**Dan Sorensen** is a seasoned Chief Information Security Officer (CISO) and security advisor with broad experience directing enterprise cybersecurity programs in excess of $50M, spanning detection engineering, intelligence operations, and incident response across multiple industries.
