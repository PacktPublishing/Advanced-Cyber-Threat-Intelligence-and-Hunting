# CTI Severity Tier Matrix
---
# Introduction
A 2D scoring model for assigning internal severity to CTI triggers (vulnerabilities, campaigns, malware reports, peer incidents, sectoral targeting shifts, geopolitical events, and supply chain compromises). Used in SOP steps 1.4 (provisional severity) and 2.6 (contextual/final severity).

Threat actor profiles are scored separately using the *Threat Actor Scoring Model*, which produces a persistent *Threat Score* (TS) that maps to Axis 1 of this matrix at the moment of response. See the Actor Threat Scoring Model document for the full scoring framework.

The model scores two axes independently and reads the final tier from the grid:
* **Threat Severity:** Intrinsic properties of the threat itself, independent of our environment
* **Exposure Level:** Specific to our environment, output of Phase 2
---
# Axis 1: Threat Severity
Scored from the threat itself, before any consideration of internal exposure. Used to assign **provisional severity** in Phase 1. Different trigger types are scored against different facets, but all map to the same four levels.

**Facets considered:**
* For **vulnerabilities and malware**: impact (RCE, privilege escalation, data exposure), prerequisites for exploitation, exploit availability (PoC, in-the-wild use), patch status.
* For **campaigns**: sophistication, scale, operational tempo, success rate against comparable victims, infrastructure resilience.
* For **peer incidents**: severity of the underlying compromise, attacker sophistication observed, time-to-impact.
* For **geopolitical events**: historical correlation with cyber activity, scope of likely affected actors, timing.
* For **supply chain compromises**: severity of the vendor compromise, reach of the affected dependency, attacker access scope.

**Critical Severity**
* **Vulnerability/malware pattern**: unauthenticated RCE, authentication bypass, or equivalent direct compromise; no/minimal prerequisites; public PoC or in-the-wild exploitation; no patch or patch <72h old.
* **Campaign pattern**: highly sophisticated, active campaign with high success rate, broad targeting including our sector, no public detection guidance yet.
* **Peer incident pattern**: confirmed compromise at a directly comparable peer, sophisticated attacker, recent (<30 days).
* **Supply chain pattern**: confirmed compromise of a widely deployed vendor or dependency, attacker has had distribution capability.

**High Severity**
* **Vulnerability/malware pattern**: privilege escalation, sensitive data exposure, or significant lateral movement enabler; public PoC or credible exploitation reporting; patch available but not broadly deployed.
* **Campaign pattern**: capable, active campaign with relevant TTPs; sector adjacency rather than direct sector targeting; some public detection guidance emerging.
* **Peer incident pattern**: confirmed compromise at a sector peer, attribution unclear or attacker sophistication moderate.
* **Supply chain pattern**: compromise of a vendor with meaningful but bounded reach, or dependency in widespread use.

**Medium Severity**
* **Vulnerability/malware pattern**: meaningful impact constrained by prerequisites (local access, specific config, chained exploitation); PoC may exist; no confirmed in-the-wild use.
* **Campaign pattern**: lower sophistication, established TTPs with broad public detection coverage, no direct sector targeting.
* **Peer incident pattern**: confirmed incident at an adjacent (non-sector) organization, limited applicability.
* **Supply chain pattern**: compromise of a vendor with limited reach or where impact is well-bounded.

**Low Severity**
* **Vulnerability/malware pattern**: limited impact, theoretical or hard-to-exploit, edge-case configurations; no PoC.
* **Campaign pattern**: low sophistication, well-detected commodity activity, no telco-sector relevance.
* **Peer incident pattern**: distant or weakly relevant; reported for tracking only.
* **Supply chain pattern**: minimal reach, well-bounded impact, or vendor not in use by us.
---
# Axis 2: Exposure Level
Scored from the Phase 2 exposure assessment. Used together with threat severity to assign **contextual severity** in step 2.6. Exposure is multi-dimensional, as the analyst combines the relevant dimensions for the trigger type:
* **Technical exposure:** Do we run the affected technology?
* **Targeting exposure:** Does the actor or campaign target our sector, geography, or organization type?
* **Tradecraft exposure:** Are we susceptible to the relevant TTPs given current control coverage?
* **Relational exposure:** Are we connected to victims via shared infrastructure, supply chain, peering, or partnerships?
* **Detection exposure:** Do we have visibility into the relevant TTPs or IOCs, or are we blind to this activity class?

**Extensive Exposure**
* **Technical**: large affected asset population (organization-wide or thousands of assets) AND concentration in critical domains (Mobile Core, OSS/BSS, RAN, Transport, internet-exposed infrastructure, identity systems).
* **Targeting**: we are squarely in the actor's or campaign's stated or observed target set.
* **Tradecraft**: relevant TTPs map to material control gaps; no compensating detection or prevention in place.
* **Relational**: direct dependency on a compromised vendor, or shared infrastructure with confirmed victims.
* **Detection**: no visibility into the TTP class in question; we would not see this activity if it were happening.

**Significant Exposure**
* **Technical**: meaningful asset population (hundreds to low thousands) OR concentration in critical domains regardless of total count.
* **Targeting**: we are sector- or geography-adjacent to the targeted population; partial overlap.
* **Tradecraft**: relevant TTPs map to partial control coverage; detection exists but is incomplete or unvalidated.
* **Relational**: indirect dependency on a compromised vendor, or peering with affected parties.
* **Detection**: partial visibility into the TTP class; we might see this activity but with low confidence.

**Limited Exposure**
* **Technical**: small asset population (tens of assets) in non-critical domains, or critical-domain assets covered by strong compensating controls.
* **Targeting**: outside the targeted population but with weak adjacency signals.
* **Tradecraft**: relevant TTPs largely covered by existing controls; only edge cases uncovered.
* **Relational**: distant or non-material connection to affected parties.
* **Detection**: solid visibility; we would likely catch this activity.

**⠀None / Negligible Exposure**
* **Technical**: no affected assets, or affected assets fully isolated/mitigated by existing controls.
* **Targeting**: outside the targeted population with no adjacency signals.
* **Tradecraft**: relevant TTPs fully covered by existing controls.
* **Relational**: no dependency, peering, or shared infrastructure with affected parties.
* **Detection**: comprehensive visibility into the activity class.
---
# The Tier Grid
|  | **Exposure:** **None** | **Limited** | **Significant** | **Extensive** |
|:-:|:-:|:-:|:-:|:-:|
| **Critical** | P3 | P2 | **P1** | **P1** |
| **High** | P4 | P3 | P2 | **P1** |
| **Medium** | P4 | P3 | P3 | P2 |
| **Low** | P4 | P4 | P4 | P3 |

**Reading the score**
* Find the row matching the threat severity (Axis 1)
* Find the column matching the exposure level (Axis 2)
* The cell value is the final contextual severity

**Design notes**
* **Asymmetric weighting**: High-exposure pulls the tier up faster than low-exposure pulls it down. A Critical vulnerability with no current exposure still warrants P3 tracking because exposure can emerge (new deployments, missed inventory, supply chain shifts), whereas a Low vulnerability with extensive exposure is still capped at P3 because the impact ceiling limits risk.
* **No P1 below Critical/High**: A P1 designation requires both meaningful threat severity *and* meaningful exposure. This prevents tier inflation.
* **No automatic close**: Even the lowest cell (Low / None) is P4 — tracked, not ignored.
---
# Re-Tiering
Severity is not fixed at assignment. Re-tiering events:
* **In-the-wild exploitation or active campaign use confirmed** → re-evaluate threat severity upward
* **New exposure surfaces** (missed inventory, new deployment, newly discovered vendor relationship, control gap identified) → re-evaluate exposure level upward
* **Mitigation coverage reaches threshold** (patches deployed, detections live and validated, controls tightened, vendor remediated) → re-evaluate exposure level downward
* **Threat actor of concern adopts the technique or vulnerability** → re-evaluate threat severity upward, regardless of patch status
* **Sectoral targeting shift** (actor or campaign moves into telco scope) → re-evaluate threat severity upward
* **Attribution downgrade** (e.g. attacker reassessed as commodity rather than APT) → re-evaluate threat severity downward

Re-tiering is logged in the team's tracking system with timestamp, rationale, and old/new tier values.

---
# Worked Example 1: CVE-2026-31431 (Copy Fail)
A vulnerability case, end-to-end against the model.
### Threat severity scoring
* Local privilege escalation (not RCE) → not Critical on impact alone
* Public PoC available at disclosure, broadly reliable across distros
* Patch available shortly after disclosure
* Requires local code execution as unprivileged user (prerequisite)
* **Score: High**

### Exposure scoring (hypothetical EU telco organization)
* **Technical**: Linux kernel vulnerability affecting all kernels since 2017 → very broad. Telco environment runs 5G core on Kubernetes, OSS/BSS Linux estate, CI/CD runners, corporate Linux fleet.
* **Targeting**: Low relevance, as LPE is opportunistic, not actor-driven at this stage.
* **Tradecraft**: Detection gap for AF_ALG abuse and unprivileged-to-root transitions.
* **Detection**: Partial visibility on Linux endpoints; gaps on Kubernetes nodes.
* **Combined score: Extensive**

### Final Tier Calculation
* Row: High / Column: Extensive → **Final tier: P1**

### Downstream actions
* Threat Advisory issued same day to CISO, SOC lead, VM lead, mobile core platform owner, OSS platform owner
* Exposure assessment combines self-service queries (CMDB, EDR consoles, vulnerability scanner for corporate IT and cloud-managed Linux fleet) with dispatched queries to mobile core, OSS, and transport domain owners
* Threat Assessment targeted within 48h with full hunt package (auditd rules for AF_ALG socket creation by non-root users, eBPF anomaly hypotheses, unprivileged-to-root transition hunts)
* Daily tracking until patch coverage reaches threshold

This is the kind of case where the matrix output materially changes the response, as a naive read of "LPE, CVSS 7.8" might suggest P2; the contextual scoring correctly elevates it to P1 for a Linux-heavy telco.
