# APT41 Threat Profile

## 1. Executive Summary

APT41 is a China-linked threat group assessed by multiple public sources as conducting both state-sponsored espionage and financially motivated activity. Mandiant describes APT41 as unusual among tracked China-based actors because it has used non-public malware normally associated with espionage operations in activity that appears to support personal financial gain. MITRE ATT&CK tracks APT41 as active since at least 2012, targeting sectors including healthcare, telecommunications, technology, finance, education, retail and video games across multiple countries.

APT41 does not fit a single static persona. Its espionage activity aligns with strategic intelligence collection, long-term access and targeted data acquisition, while its financially motivated activity has historically included video game industry targeting, theft of source code and digital certificates, manipulation of virtual currencies and attempted ransomware deployment. This dual motivation model directly affects defensive analysis: a profile that treats APT41 only as a state espionage actor or only as a cybercriminal group will miss important behavioral patterns.

From a hunting perspective, APT41 should be treated as a high-capability actor with a broad intrusion playbook. Public reporting links the group to exploitation of internet-facing applications, supply-chain compromises, web shells, Cobalt Strike, custom malware families, credential theft, lateral movement across Windows and Linux systems, database collection, use of stolen code-signing certificates, cloud or web-service-based command and control and exfiltration to cloud storage. Recent reporting on the APT41 DUST campaign shows continued use of multi-stage malware, in-memory execution, DLL side-loading, Windows service persistence, Cloudflare Workers or self-managed infrastructure for command and control, and Microsoft OneDrive for exfiltration.

## 2. Source Basis and Confidence Assessment

| Assessment Area | Confidence | Rationale |
|---|---:|---|
| APT41 conducts both espionage and financially motivated operations | High | Reported by Mandiant/Google and reflected in MITRE ATT&CK group description. |
| APT41 is China-linked / Chinese state-sponsored | High | Assessed by Mandiant and MITRE; U.S. DOJ/FBI public actions associate named individuals with APT41/BARIUM. |
| APT41 has targeted healthcare, telecom, technology, finance, education, retail, video games and government entities | High | Consistent across Mandiant, MITRE and DOJ reporting. |
| APT41 uses both custom malware and public tools | High | MITRE and Mandiant list many associated software families and observed procedures; Group-IB describes operational tool use including Cobalt Strike, SQLmap, Mimikatz and native Windows utilities. |
| APT41 uses supply-chain compromise as part of its operational playbook | High | Mandiant and DOJ public reporting describe supply-chain compromise and injection of malicious code into legitimate software or updates. |
| APT41 working hours, internal structure and individual operator identities | Medium | DOJ/FBI public material identifies alleged individuals; broader organizational structure and tasking relationships remain partially inferred from activity patterns and public reporting. |
| Current infrastructure preferences beyond the most recently published campaigns | Medium | Infrastructure changes frequently; public reporting provides snapshots rather than continuous visibility. |
| Specific future targeting | Low to Medium | Historical victimology supports prioritization, but future targeting depends on strategic tasking, geopolitical context and opportunity. |

## 3. Actor Overview

### 3.1 Naming and Aliases

Public reporting associates APT41 with several overlapping names, including:

- APT41
- BARIUM
- Wicked Panda
- Brass Typhoon
- Winnti Group overlap in some public reporting
- Double Dragon, used by some reporting to capture the actor's dual espionage and cybercrime nature

Alias mapping should be handled carefully. Public vendor names do not always map one-to-one across datasets. For operational use, the profile should preserve alias relationships but avoid assuming that every report using a related name describes the exact same operational cell, campaign or intrusion set.

### 3.2 Attribution

APT41 is publicly assessed as a China-linked actor. Mandiant describes the group as carrying out state-sponsored espionage in parallel with financially motivated operations. MITRE ATT&CK describes APT41 as a Chinese state-sponsored espionage group that also conducts financially motivated operations. The U.S. Department of Justice announced charges in 2020 against five PRC nationals alleged to be associated with APT41 activity affecting more than 100 victim organizations worldwide.

Analytical note: attribution should be expressed with source and confidence. For defensive hunting, the most important use of attribution is not geopolitical labeling, but the prioritization of likely objectives, target types, access methods and post-compromise behaviors.

### 3.3 Activity Timeline

| Period | Publicly Reported Activity Pattern |
|---|---|
| At least 2012 onward | MITRE tracks APT41 as active since at least 2012. |
| 2014 onward | Mandiant reports simultaneous cybercrime and cyber espionage operations from 2014 onward. |
| 2019 | Mandiant publishes a major public report on APT41's dual espionage and cybercrime operations. |
| 2020 | U.S. DOJ announces charges against individuals alleged to be associated with APT41 activity. |
| 2021–2022 | Public reporting describes exploitation of internet-facing applications, U.S. state government compromises, rapid use of disclosed and zero-day vulnerabilities and re-compromise after remediation. |
| 2023–2024 | APT41 DUST campaign targets entities in Europe, Asia and the Middle East, including shipping, logistics and media; activity includes DUSTPAN, DUSTTRAP, Cloudflare Workers, compromised Google Workspace accounts and OneDrive exfiltration. |

### 3.4 Diamond Model Synthesis

The following synthesis applies the Diamond Model to APT41 across its publicly observed activity. This is a consolidated view, not a single-event diamond: vertices are populated from recurring patterns across multiple campaigns. Specific incidents will instantiate the diamond differently, but the recurring shape across the actor's operational lifetime is consistent enough to support a generalized view.

| Vertex | Populated Content |
|---|---|
| **Adversary** | China-linked threat group tracked as APT41, BARIUM, Wicked Panda, Brass Typhoon, Double Dragon, with overlap into Winnti-related reporting. Publicly assessed by Mandiant and MITRE as state-sponsored, with parallel financially motivated activity attributed to overlapping personnel. U.S. DOJ 2020 indictments named five PRC nationals associated with APT41/BARIUM activity, with operational ties to Chengdu 404 Network Technology. |
| **Capability** | Multi-family custom malware ecosystem (recent: DUSTPAN, DUSTTRAP, PINEGROVE; historical: LOWKEY.PASSIVE, DEADEYE, MESSAGETAP and Winnti-adjacent families). Commodity tooling (Cobalt Strike BEACON, Mimikatz, Procdump, SQLmap, certutil, native Windows utilities). Web shells (ANTSWORD, BLUEBEAM). Database export tooling (SQLULDR2). Stolen code-signing certificates used to legitimize payloads. DLL side-loading, in-memory execution, named-pipe impersonation (BADPOTATO), pass-the-hash. |
| **Infrastructure** | Self-managed C2 infrastructure, Cloudflare Workers serverless C2, compromised Google Workspace accounts, Microsoft OneDrive for exfiltration, web shells on compromised internet-facing servers, abuse of legitimate software update channels in supply-chain operations. Infrastructure rotates quickly; serverless and cloud-service abstraction reduces the operational value of static network indicators. |
| **Victim** | Telecommunications, healthcare, high technology, software development, video games, education, travel, government, shipping/logistics, media, automotive. Targeted in at least 14 countries; recent (2023–2024) APT41 DUST activity in Italy, Spain, Taiwan, Thailand, Turkey and the United Kingdom. |

**Meta-features:**

| Meta-Feature | Value |
|---|---|
| Timestamp | Sustained activity from at least 2012 to present, with recent (2023–2024) DUST campaign activity. |
| Phase | All phases of the intrusion lifecycle observed in public reporting. |
| Result | Long-term persistent access; theft of telecom call records, healthcare research data, source code, code-signing material, virtual currency, database content and credentials. |
| Direction | Adversary-to-Victim primary direction; bidirectional during active C2. |
| Methodology | Mixed. Espionage operations favor low-and-slow access maintenance with selective collection. Financially motivated operations show more direct monetization behaviors but use the same operator skill base and tool families. |
| Resources | Substantial and sustained: multi-family malware development across more than a decade, supply-chain compromise capability, stolen code-signing material, infrastructure budget supporting cloud and serverless abstraction, multi-country operational reach. |

**Edges:**

- **Adversary–Victim (social-political):** APT41's espionage targeting aligns with PRC strategic intelligence collection priorities (telecom metadata, healthcare research, technology intellectual property, regional intelligence). Financial targeting of monetizable assets (gaming, certificates, virtual currency) sits outside the core state mission and is the strongest evidence of operator polymorphism.
- **Capability–Infrastructure (technology):** A recurring operational chain anchors the actor's profile: web exploitation → web shell deployment → DLL side-loading and in-memory loader execution → Cobalt Strike or custom backdoor C2 → cloud-service or serverless C2 abstraction → cloud-storage exfiltration. Defenders should treat this chain as the durable behavioral signature rather than any single component within it.

## 4. Intent and Motivation

### 4.1 State-Sponsored Espionage

APT41's espionage activity appears aligned with strategic intelligence collection. Public reporting describes targeting of healthcare, high technology, telecommunications, travel services, higher education and news/media organizations. Mandiant notes that APT41 has established and maintained access in sectors such as healthcare, high-tech and telecommunications, and has repeatedly targeted call record information at telecommunications companies.

Operational implication: defenders in sectors aligned with strategic intelligence priorities should treat APT41-like activity as potentially long-dwell, collection-oriented and access-preserving. The actor may prioritize stealth, credential access, database access, selective data collection and persistence over immediate disruption.

### 4.2 Financially Motivated Operations

APT41's financially motivated operations have historically focused heavily on the video game industry. Public reporting describes theft of source code and digital certificates, manipulation of virtual currencies and attempted ransomware deployment. This activity appears distinct from traditional state-directed collection but overlaps in tooling and operator capability.

Operational implication: when the actor pursues financially motivated objectives, defenders may observe behaviors closer to cybercriminal intrusion patterns: access to production environments, theft of monetizable assets, manipulation of in-game economies, certificate theft and potentially disruptive actions. However, the underlying operator skill and tooling may remain consistent with a high-end APT intrusion.

### 4.3 Dual-Track Motivation Model

APT41's dual motivation model should be represented explicitly in the profile because it changes how evidence is interpreted. For example:

- Theft of telecom call records is more consistent with state intelligence collection.
- Theft of video game source code, digital certificates or virtual currency is more consistent with financial gain.
- Supply-chain compromise can support either mission: strategic access for espionage or downstream monetization.
- Use of the same or related malware across mission types can create analytical ambiguity.

This is why the actor should not be reduced to a single motivational label. Motivation should be treated as a dynamic assessment linked to campaign context, victimology, data accessed and post-compromise behavior.

## 5. Sophistication and Capability

**STIX Threat Actor Sophistication: Strategic.**

This assessment reflects the actor's organized team structure, sustained funding for multi-year operations, demonstrated capability to develop novel exploits and supply-chain compromises, deep cross-platform knowledge spanning Windows, Linux and cloud environments, and operational resilience across more than a decade of public visibility. These attributes align with the highest tier of the STIX threat actor sophistication vocabulary.

### 5.1 Technical Capabilities

APT41 demonstrates high technical capability across multiple domains:

- Exploitation of internet-facing applications, including rapid operationalization of public vulnerabilities and reported use of zero-days.
- Use of supply-chain compromise to inject malicious code into legitimate software or update channels.
- Use of both custom malware and public tools.
- Ability to move laterally across Windows and Linux environments.
- Use of stolen digital certificates and code signing to increase trust in malware.
- Use of web shells, in-memory loaders, plugin frameworks and stealthy passive backdoors.
- Use of cloud or web services for command and control and exfiltration.

The actor's capability is not limited to malware development. APT41 has shown operational capability in target development, exploitation, privilege escalation, credential access, persistence, lateral movement, collection, staging and exfiltration.

### 5.2 Operational Discipline

APT41's operational discipline appears mixed but generally mature. The group has demonstrated the ability to maintain prolonged unauthorized access, selectively deploy follow-on malware, use victim-specific checks and return to compromised environments after remediation. At the same time, public reporting has associated the actor with repeated use of recognizable tools and operational patterns, including Cobalt Strike, web shells, credential dumping utilities and characteristic exploitation workflows.

Analytical assessment: the actor is sophisticated enough to adapt quickly, maintain access and conduct multi-stage operations, but not so opaque that defenders lack behavioral hunting opportunities. APT41's reliance on repeatable intrusion mechanics creates detectable patterns when telemetry coverage is adequate.

### 5.3 Adaptability and Exploitation Tempo

Public reporting on APT41 campaigns indicates rapid adaptation to public vulnerabilities and exploitation of internet-facing systems. MITRE's C0017 campaign description notes that APT41 compromised at least six U.S. state government networks through vulnerable internet-facing applications and was quick to adapt to both publicly disclosed and zero-day vulnerabilities.

Operational implication: defenders should not only monitor known APT41 indicators. They should prioritize exploit-to-post-exploitation chains around internet-facing services, especially when newly disclosed vulnerabilities affect externally exposed infrastructure.

## 6. Victimology

### 6.1 Targeted Sectors

APT41's publicly reported targeting spans both espionage-relevant and financially relevant sectors:

| Sector | Likely Strategic Relevance |
|---|---|
| Telecommunications | Call records, subscriber metadata, network access, strategic surveillance value. |
| Healthcare | Research, sensitive personal data, strategic and economic intelligence. |
| High technology | Intellectual property, product roadmaps, supply-chain access. |
| Software development | Source code, build environments, code-signing material, downstream compromise opportunities. |
| Video games | Financial gain through virtual currency manipulation, source code theft and certificate theft. |
| Education and research | Intellectual property, research, access to individuals and networks. |
| Travel and hospitality | Tracking individuals, diplomatic or intelligence-related travel visibility. |
| Government | Strategic intelligence, PII, internal communications and policy information. |
| Shipping and logistics | Supply-chain intelligence, operational data, strategic movement visibility. |
| Media and entertainment | Information collection, influence-relevant data, regional strategic visibility. |

### 6.2 Geographic Targeting

APT41 has been observed targeting organizations across multiple countries. MITRE describes APT41 as active in 14 countries, while recent APT41 DUST reporting includes entities in Europe, Asia and the Middle East. The 2024 DUST-related reporting includes sectors and countries such as shipping/logistics, media, technology and automotive in Italy, Spain, Taiwan, Thailand, Turkey and the United Kingdom.

Operational implication: geographic exposure should be assessed through both headquarters location and operational footprint. Multinational subsidiaries, regional business units and sector-specific partners may be targeted even when the parent organization is headquartered elsewhere.

### 6.3 Targeted Data and Assets

Based on public reporting, priority data and assets may include:

- Telecommunications call records and subscriber-related metadata.
- Personally identifiable information from government or public-sector networks.
- Source code and software build assets.
- Digital certificates and code-signing material.
- Game production environments and virtual currency systems.
- Database content, including Oracle database exports in recent reporting.
- Sensitive business, logistics, media and technology data.
- Credentials enabling long-term access or lateral movement.

## 7. Persona, OPSEC and Human Factors

### 7.1 Polymorphic Operational Persona

APT41 is a strong example of an actor whose persona can change by campaign context. In espionage operations, the actor behaves as a strategic access and intelligence collection group. In financially motivated activity, the same broader actor cluster has demonstrated behavior closer to cybercriminal monetization.

This polymorphism matters because defenders often build mental models around single-purpose adversaries. APT41 demonstrates that the same actor profile can include:

- Long-term access and selective collection.
- Theft of monetizable assets.
- Supply-chain compromise.
- Credential theft and lateral movement.
- Use of custom malware and commodity tools.
- Operational behaviors that vary by target and mission.

### 7.2 OPSEC Characteristics

APT41 demonstrates meaningful OPSEC capability, including selective deployment of follow-on malware, use of legitimate services, use of code signing, in-memory execution and infrastructure abstraction through services such as Cloudflare Workers in recent reporting. However, public reporting also shows repeated operational patterns that defenders can exploit, including use of web shells, Windows services, certutil, Cobalt Strike, SQLmap and credential dumping tooling.

Assessment: APT41's OPSEC should be considered strong at the campaign level but not immune to behavioral detection. Durable hunting opportunities exist at the intersection of exploitation, web shell activity, process lineage, credential dumping, database collection, staging and cloud-service exfiltration.

### 7.3 Human and Organizational Indicators

The actor's long-running activity, broad tooling, malware diversity, supply-chain capability and ability to conduct both espionage and financial operations suggest an organized capability rather than opportunistic intrusion activity. The U.S. DOJ and FBI have publicly associated named individuals with APT41/BARIUM-related activity, but defenders should avoid overfitting operational detection to named individuals. The useful defensive abstraction is the actor's recurring operational playbook.

### 7.4 Linguistic and Cultural Indicators

Linguistic attribution evidence for APT41 is mixed and has degraded over time. Older Winnti-adjacent samples and early Duke-family-adjacent code contained Chinese-language artefacts in PDB paths, code comments and string tables. Recent samples (DUSTPAN, DUSTTRAP) show evidence of operational sanitization: developer artefacts have been progressively removed, and the use of commodity malware such as Cobalt Strike BEACON further obscures author signatures.

The strongest direct linguistic and cultural attribution evidence is the U.S. Department of Justice's 2020 indictments, which named five PRC nationals operationally associated with APT41/BARIUM activity, with documented ties to Chengdu 404 Network Technology — a Chinese company described in the indictments as serving as a contractor for activity overlapping with state intelligence interests. Court-disclosed evidence of this kind is rare in nation-state attribution and is more probative than typical artefact-based linguistic inference.

Phishing lure quality varies by target and campaign. Recent diplomatic and corporate lures show consistent grammatical fluency, but variation in idiom and register suggests multiple operators, or external content generation tooling, rather than a single author. Cultural indicators include:

- Operational interest in regional matters consistent with PRC strategic priorities, including Hong Kong-related entities, the Taiwanese government and technology sector, Southeast Asian governments, and telecommunications companies in countries with PRC strategic exposure.
- Recurrent video game industry targeting that does not align with conventional state intelligence priorities and is publicly assessed as reflecting operator personal financial interest. This is itself a significant cultural-organizational signal: it suggests an operator population with overlapping state-sanctioned and personal-monetization activity using the same toolset.

Analytical caveat: linguistic indicators in current samples are of limited operational value for attribution due to sanitization and commodity tool use. The broader cultural-organizational inference — a state contractor relationship with personal-monetization side activity — is supported by both technical patterns and court-disclosed evidence and is more durable than the underlying linguistic artefacts.

### 7.5 Operational Tempo and Timestamp Analysis

APT41's espionage activity has consistently shown an operational tempo aligned with UTC+8 business hours on a Monday-to-Friday cycle. Mandiant's 2019 reporting documented this pattern across file system timestamps on compromised hosts, network log timestamps from C2 communications, and compilation timestamps embedded in custom malware samples. The pattern is consistent with operators based in mainland China working a standard professional schedule.

The financially motivated operations attributed to overlapping APT41 personnel show a different timestamp profile. Activity associated with video game industry targeting and similar monetization-focused operations occurs disproportionately outside standard business hours, including evenings and weekends. The most parsimonious explanation is that the same operators use state-developed tooling for personal financial gain on their own time, outside the official tasking cycle. This is the single clearest behavioral signal of operator polymorphism in the public record for this actor.

The dual-tempo pattern has two practical implications for defenders:

- For state-mission APT41 activity, monitoring windows aligned with UTC+8 business hours — approximately 01:00 to 09:00 UTC, Monday through Friday — will see the highest concentration of operator activity. Authentication anomalies, web shell interaction, lateral movement and collection commands clustered in these windows warrant priority review.
- For financially motivated activity in monetizable sectors (gaming, cryptocurrency-adjacent services, certificate-issuing infrastructure), evening and weekend windows in UTC+8 are higher-yield monitoring intervals. The same toolset will be observed, but timestamp clustering will differ.

The timestamp pattern also supports an inference about organizational structure that goes beyond timestamp analysis itself. The maintenance of a custom malware ecosystem of more than 40 distinct families over more than a decade, on a regular business schedule, implies a dedicated software development team with version control, quality assurance, tasking processes and budget — characteristic of a professional organization rather than a loose criminal collective. The 2020 DOJ indictments support this organizational inference directly by tying named individuals to Chengdu 404 Network Technology, a structured commercial entity. The convergence of timestamp inference with court-disclosed organizational evidence is unusual in public CTI reporting and makes the organizational characterization unusually well-supported for this actor.

For defenders building hunt schedules and ephemeral baselines, the operational tempo finding is: APT41 has two distinct activity rhythms — official-mission UTC+8 business hours and after-hours financial activity — and detection should be calibrated to both. The rhythm itself functions as a soft attribution signal when distinguishing APT41 from other PRC-linked actors that may share toolsets but not tempo patterns.

## 8. Infrastructure Profile

### 8.1 Infrastructure Types

APT41-associated public reporting includes several infrastructure patterns:

- Self-managed command-and-control infrastructure.
- Infrastructure hidden behind Cloudflare or using Cloudflare Workers.
- Web shells deployed on compromised internet-facing servers.
- Compromised or abused cloud accounts and web services.
- Use of Microsoft OneDrive for exfiltration in recent reporting.
- Use of legitimate software update or supply-chain channels in selected campaigns.

### 8.2 Hosting and Domain Patterns

APT41 infrastructure should not be modeled only as a list of domains and IP addresses. The actor's infrastructure has included compromised servers, cloud services, legitimate software distribution paths, serverless infrastructure and web-service abuse. This means that infrastructure hunting should include:

- Passive DNS and historical resolution analysis.
- Certificate and TLS fingerprinting where applicable.
- Cloud service and web service telemetry.
- Web shell detection on externally exposed servers.
- Process lineage from web server processes.
- Outbound connections from servers that normally should not initiate internet connections.
- New or unusual service registrations on compromised hosts.

### 8.3 Infrastructure Reuse and Rotation

APT41 has demonstrated both reuse and evolution. Some campaigns involve recognizable malware families, tooling and operational sequences, while newer activity introduces updated components such as DUSTTRAP. Infrastructure may rotate quickly, and the actor may use legitimate services to reduce the value of static network indicators.

Operational implication: defenders should prioritize infrastructure behavior and relationships over single indicators. Examples include newly observed outbound HTTPS from web servers, web server child processes invoking transfer utilities, Cloudflare Worker endpoints in unusual server traffic, service creation following web exploitation and large outbound transfers to cloud storage.

### 8.4 Defensive Pivot Opportunities

| Seed Artifact | Pivot Question | Defensive Use |
|---|---|---|
| Web shell path or hash | Which hosts contain similar web shell artifacts or access patterns? | Identify additional compromised externally exposed systems. |
| C2 domain or IP | Which internal hosts resolved or connected to this infrastructure? | Scope affected systems and possible lateral movement. |
| TLS/JARM/certificate pattern | Which other servers share this fingerprint? | Discover related infrastructure or tool deployments. |
| Stolen or suspicious code-signing certificate | Which binaries are signed with the same certificate? | Identify related malware and supply-chain artifacts. |
| Cloud storage destination | Which internal hosts uploaded unusual archives to the same service? | Hunt for collection and exfiltration. |
| Windows service name/path | Where else was the same service created? | Detect persistence across hosts. |
| Database export utility execution | Which servers executed unusual database export commands? | Detect collection from backend systems. |

## 9. Malware and Tooling Profile

### 9.1 Custom Malware and Frameworks

Public reporting associates APT41 with a broad malware ecosystem. Recent APT41 DUST reporting includes DUSTPAN and DUSTTRAP:

- **DUSTPAN**: in-memory dropper that decrypts and executes embedded or external payloads. Recent reporting observed DUSTPAN masquerading as legitimate Windows binaries and persisting via Windows services.
- **DUSTTRAP**: multi-stage plugin framework using encrypted on-disk components, victim-specific decryption material and in-memory execution.
- **BEACON / Cobalt Strike**: observed in multiple APT41-related operations, including recent reporting.
- **LOWKEY.PASSIVE, DEADEYE and related components**: observed in earlier public reporting and MITRE campaign mappings.

### 9.2 Publicly Available and Dual-Use Tools

APT41 has used publicly available and dual-use tooling, including:

- Cobalt Strike Beacon.
- SQLmap for SQL injection and exploitation workflows.
- Mimikatz and credential access tooling.
- Procdump for LSASS dumping workflows.
- Certutil for payload transfer, decoding or execution chains.
- Nmap and other scanning utilities.
- Web shells such as ANTSWORD and BLUEBEAM in recent reporting.
- Native Windows commands for discovery and execution.

Defensive implication: detections should not depend only on tool names. Many of these utilities are dual-use or commonly abused. Hunt logic should emphasize context: parent process, host role, command-line parameters, timing, remote origin, output artifacts and follow-on behavior.

### 9.3 Living-off-the-Land Behavior

APT41 activity frequently includes commands and utilities already present in enterprise environments. Examples from public reporting include use of `cmd.exe`, `certutil.exe`, Windows services, web server processes, credential dumping utilities and native discovery commands.

Hunting approach: identify abnormal use of legitimate binaries from unusual parent processes or on unusual host roles, especially after web exploitation or web shell activity.

## 10. Operational Playbook

### 10.1 Reconnaissance and Target Development

APT41 has used active and passive reconnaissance to identify vulnerable systems. Public reporting describes use of tools such as Nmap, Acunetix, directory brute-forcing utilities, subdomain enumeration tools, JexBoss and internet scan databases such as FOFA.

Defensive opportunities:

- Monitor internet-facing services for scanning followed by exploitation attempts.
- Correlate WAF, reverse proxy and web server logs for enumeration patterns.
- Hunt for suspicious requests to known vulnerable application paths.
- Track spikes in 404s, unusual user agents and directory brute forcing against externally exposed applications.

### 10.2 Initial Access

APT41 has used multiple initial access vectors:

- Exploitation of public-facing applications.
- SQL injection.
- Spear phishing.
- Watering hole attacks.
- Supply-chain compromise.
- Exploitation of newly disclosed and zero-day vulnerabilities.

Defensive opportunities:

- Prioritize vulnerable internet-facing systems for log review immediately after public disclosure of high-impact vulnerabilities.
- Hunt for web server process chains spawning command shells, scripting engines or transfer utilities.
- Monitor for web shell file creation in web roots and application upload directories.
- Review authentication anomalies following exploitation windows.

### 10.3 Execution

APT41 execution patterns include command shell activity, web shell command execution, Base64-encoded payload transfer, certutil decoding and execution, DLL side-loading and in-memory execution of encrypted payloads.

Defensive opportunities:

- Detect web server processes spawning `cmd.exe`, `powershell.exe`, `wscript.exe`, `cscript.exe`, `certutil.exe`, `bitsadmin.exe`, `rundll32.exe` or `regsvr32.exe`.
- Hunt for repeated `echo` commands reconstructing large Base64 payloads.
- Detect `certutil -decode`, `certutil -urlcache`, `certutil -split` or `certutil -hashfile` on servers.
- Hunt for DLL side-loading using signed binaries from unusual paths.

### 10.4 Persistence

APT41 persistence has included web shells, Windows services, scheduled tasks and passive backdoors. Recent APT41 DUST reporting describes DUSTPAN persistence via Windows services with names such as `Windows Defend`.

Defensive opportunities:

- Monitor new services on servers, especially with names masquerading as Windows components.
- Hunt for services whose binary paths point to writable directories, web roots, temporary paths or unusual application directories.
- Detect new scheduled tasks with Microsoft-like paths but non-standard binaries.
- Compare service creation events against server role baselines.

### 10.5 Privilege Escalation

Public campaign reporting includes privilege escalation using named-pipe impersonation via BADPOTATO in the C0017 campaign. Credential access and token abuse may support privilege escalation and lateral movement.

Defensive opportunities:

- Hunt for exploitation tools executed shortly after web shell activity.
- Monitor suspicious token manipulation, named-pipe impersonation and service abuse.
- Detect local privilege escalation binaries written to temporary directories or web-accessible paths.

### 10.6 Defense Evasion

APT41 has used obfuscation, packing, file deletion, masquerading, code signing, in-memory execution and legitimate service abuse. Recent DUSTTRAP activity used stolen code-signing certificates and encrypted payloads executed in memory.

Defensive opportunities:

- Hunt for signed binaries with low prevalence or newly observed certificate chains.
- Detect binaries masquerading as legitimate Windows components but running from non-standard paths.
- Monitor file deletion after tool execution.
- Use first-seen analysis for signed executables, service binaries and DLLs loaded by trusted processes.

### 10.7 Credential Access

Public reporting describes APT41 use of credential dumping and credential theft techniques, including LSASS dumping with Procdump and Mimikatz, extraction of SAM/NTDS data and browser credential theft with tools such as BrowserGhost.

Defensive opportunities:

- Detect LSASS access by non-standard processes.
- Monitor `procdump` execution against LSASS.
- Hunt for `ntds.dit`, registry hive saves and access to SAM/SYSTEM/SECURITY hives.
- Detect browser credential store access on servers where browser use is not expected.
- Correlate credential dumping with subsequent lateral movement.

### 10.8 Discovery

APT41 discovery activity includes host, account, domain, network, share and configuration discovery. Public reporting describes use of native Windows commands and registry queries.

Defensive opportunities:

- Stack count discovery command sequences by host role.
- Identify rare discovery bursts from web servers, database servers and application servers.
- Correlate `net`, `whoami`, `ipconfig`, `nltest`, `tasklist`, `net view`, `net group` and registry queries within short windows after suspicious execution.

### 10.9 Lateral Movement

APT41 has used stolen credentials and pass-the-hash techniques for lateral movement. Mandiant reporting notes that the actor is adept at moving laterally within targeted networks, including movement across Windows and Linux systems.

Defensive opportunities:

- Detect lateral authentication from servers that do not normally initiate administrative sessions.
- Hunt for remote service creation, remote scheduled tasks, SMB admin share usage and pass-the-hash indicators.
- Correlate credential access events with authentication fan-out.

### 10.10 Collection

APT41 collection depends on mission context. Recent APT41 DUST reporting includes data collection from Oracle databases using SQLULDR2, local staging of exported CSV files and use of archive utilities. Earlier reporting includes collection of PII, call records, source code, certificates and production environment assets.

Defensive opportunities:

- Monitor database servers for unusual export utilities, bulk queries and large local output files.
- Hunt for archive creation in unusual directories on servers.
- Detect access to code-signing material, build pipelines and certificate stores.
- Monitor access to telecom call record systems, subscriber databases or other high-value datasets.

### 10.11 Command and Control

APT41 has used HTTP/HTTPS C2, Cobalt Strike, self-managed infrastructure, Cloudflare Workers, compromised Google Workspace accounts and web services. Recent reporting describes HTTPS C2 and use of Cloudflare or Cloudflare Workers.

Defensive opportunities:

- Hunt for new outbound HTTPS destinations from servers with low egress variability.
- Identify Cloudflare Worker or cloud-service destinations that are rare for a given server role.
- Use JA3/JARM, certificate, HTTP header and destination reputation enrichment as pivot points, while avoiding reliance on any single fingerprint.
- Detect Cobalt Strike behavioral patterns using process, network and memory telemetry.

### 10.12 Exfiltration

APT41 has exfiltrated or attempted to exfiltrate different data types depending on campaign objectives. Recent APT41 DUST reporting includes use of Microsoft OneDrive via PINEGROVE to transmit large volumes of sensitive data. MITRE maps APT41 DUST to exfiltration over web service and exfiltration to cloud storage.

Defensive opportunities:

- Monitor unusually large uploads to cloud storage from servers.
- Detect cloud storage usage from hosts or service accounts that do not normally use those services.
- Correlate archive creation, database exports and outbound web-service uploads.
- Hunt for OneDrive API usage from non-user endpoints or server networks.

## 11. ATT&CK Mapping

This section is not a complete mapping of every technique publicly associated with APT41. It prioritizes behaviors that are most useful for hunting, detection engineering and incident scoping.

| Tactic | Technique | APT41-Relevant Behavior |
|---|---|---|
| Reconnaissance | T1595 Active Scanning | Use of scanning tools and vulnerability scanners during target development. |
| Reconnaissance | T1596.005 Search Open Technical Databases: Scan Databases | Use of internet scan data and services such as FOFA in public reporting. |
| Resource Development | T1583.007 Acquire Infrastructure: Serverless | APT41 DUST used Cloudflare Workers / serverless infrastructure. |
| Resource Development | T1588.003 Obtain Capabilities: Code Signing Certificates | Use of stolen code-signing certificates. |
| Initial Access | T1190 Exploit Public-Facing Application | Exploitation of vulnerable internet-facing applications. |
| Initial Access | T1566 Phishing | Spear-phishing appears in public reporting as one of several access vectors. |
| Initial Access | T1195 Supply Chain Compromise | Injection of malicious code into legitimate software/update paths. |
| Execution | T1059.003 Windows Command Shell | Use of `cmd.exe` for reconnaissance and execution. |
| Execution | T1059.007 JavaScript/JScript | JScript web shells in public campaign reporting. |
| Persistence | T1505.003 Web Shell | Use of web shells including ANTSWORD and BLUEBEAM in recent reporting. |
| Persistence | T1543.003 Windows Service | DUSTPAN persistence via Windows services. |
| Privilege Escalation | T1134 Access Token Manipulation | Named-pipe impersonation / BADPOTATO in C0017 reporting. |
| Defense Evasion | T1027 Obfuscated Files or Information | Obfuscated, encrypted and packed payloads. |
| Defense Evasion | T1036.004 Masquerade Task or Service | Malware masquerading as legitimate Windows binaries or services. |
| Defense Evasion | T1553.002 Code Signing | Use of stolen code-signing certificates. |
| Defense Evasion | T1070.004 File Deletion | Deletion of artifacts after use. |
| Credential Access | T1003 OS Credential Dumping | LSASS, SAM and NTDS credential access in public reporting. |
| Credential Access | T1555.003 Credentials from Web Browsers | Browser credential theft tooling reported by Group-IB. |
| Discovery | T1087 Account Discovery | Use of native commands to enumerate users and groups. |
| Discovery | T1135 Network Share Discovery | Use of `net share` and `net view`. |
| Discovery | T1016 System Network Configuration Discovery | Network and registry-based discovery. |
| Lateral Movement | T1550.002 Pass the Hash | Pass-the-hash activity using stolen credentials. |
| Collection | T1213.006 Data from Information Repositories: Databases | Oracle database collection using SQLULDR2 in APT41 DUST. |
| Collection | T1119 Automated Collection | Automated collection using tools such as SQLULDR2 and PINEGROVE. |
| Collection | T1074.001 Local Data Staging | Exported database content staged locally before exfiltration. |
| Command and Control | T1071.001 Web Protocols | HTTP/HTTPS C2. |
| Command and Control | T1102 Web Service | Use of web/cloud services and compromised cloud accounts. |
| Exfiltration | T1567.002 Exfiltration to Cloud Storage | OneDrive exfiltration in APT41 DUST. |

## 12. Hunt Hypotheses

### Hunt Hypothesis 1: Exploitation-to-Web-Shell Chain on Internet-Facing Servers

**Hypothesis:** If APT41 or an actor using a similar playbook is attempting initial access, externally exposed web servers may show exploitation attempts followed by web shell placement and command execution through the web server process.

**Priority telemetry:** Web server logs, WAF logs, EDR process creation, file creation in web roots, network egress from web servers.

**Behavioral signals:**

- Web server process spawning command interpreters or transfer utilities.
- Newly created `.aspx`, `.jsp`, `.php` or script files in web-accessible directories.
- Suspicious POST requests to newly created or rarely accessed server-side scripts.
- Outbound connections from web servers to rare external destinations.
- Short sequence: exploit request → file write → web shell request → command execution.

### Hunt Hypothesis 2: Certutil-Based Payload Reconstruction or Transfer

**Hypothesis:** If APT41 operators are transferring payloads through living-off-the-land methods, servers may show `certutil.exe` execution for download, decode or hash verification shortly after web shell activity.

**Priority telemetry:** EDR process creation, command-line logging, web server process lineage, file creation events.

**Behavioral signals:**

- `certutil.exe` launched by `w3wp.exe`, `httpd.exe`, `nginx.exe`, `tomcat.exe`, `java.exe` or a command shell descended from a web process.
- `certutil -decode`, `certutil -urlcache`, `certutil -split` or `certutil -hashfile` on servers.
- Repeated `echo` commands writing Base64 chunks to disk.
- Newly decoded binaries in temporary directories, web roots or application paths.

### Hunt Hypothesis 3: Suspicious Windows Service Persistence Masquerading as Legitimate Components

**Hypothesis:** If APT41 establishes persistence after compromise, it may create Windows services with Microsoft-like names and binaries stored in unusual paths.

**Priority telemetry:** Windows service creation events, EDR process creation, registry changes, file creation.

**Behavioral signals:**

- New services with names resembling Windows security or management components.
- Service binaries located in writable directories, application paths or web roots.
- Service creation shortly after web shell or exploit activity.
- Service binaries with low prevalence, suspicious signing status or recent first-seen timestamp.

### Hunt Hypothesis 4: Database Collection and Local Staging

**Hypothesis:** If APT41 is collecting data from backend repositories, database servers may show unusual export utilities, large local CSV/text output and archive creation before outbound transfer.

**Priority telemetry:** Database audit logs, EDR file creation, process creation, command-line logs, network egress, DLP/cloud proxy logs.

**Behavioral signals:**

- Execution of database export tools from non-standard directories.
- Large CSV, TXT, DAT, RAR or ZIP files created on database servers.
- Archive utility execution following database export.
- Outbound transfers to cloud storage or rare HTTPS destinations.
- Database access outside normal service-account patterns.

### Hunt Hypothesis 5: Cloud Storage Exfiltration from Non-User Endpoints

**Hypothesis:** If APT41 uses cloud storage for exfiltration, servers or compromised accounts may upload staged archives to services such as OneDrive from endpoints that do not normally interact with those services.

**Priority telemetry:** Cloud proxy logs, CASB/SaaS logs, firewall/proxy logs, EDR network telemetry, identity logs.

**Behavioral signals:**

- OneDrive or other cloud storage uploads from server subnets.
- Large outbound uploads following local archive creation.
- OAuth or cloud account activity from unusual devices, geographies or autonomous systems.
- Service accounts or administrative accounts interacting with consumer or enterprise cloud storage unexpectedly.

### Hunt Hypothesis 6: Stolen or Rare Code-Signing Certificate Abuse

**Hypothesis:** If APT41 uses stolen code-signing material, the environment may contain signed binaries that are technically trusted but rare, newly observed or inconsistent with the claimed publisher.

**Priority telemetry:** File metadata, code-signing certificate details, EDR file prevalence, software inventory, binary execution logs.

**Behavioral signals:**

- Signed binaries with low global or enterprise prevalence.
- Recently first-seen signed executables executing from temporary, user-writable or application directories.
- Certificate reuse across unrelated binaries or hosts.
- Signed DLLs side-loaded by legitimate executables from non-standard paths.

### Hunt Hypothesis 7: Discovery Burst After Server-Side Exploitation

**Hypothesis:** If APT41 gains command execution through a web application, operators may run a burst of native discovery commands to determine host role, privileges, domain context and lateral movement paths.

**Priority telemetry:** Process creation, command-line logs, authentication logs, web server lineage.

**Behavioral signals:**

- `whoami`, `hostname`, `ipconfig`, `net user`, `net group`, `net localgroup`, `nltest`, `tasklist`, `net view`, `net share` executed within a short window.
- Discovery commands launched from web server process descendants.
- Discovery on servers where interactive administrative activity is rare.
- Follow-on credential access, lateral authentication or file staging.

## 13. Priority Telemetry Sources

| Telemetry Source | Why It Matters for APT41 Hunting |
|---|---|
| EDR process creation and command-line telemetry | Detect web shell execution, living-off-the-land utilities, credential dumping, service creation and archive tooling. |
| Web server and reverse proxy logs | Reconstruct exploitation, web shell access and suspicious HTTP requests. |
| WAF logs | Identify exploit attempts against internet-facing applications. |
| File creation and modification telemetry | Detect web shell placement, payload decoding, staged archives and unusual service binaries. |
| Windows service and scheduled task events | Detect persistence and masquerading. |
| Authentication logs | Detect lateral movement, credential reuse, pass-the-hash-like patterns and abnormal administrative access. |
| Database audit logs | Detect unusual export, bulk queries and access to sensitive repositories. |
| DNS, proxy and firewall logs | Detect C2, rare destinations, cloud-service usage and exfiltration. |
| SaaS / cloud storage audit logs | Detect OneDrive or other web-service exfiltration, compromised account usage and unusual uploads. |
| Certificate and binary metadata | Detect stolen certificate abuse, rare signed binaries and side-loading opportunities. |
| Vulnerability management and external attack surface data | Prioritize exposed systems likely to be targeted through rapid exploit adoption. |

## 14. Detection and Hunting Opportunities

### 14.1 Behavioral Clustering Opportunities

APT41 lends itself well to behavioral clustering rather than static IOC matching. Useful clusters include:

- Web exploitation followed by web server child process execution.
- Web shell command execution followed by certutil payload transfer.
- Discovery bursts from server processes.
- Credential dumping followed by lateral authentication fan-out.
- Database export followed by archive creation and cloud upload.
- New Windows services with low-prevalence binaries.
- Rare signed binaries executed from unusual paths.
- Cloud storage access from server subnets or service accounts.

### 14.2 First-Seen Analysis Opportunities

Apply first-seen analysis to:

- New service names and service binary paths.
- New executable hashes on servers.
- New signed binaries from unfamiliar publishers.
- New outbound destinations from web and database servers.
- New cloud service interactions from non-user endpoints.
- New web-accessible scripts in application directories.

### 14.3 Ephemeral Baselining Opportunities

Build temporary baselines around:

- Normal web server child processes by application pool or host role.
- Normal outbound destinations from internet-facing servers.
- Normal database export activity per database server.
- Normal OneDrive/cloud storage activity by subnet and host role.
- Normal Windows service creation by administrative tool and change window.
- Normal administrative authentication paths between server tiers.

## 15. Intelligence Gaps

| Gap | Why It Matters | Collection Priority |
|---|---|---:|
| Current APT41 infrastructure fingerprints | Static IOCs age quickly; recent infrastructure may rely on cloud/service abstraction. | High |
| Current exploitation priorities | The actor adapts rapidly to vulnerable internet-facing applications. | High |
| Sector-specific targeting after 2024 | Recent reporting shows renewed targeting in logistics, media, technology and automotive. | Medium |
| Updated malware lineage across DUSTPAN/DUSTTRAP and related tools | Helps connect new samples to known activity. | Medium |
| Cloud account abuse patterns | Recent reporting includes compromised Google Workspace accounts and OneDrive exfiltration. | High |
| Supply-chain compromise indicators | High-impact but hard to detect without build pipeline and software integrity telemetry. | Medium |
| Operator working patterns | Useful for profiling but less reliable than technical behaviors. | Low to Medium |

## 16. Recommended Hunting Backlog

1. Build a hunt for web server processes spawning shells, scripting engines or transfer utilities.
2. Build a certutil misuse hunt scoped to servers and web process descendants.
3. Baseline new Windows service creation across internet-facing and database servers.
4. Hunt for database export utilities, large CSV/TXT output and archive creation on database servers.
5. Hunt for cloud storage uploads from server subnets, especially OneDrive and other web services.
6. Build first-seen analytics for signed binaries with low prevalence or suspicious execution paths.
7. Correlate vulnerability exposure data with exploitation attempts and post-exploitation process chains.
8. Hunt for discovery command bursts from non-interactive service contexts.
9. Build detection around web shell file creation and suspicious POST access patterns.
10. Correlate archive creation, staging directory use and outbound HTTPS uploads.

## 17. Indicators of Compromise

The indicators below are extracted from public reporting, primarily Mandiant's 2024 reporting on APT41 DUST. They should be treated as historical, source-derived indicators for scoping, retro-hunting and enrichment. They should not be used as the sole basis for attribution or blocking decisions without additional context.

### 17.1 Host-Based Indicators

| Indicator | Type | Associated Family / Tool | Notes | Source |
|---|---|---|---|---|
| `fcff642268898fcf65702a214aefbf9e` | MD5 | SQLULDR2 | Oracle database export utility observed in APT41 DUST activity. | Mandiant, 2024 |
| `ac125aea0b703de37980779599438b4a` | MD5 | PINEGROVE | OneDrive uploader observed in APT41 DUST exfiltration workflow. | Mandiant, 2024 |
| `17d0ada8f5610ff29f2e8eaf0e3bb578` | MD5 | DUSTPAN | In-memory dropper; filename reported as `aclui.dll`. | Mandiant, 2024 |
| `35f650c94faf6a2068e8238dd99edbea` | MD5 | DUSTPAN | In-memory dropper; filename reported as `conn.exe`. | Mandiant, 2024 |
| `3bb44c0dd7f424864d76d4df09538cb6` | MD5 | DUSTPAN | Reported as `PrintWorkflowUserSvc_a0c15f9d.dll` / `cbi.dll`. | Mandiant, 2024 |
| `9991ce9d2746313f505dbf0487337082` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `dbgeng.dll`. | Mandiant, 2024 |
| `c33247bc3e7e8cb72133e47930e6ddad` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `dbgeng.dll`. | Mandiant, 2024 |
| `cfce85548436fb89a83bf34dc17f325d` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `hostfxr.dll`. | Mandiant, 2024 |
| `e98b9e21928252332edf934f3d18ac21` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `dbgeng.dll`. | Mandiant, 2024 |
| `8222352a61eacca3a1c6517956aa0b55` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `dbgeng.dll`. | Mandiant, 2024 |
| `dc725f5e9b1ae062fbec86ee4d816b45` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `d72f202c1d684c9a19f075290a60920f` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `Sbiedll.dll`. | Mandiant, 2024 |
| `393065ef9754e3f39b24b2d1051eab61` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `atstrust.dll`. | Mandiant, 2024 |
| `0e74285f3359393e57f5d49c156aca47` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `aca5c6daecf463012a09564764584937` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename reported as `dbgeng.dll`. | Mandiant, 2024 |
| `336a0d6f8cc92bf9740ce17de600463b` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `6bc4a92ff4d2cfc9da91ae6a5d2ad3d5` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `a689e182fe33b9d564dddc35412ea0a7` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `e4a4aafb49b8c86a5ac087ae342c0ee6` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `e584119a4766e6cf49093c666965c8be` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |
| `f1769ad5a9dc44794895275c656ed484` | MD5 | DUSTTRAP | Multi-stage plugin framework; filename not reported. | Mandiant, 2024 |

### 17.2 Network-Based Indicators

| Indicator | Type | Associated Family / Tool | Notes | Source |
|---|---|---|---|---|
| `ns2[.]akacur[.]tk` | Domain | BEACON | Reported C2-related domain. | Mandiant, 2024 |
| `ns1[.]akacur[.]tk` | Domain | BEACON | Reported C2-related domain. | Mandiant, 2024 |
| `orange-breeze-66bb[.]tezsfsoikdvd[.]workers[.]dev` | Cloudflare Worker domain | BEACON | Reported Cloudflare Workers C2 channel. | Mandiant, 2024 |
| `www[.]eloples[.]com` | Domain | DUSTTRAP | Reported as first observed on 2024-02-21 and last observed on 2024-07-16. | Mandiant, 2024 |
| `95.164.16[.]231` | IP address | DUSTTRAP | Related to DUSTTRAP FQDN `www[.]eloples[.]com`. | Mandiant, 2024 |
| `152.89.244[.]185` | IP address | DUSTPAN | Used to deliver DUSTPAN; first activity reported on 2023-03-21. | Mandiant, 2024 |
| `hxxp://152.89.244[.]185/conn.exe` | URL | DUSTPAN | Reported DUSTPAN delivery URL. | Mandiant, 2024 |

### 17.3 Code-Signing Certificate Indicators

| Serial Number | Issuer | Subject | Notes | Source |
|---|---|---|---|---|
| `6f:97:f1:3d:a5:5e:9f:70:a6:92:7e:d1:b3:3e:ee:ee` | `thawte SHA256 Code Signing CA` | `CCR INC` | Certificate associated with a South Korean gaming-sector company and reported as abused in DUSTTRAP-related activity. | Mandiant, 2024 |
| `05:fa:8a:72:da:46:07:4f:de:1e:34:c7:46:61:ee:00` | `DigiCert SHA2 Assured ID Code Signing CA` | `OOO ALEAN-TOUR` | Reported as abused in DUSTTRAP-related activity. | Mandiant, 2024 |
| `0a:2c:bf:9b:18:fe:1b:20:b9:4e:ca:c4:b0:78:b8:c1` | `DigiCert SHA2 Assured ID Code Signing CA` | `Gala Lab Corp.` | Certificate associated with a South Korean gaming company and reported as abused in DUSTTRAP-related activity. | Mandiant, 2024 |

## 18. References

- Mandiant / Google Cloud, "APT41: A Dual Espionage and Cyber Crime Operation" — https://cloud.google.com/blog/topics/threat-intelligence/apt41-dual-espionage-and-cyber-crime-operation
- Mandiant / Google Cloud, "APT41 Has Arisen From the DUST" — https://cloud.google.com/blog/topics/threat-intelligence/apt41-arisen-from-dust
- MITRE ATT&CK, "APT41, Group G0096" — https://attack.mitre.org/groups/G0096/
- MITRE ATT&CK, "APT41 DUST, Campaign C0040" — https://attack.mitre.org/campaigns/C0040/
- MITRE ATT&CK, "C0017" — https://attack.mitre.org/campaigns/C0017/
- Group-IB, "4 malicious campaigns and a new wave of APT41 attacks" — https://www.group-ib.com/blog/apt41-world-tour-2021/
- U.S. Department of Justice, "Seven International Cyber Defendants, Including 'Apt41' Actors, Charged In Connection With Computer Intrusion Campaigns Against More Than 100 Victims Globally" — https://www.justice.gov/archives/opa/pr/seven-international-cyber-defendants-including-apt41-actors-charged-connection-computer
- FBI, "APT 41 Group" — https://www.fbi.gov/wanted/cyber/apt-41-group

