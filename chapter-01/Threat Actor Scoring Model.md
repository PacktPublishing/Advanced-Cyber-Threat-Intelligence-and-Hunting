# Threat Actor Scoring Model
---
# Introduction
A scoring framework for assessing how much of a threat a specific threat actor poses to our organization. Used to produce a persistent *Threat Score* that lives in the actor profile and informs response tiering when an actor profile or update triggers the CTI SOP.

This model complements the Severity Tier Matrix, it does not replace it. The Severity Tier Matrix continues to score vulnerabilities, campaigns, malware reports, peer incidents, supply chain compromises, and geopolitical events. The Threat Actor Scoring Model scores threat actors specifically.

Threat actors differ from other CTI triggers in ways that matter for scoring:
* **Persistence**: Actors are not "remediated”. A CVE gets patched, a campaign ends, but an actor continues to exist and evolve. Their threat score should persist alongside the actor profile, not be reset on each engagement.
* **Asymmetric dimensions**: With actors, *intent* matters disproportionately. A highly capable actor with no interest in our sector is not a threat. The classic example: the NSA's Tailored Access Operations unit is among the most capable cyber actors in existence, yet poses essentially zero threat to a European organization because there is no intent to target it. A scoring model that averages capability and intent equally would miss this, so intent must act as a gating factor.
* **Composite opportunity**: Opportunity for an actor is a richer concept than for a vulnerability. It includes whether they operate against our industry and geography (sectoral reach) *and* whether we are specifically reachable given our environment (organizational exposure).
## Intent, Capability, Opportunity
The model is built on the classical CTI threat triangle:
* **Intent:** Does the actor want to target us?
* **Capability:** Can they execute against us if they choose to?
* **Opportunity:** Do they have a path to us, both sectorally and organizationally?

A threat exists only when all three are present. The model scores each dimension on a 0.0–1.0 scale, then combines them with weighted averaging plus an Intent floor rule.
## How this model is used
1. **At actor profile creation**: When a new actor profile is built or a significant update lands, the analyst scores each dimension and produces the *Threat Score*. The score is stored in the actor profile.
2. **At quarterly review**: Every actor profile is reviewed quarterly. Each dimension is reassessed and the score updated if any have materially shifted.
3. **On material event**: Significant changes (new attribution, capability evolution, geopolitical shift, infrastructure change in our environment) trigger an out-of-cycle review of affected actors.
4. **On trigger response**: When an actor profile or update is the trigger for the SOP, the *Threat Score* feeds the provisional severity in Phase 1.4 and the contextual severity in Phase 2.6.
---
# Dimension 1: Intent
The actor's interest in targeting our organization, our sector, or organizations like us. Intent is inferred from observable signals, as actors do not (usually) publish target lists, so analysts work from behavior, victimology and motivation.
### What to consider
* **Stated or implied targeting**: Doctrine, leaks, indictments, defector reporting, public statements (rare but high-value when available).
* **Historical victimology**: Who has the actor compromised, in what sectors, in what geographies? 
* **Recent target selection patterns**: Is the actor's targeting drifting toward or away from us? Recent activity weighs more than older patterns.
* **Geopolitical motivation**: State-aligned actors generally target adversaries of their sponsoring state. Our headquarters geography, customer base, and government relationships all factor in.
* **Declared adversarial posture**: If the actor's sponsoring state has declared (formally or informally) adversarial posture toward our country, that raises intent.

### Scoring scale (0.0–1.0)
* **0.0 — None**: No observable signal of interest in our sector, geography, or organization type. Example: a regional ransomware affiliate focused exclusively on US healthcare.
* **0.25 — Low**: Weak indirect signals; sector or geography is not in observed targeting, but adjacent.
* **0.5 — Moderate**: Actor has targeted our sector or geography at some point, but inconsistently or not recently.
* **0.75 — High**: Actor has demonstrated consistent interest in our sector and geography; recent activity confirms ongoing targeting.
* **1.0 — Critical**: Actor has directly targeted our organization, or has stated targeting of organizations directly comparable to us, with recent confirmed activity.

**The Intent floor rule**
If Intent is scored at 0.0, the overall Threat Score is forced to 0.0 regardless of Capability and Opportunity. This reflects the foundational principle: no intent, no threat. The TAO example sits here for most organizations. If Intent is scored above 0.0 but remains low (0.25), the weighted average still suppresses the final score, but does not zero it. This handles the case of capable actors with weak but non-zero interest in us.
# Dimension 2: Capability
The actor's ability to execute an operation against us if they chose to. Capability is the most measurable of the three dimensions, as it leaves the most public evidence in the form of tooling, tradecraft and observed operations.
### What to consider
* **Tooling sophistication**: Custom-developed implants and exploits vs. commodity tooling. Custom kernel-level implants (e.g. BPFdoor) signal high capability; off-the-shelf RATs signal lower.
* **Tradecraft maturity**: OPSEC discipline, infrastructure rotation, anti-forensics, living-off-the-land usage, multi-stage operations.
* **Zero-day access**: Demonstrated use of unpatched vulnerabilities, particularly in widely deployed software. State-aligned actors often have this; criminal groups generally do not.
* **Operational tempo**: How frequently does the actor operate? Sustained, parallel operations indicate higher resourcing.
* **Demonstrated reach**: How many sectors and geographies has the actor successfully operated against? Breadth signals capability; depth signals focus.
* **Resourcing model**: State-backed > organized criminal > criminal affiliate > hacktivist > hobbyist, broadly. This is a baseline; specific actors can defy the pattern.
* **Specialization for our target type**: An actor with a track record against our sector scores higher for us than one with equal general capability but no sector-specific tooling.

### Scoring scale (0.0–1.0)
* **0.0 — Negligible**: No demonstrated offensive capability. Almost never used for tracked actors; if scored here, the actor probably should not be a tracked actor.
* **0.25 — Low**: Commodity tooling, low tradecraft maturity, opportunistic operations. Most low-end criminal actors.
* **0.5 — Moderate**: Capable but not exceptional. Mix of custom and commodity tooling, decent tradecraft, established operational patterns. Many criminal groups and lower-tier APTs.
* **0.75 — High**: Sophisticated tooling, mature tradecraft, sustained operational tempo, demonstrated success against hardened targets. Most named APT groups.
* **1.0 — Top-tier**: State-aligned, mature, with zero-day access, custom kernel-level or hypervisor-level capabilities, multi-year sustained operations against high-value targets. Examples: Equation Group / TAO, Turla, top-tier MSS-aligned groups.
# Dimension 3: Opportunity
The actor's path to us, considered along two sub-dimensions. Opportunity is the dimension most likely to shift between reviews, as sectoral reach moves on actor activity changes, organizational exposure moves on our environment changes.
### Sub-dimension 3a: Sectoral Reach
Does the actor operate against organizations like us? This is the abstract, structural question, independent of our specific environment.

**What to consider**
* **Sectoral targeting history**: Has the actor compromised sector-specific targets, or organizations directly adjacent?
* **Demonstrated access to our type of infrastructure**: Has the actor shown they can operate against the technology stacks we use?
* **Supply chain reach**: Does the actor have demonstrated access to vendors or providers we depend on?

**Scoring scale (0.0–1.0)**
* **0.0 — None**: No demonstrated reach to organizations like us in any meaningful sense.
* **0.25 — Low**: Weak indirect reach; operates in adjacent sectors or geographies but not directly against organizations like ours.
* **0.5 — Moderate**: Has operated against organizations adjacent to our sector or in our geography, but not directly against organizations comparable to us.
* **0.75 — High**: Has operated against our competitors or directly comparable organizations in our geography or with similar technology stacks.
* **1.0 — Full**: Has operated against directly comparable organizations recently, with demonstrated access to the same types of systems we run.

### Sub-dimension 3b: Organizational Exposure
Are we specifically reachable given our current environment? This is the concrete, environment-dependent question.

**What to consider**
* **Technical exposure**: Do we run the technologies the actor is known to target or has tooling for? Linux fleets for BPFdoor-style actors, Windows AD for many APT groups, specific telco platforms for telco-focused actors.
* **Relational exposure**: Peering, roaming, supply chain, and partnership relationships with confirmed victims of the actor or with organizations the actor is known to operate against.
* **Control coverage gaps**: Are there material gaps in our detection or prevention coverage against the actor's known TTPs?
* **Detection blind spots**: Do we have visibility into the kinds of activity this actor produces? An actor specializing in eBPF-level persistence is more dangerous to an org with no kernel-level visibility than to one with strong eBPF monitoring.

**Scoring scale (0.0–1.0)**
* **0.0 — None**: No technical or relational exposure to the actor's known TTPs; comprehensive detection coverage against their tradecraft.
* **0.25 — Low**: Limited technical exposure, strong compensating controls, good detection coverage.
* **0.5 — Moderate**: Meaningful technical exposure with partial control coverage and partial detection visibility.
* **0.75 — High**: Broad technical exposure, material control gaps, limited detection coverage against the actor's known TTPs.
* **1.0 — Severe**: Comprehensive technical exposure aligned to the actor's tooling, significant control gaps, blind spots against their known TTPs.

### Combining sub-dimensions
`Opportunity = (0.6 × Sectoral Reach) + (0.4 × Organizational Exposure)`
Sectoral Reach is weighted slightly higher because it is the more durable signal, as it changes when the actor's behavior shifts, which is the property we are trying to track. Organizational Exposure remains important but is more volatile and partially controllable through our own actions.
---
# Combining the Dimensions
### Weighting
* **Intent**: 50%
* **Capability**: 25%
* **Opportunity**: 25%

Formula:
`Threat Score = (0.5 × Intent) + (0.25 × Capability) + (0.25 × Opportunity)`
where:
`Opportunity = (0.6 × Sectoral Reach) + (0.4 × Organizational Exposure)`
### The Intent floor rule
If Intent = 0.0, then Threat Score = 0.0 regardless of any other dimension.
This rule overrides the weighted average. It exists because the weighted average alone would not adequately suppress the score for a top-capability, high-opportunity actor with zero intent — the average would still produce a non-trivial number, which is wrong. The floor rule enforces the principle that intent is the gating dimension.
### Score expression
The *Threat Score* is expressed as a number between 0.0 and 1.0 with two decimal places (e.g. Threat Score = 0.73). For presentation to non-CTI stakeholders, it can be expressed as a percentage (73%) or mapped to a band (see below).
# Mapping the Threat Score to Tier
When an actor profile or update triggers the SOP, the *Threat Score* is used in place of the Severity Tier Matrix's Axis 1. The exposure assessment in Phase 2 of the SOP still happens and produces Axis 2; the two combine in the existing tier grid.
### Threat Score to Threat Severity (Axis 1) and Operating Posture
The table below maps the *Threat Score* to both the Axis 1 severity used in the Severity Tier Matrix and the standing operating posture the CTI team maintains against the actor. The posture describes continuous work, distinct from the response triggered by a specific actor profile or update, which follows the SOP.

**Critical (0.85 – 1.00)** - Top-tier threat to our organization
**Operating posture**:
* Dedicated actor profile maintained with monthly dimension review.
* Active hunting against the actor's TTPs as a standing program, not an ad-hoc activity.
* Detection coverage validated and periodically tested against the actor's known tooling and tradecraft.
* Ongoing ISAC and peer engagement focused on the actor; trust-group chatter actively monitored.
* Any new public or shared reporting on the actor triggers immediate SOP intake.
* Briefed to CISO at least quarterly as part of top-threat reporting.

**High** **(0.60 – 0.84)** - Significant, sustained threat to our organization
**Operating posture**:
* Quarterly profile review with explicit reassessment of each dimension.
* Periodic hunts against the actor's TTPs.
* Detection coverage reviewed against the actor's known tooling as part of standard detection engineering planning.
* New reporting on the actor triggers SOP intake at normal priority.
* Included in annual top-threats briefing to security leadership.

**Medium** **(0.35 – 0.59)** - Real but constrained threat to our organization
**Operating posture**:
* Quarterly profile review.
* Detection coverage against the actor's TTPs is part of normal detection engineering work, not a dedicated workstream.
* New reporting on the actor triggers SOP intake at standard priority.
* No dedicated hunting programs against this actor; opportunistic only.

**Low** **(0.10 – 0.34)** - Weak threat, tracked but not pressing
**Operating posture**:
* Profile maintained but reviewed only on out-of-cycle triggers, not on a fixed cadence.
* New reporting reviewed at intake but rarely escalates beyond tracking note.
* No dedicated detection or hunting work; coverage relies on general defensive controls.

**Negligible** **(0.00 – 0.09)** - Not a meaningful threat at this time
**Operating posture**:
* Catalog-only. Actor remains in the actor catalog with explicit documented rationale for the Negligible score.
* The rationale is the artifact, as future analysts must be able to understand why an actor is rated Negligible. This is critical for Intent-floored actors, where the score appears counterintuitive on its face.
* No active work. Re-scored only if a significant change occurs (new reporting, geopolitical shift, capability or intent evolution).
# Maintenance and Review
Every tracked actor is reviewed at least quarterly. Actors scored **Critical** are reviewed **monthly** as part of the elevated operating posture for that band. The review considers each dimension:
* **Intent**: Have recent activity, victimology, or geopolitical signals shifted targeting toward or away from us?
* **Capability**: Has the actor evolved tooling, demonstrated new capability, or experienced known disruption (arrests, sanctions, infrastructure takedowns)?
* **Opportunity-Sectoral**: Has the actor expanded or contracted operating range against our sector or geography?
* **Opportunity-Organizational**: Has our environment changed in ways that affect exposure to this actor (new deployments, new controls, detection improvements, infrastructure shifts, M&A)?

The dimension scores are updated as warranted and the Threat Score is recomputed. Material changes are logged with rationale.
### Out-of-cycle review triggers
* New attribution research or significant intelligence update on the actor
* Actor named in a peer incident relevant to our sector
* Geopolitical shift affecting actor's sponsoring state and our country
* Major environmental change on our side (M&A, new platform deployment, divestiture)

### Score stability
The score should be stable enough to be useful but responsive enough to be accurate. Expect most actors to shift by no more than 0.10 in a typical quarter; larger shifts warrant explicit documentation of what changed.
---
# Worked Example 1: TAO
A canonical case of high capability and high opportunity with zero intent against an hypothetical European telco organization.
### Dimension scoring
* **Intent**: 0.0 No observable signal of TAO targeting European telcos or comparable organizations. TAO operates primarily against adversaries of the US government; a European telco of an allied country does not fit the targeting profile.
* **Capability**: 1.0 Top-tier. Documented zero-day access, custom firmware implants, supply chain operations, hardware interdiction capability. Multi-year sustained operations against hardened targets globally. By any objective measure, among the most capable cyber actors in existence.
* **Opportunity-Sectoral**: 0.5 Moderate. TAO has demonstrated capability against telco infrastructure globally (Snowden documents reference telco operations) but generally not against European allied operators. Reach exists, but not against comparable organizations.
* **Opportunity-Organizational**: 0.75 High. Our environment runs technologies TAO has demonstrated capability against (Linux, Cisco, common enterprise stacks); detection coverage against nation-state firmware-level operations is limited industry-wide.
* **Opportunity (composite)**: (0.6 × 0.5) + (0.4 × 0.75) = 0.60

**Threat Score calculation**
Intent floor rule applies, as Intent = 0.0, so **TS = 0.00**.

**Tier mapping**
* Threat Score 0.00 → Axis 1: **Negligible**
* Operating posture: Catalog-only with explicit documented rationale. TAO remains in the actor catalog precisely because the rationale — high capability, high opportunity, zero intent — must be visible to future analysts so the Negligible rating is defensible rather than appearing as an oversight.

**Interpretation**
This is the model working correctly. A naive "highest capability = highest threat" model would rank TAO as the top threat to our organization, which would be analytically wrong and would waste team resources on detection and hunting against an actor that has no interest in us. The Intent floor rule captures the operational reality that capability without intent is not threat.

# Worked Example 2: Red Menshen
The contrast case to TAO, as this example is high on all dimensions including intent. Demonstrates how the model handles a genuine top-tier threat.
### Dimension scoring
* **Intent**: 0.85 High. Red Menshen has demonstrated sustained, deliberate targeting of telecommunications providers globally, with confirmed compromises at European telcos. Telco sector is squarely in the actor's targeting set, and our geography is now in scope based on recent reporting.
* **Capability**: 0.85 High. Custom kernel-level implant (BPFdoor), mature tradecraft (TinyShell first-stage, eBPF persistence, anti-forensic operation), operational tempo aligned with PRC business hours suggesting sustained team resourcing, multi-year campaign with infrastructure rotation. Not at the absolute top tier (no confirmed zero-day access), but very close.
* **Opportunity-Sectoral**: 1.0 Full. Confirmed compromises at directly comparable European telcos in the past 18 months; same technology stacks (Linux, mobile core on Kubernetes, OSS/BSS).
* **Opportunity-Organizational**: 0.75 High. Linux estate matches BPFdoor target profile; detection coverage for eBPF-level persistence is limited; peering and roaming relationships with confirmed-victim telcos create indirect exposure paths.
* **Opportunity (composite)**: (0.6 × 1.0) + (0.4 × 0.75) = 0.90

**Threat Score calculation**
TS = (0.5 × 0.85) + (0.25 × 0.85) + (0.25 × 0.90) = 0.425 + 0.2125 + 0.225 = **0.86**

**Tier mapping**
* Threat Score 0.86 → Axis 1: **Critical**
* Operating posture: Dedicated profile with monthly review, active hunting program against BPFdoor and TinyShell TTPs, validated detection coverage, ongoing ISAC and peer engagement, quarterly CISO briefing as a top threat.

**Interpretation**
The model correctly identifies Red Menshen as a top-tier threat to our hypothetical telco, with intent driving most of the score and capability and opportunity confirming it. The same actor against a US healthcare provider would likely score much lower. This is the model behaving as intended: an actor that is a top-tier threat to one organization is not necessarily a top-tier threat to another.