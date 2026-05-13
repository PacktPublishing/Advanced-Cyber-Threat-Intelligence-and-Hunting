# CTI SOP: Emerging Threat Response
---
# Introduction
This document defines the standard procedure that the Cyber Threat Intelligence (CTI) team follows when responding to an emerging threat. It exists to ensure that responses are consistent, defensible and timely regardless of which analyst is on shift or which trigger arrives.

CTI work, by its nature, is reactive to external events that arrive on irregular schedules and with varying levels of clarity. Without a defined procedure, response quality becomes a function of who happened to be on shift that day. With one, the team operates as a system: predictable in tempo, consistent in output, and able to scale across analysts of different experience levels.

This document is a field-ready procedure broken into five phases, with steps and sub-steps under each. It is structured as a cheat sheet, with procedures that define *what* must be done at each phase and *why*.
## Target Audience
* **CTI analysts** — as the operating procedure during active response.
* **CTI team leadership** — as the basis for SLA negotiation with internal stakeholders, capacity planning and quality review.
* **Stakeholder teams** (SOC, Detection Engineering, Vulnerability Management, IR, Threat Hunting, Platform Owners, CISO) — so they understand what CTI will produce, when and what is expected of them in return.
## Scope
The procedure applies to any CTI trigger, such as vulnerabilities, campaigns, actor profiles, malware reports, peer incidents, sectoral targeting shifts, geopolitical events with cyber implications, and supply chain compromises. The trigger types are listed below; the same five phases execute for each. What changes between trigger types is the content of the analysis at each step, not the procedural spine.
The procedure does *not* cover:
* Incident Response (handled by the IR procedure once a confirmed incident is declared)
* Routine threat tracking outside of an emerging-threat response (handled by continuous CTI operations)
* Tactical IOC ingestion and feed management (handled by the detection pipeline)
## Trigger Types
This SOP applies to all of the following CTI triggers:
1. **Vulnerability:** CVE, zero-day, vendor advisory, exploit disclosure.
2. **Campaign report:** Named or unnamed campaign with TTPs, victimology and infrastructure.
3. **Threat actor profile:** New actor, updated profile, attribution shift, capability change.
4. **Malware family report:** New family, significant variant or capability evolution.
5. **Peer organization incident:** Peer organization compromise, ISAC sharing, sectoral incident disclosure.
6. **Sectoral targeting shift:** Reporting indicating the relevant sector has entered an actor's targeting scope.
7. **Geopolitical event with cyber implications:** Sanctions, conflict escalation or state-level posture shifts that historically correlate with cyber activity.
8. **Supply chain compromise:** Vendor, upstream provider or dependency compromised.
---
# Phase 1: Intake & Initial Triage
**1.1 Assign single owning analyst**
One analyst owns the item end-to-end through at least Phase 4 to avoid diffusion of responsibility. Hand-offs are documented in the tracking entry.

**1.2 Verify source credibility**
* **Assess source tier:** Credibility is source-tier-aware, not source-count-driven. A single report from a Tier 1 vendor (*Mandiant*, *CrowdStrike*, *Microsoft*, *Google TIG*, and equivalents) with strong attribution history is sufficient to act on, as waiting for corroboration would miss the response window. Mid-tier sources warrant corroboration but should not block initial work. Low-tier or unknown sources should be treated as leads, not intel, and require corroboration before action. Anonymous or unverified sources are discarded unless corroborated.
* **Confirm authenticity of the trigger:** Validate that the underlying artifact is genuine, CVE exists and is published, campaign report comes from the named vendor, ISAC sharing has expected provenance, peer report is not a fabrication or recycled material.

**1.3 Produce one-paragraph internal summary**
A plain-language description capturing the essence of the trigger, written for internal CTI use only, not a deliverable. For vulnerabilities: what it is, prerequisites, impact, exploitability. For campaigns: actor (if known), victimology, TTPs, infrastructure. For actor profiles: capability, intent, historical targeting. For peer incidents and supply chain events: what happened, when, what is confirmed vs. speculated. This summary anchors the analyst's understanding and seeds downstream artifacts.

**1.4 Assign provisional severity (intrinsic properties only)**
Assign severity level (P1 Flash / P2 Priority / P3 Standard / P4 Informational). Provisional tiering reflects the threat in the abstract, without considering internal exposure. For vulnerabilities: exploitability, scope, patch availability. For campaigns and actors: capability, intent, sophistication, operational tempo. For peer incidents and supply chain events: severity of the underlying compromise and proximity to our environment.

**1.5 Record analysis in the team's tracking system**
Capture ID, timestamp, owning analyst, source, source tier, one-paragraph summary, and provisional severity in whatever tracking system the team uses — a Threat Intelligence Platform (TIP), a wiki, GitLab, ticketing system, or equivalent. This record is the single source of truth for the response and serves both audit and handover purposes; no separate triage document is needed.

**1.6 Decide whether a Threat Advisory is warranted**
Not every item earns a formal advisory. A Threat Advisory is typically issued when the provisional severity is P1 or P2, or when external chatter is high enough that leadership will be asked about it.

**1.7 Issue Threat Advisory (deliverable, conditional)**
A short document, usually distributed via email, with narrow distribution: CISO/security leadership, SOC leadership, and key platform owners. Content includes what we know, provisional severity, expected next steps and timing. Explicitly states that a Threat Assessment will follow with exposure picture, detection guidance, and hunt package.
---
# Phase 2: Exposure Assessment
Exposure is multi-dimensional. Depending on the trigger type, the analyst evaluates one or more of the following dimensions:
* **Technical exposure:** Do we run the affected technology, software, or service? *(dominant for vulnerabilities and malware reports)*
* **Targeting exposure:** Does the actor or campaign target our sector, geography, or organization type? *(dominant for actor profiles, campaign reports, sectoral shifts, geopolitical events)*
* **Tradecraft exposure:** Are we susceptible to the specific TTPs in question given current control coverage? *(dominant for campaigns and malware)*
* **Relational exposure:** Are we connected to victims via shared infrastructure, supply chain, peering, partnerships, or M&A activity? *(dominant for peer incidents and supply chain compromises)*
* **Detection exposure:** Do we have visibility into the relevant TTPs or IOCs, or are we blind to this activity class? *(always relevant)*

**2.1 Define exposure scope precisely**
* **Technical scope (where applicable):** Affected versions, configurations, deployment patterns. Translate the trigger into specific, queryable identifiers (i.e. `curl version < 8.19.0`), not generic labels like "Linux" or "Windows".
* **Targeting / tradecraft scope (where applicable):** TTPs, tooling, infrastructure patterns, victimology characteristics. Translate the trigger into observable behaviors and control coverage questions.
* **Relational scope (where applicable):** Specific vendors, partners, peering relationships, or shared infrastructure that create exposure pathways.
* **High-risk preconditions:** Call out the contexts that materially raise impact, such as multi-tenant hosts, container platforms, internet-exposed services, identity systems, etc.

**2.2 Determine exposure via available means**
Use whatever path produces the answer fastest and most accurately. Where CTI has direct, self-service access to relevant data sources — CMDB, asset inventory tools, EDR consoles, cloud provider consoles, IAM systems, vulnerability scanners, IAM/access management software — query them directly. Where direct access is not available, dispatch a query to the relevant domain owner.

**2.3 Identify relevant domains and owners**
* **Asset domains (technical exposure):** For example corporate IT, OSS, BSS, 5G mobile core, cloud platform, lab and dev, and so on.
* **Control domains (tradecraft exposure):** Detection Engineering, SOC tooling owners, identity and access management, network security, endpoint security, and so on.
* **Relational domains (relational exposure):** Vendor management, procurement, M&A, partnerships, peering and interconnect. Each domain has its own owner; the analyst maps the exposure scope to the right combination of domains rather than blasting all of them.

**2.4 Execute exposure queries**
* **For domains where CTI has self-service access:** Query directly via the available tooling and capture results in the tracking system.
* **For domains requiring dispatched queries:** Send the templated query via the pre-built domain owner contact map.

**2.5 Collect and consolidate responses**
* **Affected scope:** For technical exposure: affected asset counts. For tradecraft exposure: coverage gaps against the relevant TTPs. For relational exposure: count of relevant vendor/partner relationships. Precision is not required at this stage; an order-of-magnitude picture is enough to drive tiering and notification.
* **Concentration by domain:** Where the exposure clusters matters more than the total — for example, 500 affected assets in the 5G mobile core network is a different problem than 500 across corporate laptops; a detection gap in identity systems is more material than one in a tertiary domain.
* **Outstanding / non-responding domains:** Track domains that haven't responded so the picture is honest and gaps are visible.

**2.6 Assign contextual severity (final tier)**
Adjust the provisional tier up or down based on actual exposure across the relevant dimensions. The same trigger — whether a CVE, a campaign, or an actor profile — can be P1 for one organization and P4 for another depending on stack, sector relevance, control coverage, and relationships.

**2.7 Document confidence level (high / medium / low + rationale)**
State explicitly how complete and reliable the exposure picture is. Confidence is part of the deliverable, not a footnote.

**2.8 Gate decision (proceed / deprioritize / close)**
Decide whether to escalate to Phase 3, downgrade to passive tracking, or close the item. This is the moment where CTI prevents wasted cycles across the organization.
---
# Phase 3: Deep-Dive Analysis
**3.1 Deep threat analysis**
This is where the analyst develops genuine understanding of the threat and translates it into the organization’s environment. Expect this step to consume the bulk of Phase 3 effort, as it is the analytical heart of CTI work.
* **Fully understand the trigger:** For a vulnerability: the underlying mechanism, exploitation path, prerequisites, attacker capability needed, patch behavior, bypass potential. For a campaign: the full kill chain, tooling, infrastructure patterns, evolution over time, public detection coverage. For an actor: capability profile, intent and motivation, historical operational tempo, attribution confidence and dissenting views. For a supply chain or peer incident: what is confirmed vs. speculated, how the compromise unfolded, attacker dwell time and objectives.
* **Translate to our environment:** Given how the threat actually works, where would it land in our stack? Which assets, identities, or data would be the realistic targets? What would the path from initial access to objective look like in our architecture? This is where the abstract threat becomes concrete to us.
* **Build realistic attack scenarios:** Not theoretical worst case, but plausible end-to-end scenarios that would actually unfold given our environment, controls, and the attacker's known behavior. Worked scenarios drive better detection logic and hunt hypotheses than abstract TTP lists.
* **Map to detection and prevention coverage:** For each scenario, identify what we would see, where we would see it, and what we would miss. Detection exposure is an output of this analysis, not an input — it cannot be assessed until the scenarios are constructed.
* **Operational tempo and exploitation status:** In-the-wild exploitation reporting for vulnerabilities; observed campaign activity and target selection for campaigns and actors; sector-specific targeting indicators across all trigger types.
* **Actor and capability attribution:** Which actors are known to weaponize the vulnerability class, run the campaign, or share infrastructure/tooling with the reported activity. Be explicit about attribution confidence, as actor attribution is rarely certain.

**3.2 Peer & ISAC engagement**
* **Trust group chatter:** Pull on trusted communities for non-public observations and early warning.
* **Direct contact with peer CTI teams:** Targeted outreach to peers in the same sector to compare exposure and detection signal.

**3.3 Detection development support**
* **Validate logic against realistic tradecraft:** CTI ensures detections target real attacker behavior, not just published PoCs or surface IOCs that attackers will rotate. For campaigns and actors, focus on durable behavioral signatures rather than ephemeral infrastructure.
* **Map to frameworks:** Anchor detection coverage to ATT&CK techniques for consistency with the broader detection program.

**3.4 Hunt package development**
Assist Detection Engineering in creating a structured hunt package, so hunters can execute without re-deriving context and results are evaluable against a defined bar.

**3.5 Working-level coordination throughout**
CTI maintains continuous informal contact with SOC, Detection Engineering, Threat Hunting, and relevant teams via Teams/Slack during this phase. Recipient teams should not wait for the Threat Assessment to begin work; they should already have early access to draft detection logic and hunt hypotheses.
---
# Phase 4: Stakeholder Notification & Final Threat Assessment
**4.1 Draft Threat Assessment**
* **Executive Summary:** Brief, no jargon, written for leadership who will be asked about this by the CTO or board.
* **Timeline:** A timeline of when/how the events unfolded.
* **Technical Summary:** For vulnerabilities: mechanics, prerequisites, exploitation tradecraft. For campaigns and actors: TTPs, infrastructure, victimology. For peer incidents and supply chain events: what happened, attribution status, observed tradecraft.
* **Exposure Picture:** The Phase 2 output across all relevant dimensions: where we are exposed, how much, with what confidence.
* **Recommended Actions:** Specific, addressed asks, not generic guidance. Actions vary by trigger: patching and mitigation for vulnerabilities; detection deployment and hunting for campaigns and actors; vendor engagement and contract review for supply chain events.
* **Detection Guidance:** The Phase 3 outputs, packaged for direct consumption by SOC, Detection Engineering and Threat Hunting.
* **IOCs:** Any indicators available; explicitly note when none yet exist.
* **Confidence & Sources:** Sources cited; confidence levels stated per major claim.

**4.2 Distribute via defined channel**
A single source-of-truth artifact, typically a formal PDF shared via email. This avoids fragmented messaging.

**4.3 Notify recipients per tier matrix**
Recipient list is trigger-dependent, as not every team is relevant to every trigger. The full set of potential recipients:
* **Vulnerability Management:** Patch and mitigation ownership. Receives technical detail and patch/mitigation guidance.
* **SOC / Detection Engineering:** Receives the finalized detection guidance, building on working-level exchanges from Phase 3. Relevant for all trigger types.
* **IR / Threat Hunting:** Receives the structured hunt package as a formal handoff. Relevant for all trigger types.
* **Infrastructure / Platform owners:** Owns the change windows and operational risk of patching, mitigation, or architectural changes.
* **NOC / Network Engineering:** If relevant to threat.
* **Vendor Management / Procurement:** For supply chain triggers. Owns vendor engagement, contract review, and external escalation when a vendor is implicated.
* **CISO / Security Leadership:** Needs to be ahead of the question, not behind it.

**4.4 Require acknowledgment from each recipient team**
Every recipient acknowledges receipt, even a thumbs-up reaction is enough. Creates an audit trail and prevents "*we never heard about this*" later.

**4.5 Log distribution and acknowledgments**
The tracking entry now records who was notified, when, and who acknowledged. Closes the loop on the notification phase.
# Phase 5: Tracking & Follow-Up
**5.1 +7 day checkpoint**
* **In-the-wild exploitation status:** Has the threat picture shifted since the Threat Assessment was issued?
* **Patch, mitigation, or control coverage:** What percentage of exposed assets are remediated, or what percentage of relevant TTPs are covered by detection / preventive controls?
* **Hunt completion status:** Have the hunts run, and what did they return?
* **Detection deployment status:** Are detections live, tuned, and producing signal?

**5.2 +30 day checkpoint**
* **Close-out or roll into ongoing tracking:** Decide whether the item is closed, or whether it becomes part of standing threat tracking.
* **Re-tier if exploitation has emerged:** Severity is not static; reassess as the threat landscape evolves.

**5.3 Lessons learned capture**
* **Procedure gaps:** Where did the SOP fail or feel awkward?
* **Domain owner map gaps:** Which domains (asset, control, or relational) were slow, missing, or wrongly mapped?
* **SLA breaches:** Where did response timing miss agreed targets, and why?

**5.4 Archive in CTI knowledge base**
The Threat Advisory, Exposure Assessment, Hunt Package and Threat Assessment are archived together, as this becomes reference material for future similar events.
---
# Appendix: Cross-Cutting Principles
* **CTI informs and enables; CTI does not own action.** The team is the authoritative source on the threat, not the executor of patching, detection, hunting, or vendor engagement.
* **One channel, one artifact, one owner per formal deliverable.** Fragmented comms are the failure mode under pressure.
* **Acknowledgment from recipient teams is mandatory.** Action stays with the recipient; awareness is auditable.
* **Tiering is provisional → contextual; exposure assessment is the gate.** Internal severity is the only severity that drives action.
* **All artifacts are timestamped, versioned, and stored centrally.** The tracking entry is the spine of the response.
* **Persistent learnings flow into the team's knowledge base continuously.** Actor profiles, campaign trackers, and recurring TTP notes are maintained as part of normal CTI operations.