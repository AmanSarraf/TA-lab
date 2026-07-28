# Talent Angels — Infrastructure Budget Estimate

**Audience:** Mentors, mentees, and future contributors reviewing Talent Angels infrastructure cost for coordinated work  
**LFX Mentorship 2026 — Sprint 3**   
**Status:** **Draft for mentor review** — unit-price estimate and provisional workload model; it does not approve procurement, provisioning, spending, or sponsor outreach.  
**Companion doc:** [Environment design](./Talent_Angels_Sprint3_Environment.md)

> **This document is an infrastructure cost estimate**, not a sponsorship package and not a procurement quote. Vendor prices and programme eligibility can change. Amounts are in USD because the cited providers publish these services in USD.

---

## Mentor decision summary

| Item | Value |
|---|---|
| **Provisional scenario for mentor evaluation** | **B — Shared mentorship environment** |
| **Monthly cost (incl. 20% contingency)** | **$261.54** |
| **4-month total (gross estimate)** | **$1,046.16** (~**$1,050**) |
| **Planning envelope (upper bound, not a spend target)** | **$1,500** |
| **Main cost drivers** | LLM inference, AuraDB Professional 1 GB ($65.70/mo), 4 GB-class shared compute (~$20/mo) |
| **Project baseline on contributor machines + public GitHub Actions** | **$0** |
| **Next gate for this estimate** | Mentor validation of assumptions and sizes |
| **Crowdfunding / sponsor outreach** | **Proposed to be deferred** until the estimate is validated; mentor confirmation requested |

### Gross estimate vs planning envelope

- **Gross estimate (~$1,050):** Scenario B monthly total × 4 months, including 20% contingency. This is the figure mentors should treat as the expected planning cost if Scenario B is approved.
- **Planning envelope ($1,500):** An upper bound for usage variance (extra LLM/evaluation load, temporary DB scale, small networking/storage, observability experiments). **It is not a spending target.** The team should not spend $1,500 by default.

> **Scope clarification requested:** The original Sprint 3 brief requested five initial funding submissions. The team subsequently proposed validating infrastructure requirements and budget estimates before sponsor outreach. Mentors are requested to confirm this revised sequence.

---

## 1. Purpose and estimate funnel

Sprint 3 needs a clear answer to: **what shared infrastructure does Talent Angels need, and what does it cost over Aug–Nov 2026?**

The estimate funnel is:

```text
Define environment
  → Estimate gross cost (this document)
  → Prefer OSS / free resources and cost controls
  → (After mentor validation) seek vendor/cloud credits
  → (Later) fund only the residual gap if still needed
```

Related environment design—8 local workstations, one shared staging server, local vs server map, and verification—is in the [Environment doc](./Talent_Angels_Sprint3_Environment.md).

**Proposed sequencing for mentor review:** LOIs, the LFX Crowdfunding form, and outreach to the five named sponsors would follow validation of the estimate. This revises the sequence in the original Sprint 3 brief and therefore requires mentor confirmation.

---

## 2. Scenario B assumptions (auditable)

Planning assumptions for the **provisional** scenario. Token volumes are **placeholders** until measured in development; LLM unit prices are a **reference only**, not a product lock-in.

| Assumption | Value | Notes |
|---|---|---|
| Planning horizon | Aug–Nov 2026 (4 months) | Mentorship shared-env window |
| Shared compute | 4 GB-class instance | Lightsail-style reference ~$20/mo; vendor TBD |
| Managed graph (shared) | Neo4j AuraDB Professional **1 GB** | **$65.70/mo** list price |
| LLM requests | **25,000 requests / month** | Shared staging + integration usage |
| Tokens per request | **5,000 input** + **1,500 output** | Planning average |
| Tokens / month | **125M in** + **37.5M out** | 25k × 5k / 1.5k |
| LLM price reference | Gemini 2.5 Flash rates: **$0.30/1M in**, **$2.50/1M out** | Transparent reference only; efficient-model **class** default |
| LLM monthly (ref) | **$131.25** | See §4 Scenario B |
| Embedding allowance | **$1.00/mo** | Small; embeddings unlikely to dominate |
| Contingency | **20%** on subtotal | Usage variance buffer |
| Contributor machines | Existing hardware | **$0** project cost |
| GitHub Actions (public repos, standard runners) | **$0** baseline | Public repos / standard hosted runners[^17] |
| Paid observability (Scenario B) | **$0** baseline | Free/OSS tracing first |

### Math check (do not change without re-deriving)

```text
Subtotal = $20.00 + $65.70 + $131.25 + $1.00 = $217.95
+ 20% contingency = $43.59
Monthly = $261.54
× 4 months = $1,046.16
```

---

## 3. Infrastructure bill of resources

| Capability | Purpose | Baseline strategy |
|---|---|---|
| Contributor machines | Local development (8 workstations) | Existing hardware: **$0** project cost |
| GitHub / CI | PRs, tests, integration checks | Public repositories: **$0** baseline |
| Shared compute | API, agents, staging | Small cloud instance (Scenario B: 4 GB-class) |
| Neo4j | Taxonomy / skills graph | Free locally; managed shared DB on staging |
| Vector retrieval | Semantic retrieval | Benchmark Neo4j-native first |
| LLM inference | Agent reasoning | Token-based API |
| Embeddings | Semantic indexing | Token-based; mainly initial / incremental |
| Taxonomy storage | O\*NET, ESCO, BLS, SFIA, JobTech, Lightcast (as available) | Included storage initially; Lightcast is proprietary (fixtures/partner until funded) |
| Observability | Agent tracing / evaluation | Free / OSS first |
| Secrets / networking | Secure deployment | Platform capabilities initially |

---

## 4. Current pricing references

### Shared compute

Amazon Lightsail is used as a **simple budgeting reference**, not a final provider decision. Current Linux/Unix **IPv6-only** bundles include 2 GB RAM at **$10/month**, 4 GB at **$20/month**, and 8 GB at **$40/month**.[^1] The comparable 4 GB Linux/Unix bundle with a public IPv4 address is **$24/month**; the estimate retains the $20 IPv6-only reference pending a networking and provider decision.

### Neo4j AuraDB

Neo4j lists AuraDB Free at **$0** and AuraDB Professional at **$65.70/month for 1 GB**, **$131.40/month for 2 GB**, and **$262.80/month for 4 GB**.[^2]

### LLM reference (price only)

Calculations use **Gemini 2.5 Flash rates only as a transparent reference**, not as a final model decision. Current paid pricing used in this estimate: **$0.30/1M input tokens** and **$2.50/1M output tokens**.[^3]

Talent Angels keeps model access **configuration-driven**. The temporary default is an **efficient mid-tier / Flash-class** model for routine development (aligned with this price reference). Premium models are reserved for measured quality needs.

### Embeddings reference

OpenAI `text-embedding-3-small` is currently **$0.02/1M input tokens**.[^4]

```text
10M embedding tokens / 1M × $0.02 = $0.20
```

Embedding generation is therefore unlikely to dominate the Sprint 3 budget.

### Observability

LangSmith Developer is **$0/seat/month** for one seat with up to 5,000 base traces/month. Plus is **$39/seat/month** with up to 10,000 base traces/month, then usage-based charges.[^5]

### Optional size-class comparison (estimate quality only)

Vendor choice remains open. Approximate **size-class** comparison for shared compute budgeting (list prices change; use only for order-of-magnitude):

| Size class | Lightsail-style ref (USD/mo) | Typical use in this estimate |
|---|---|---|
| ~2 GB RAM | ~$10 | Scenario A minimum shared host |
| ~4 GB RAM | ~$20 IPv6-only; ~$24 with public IPv4 | **Scenario B provisional** staging |
| ~8 GB RAM | ~$40 | Scenario C early public (future) |

Equivalent AWS EC2 / Google Compute Engine shapes in the same RAM band should be compared when a provider is chosen; this table does not lock vendor or SKU.

---

## 5. LLM cost model

These are **planning assumptions**. Actual cost depends on prompts, tool calls, retries, caching, model routing, and agent design.

### Scenario A — Minimal mentorship

Assumption: 10,000 requests/month; 4,000 input and 1,000 output tokens per request.

```text
Input  = 10,000 × 4,000 = 40M tokens
Output = 10,000 × 1,000 = 10M tokens

Input cost  = 40 × $0.30 = $12.00
Output cost = 10 × $2.50 = $25.00
LLM total   = $37.00/month
```

### Scenario B — Provisional shared development

Assumption: 25,000 requests/month; 5,000 input and 1,500 output tokens per request.

```text
Input  = 125M tokens
Output = 37.5M tokens

Input cost  = 125 × $0.30 = $37.50
Output cost = 37.5 × $2.50 = $93.75
LLM total   = $131.25/month
```

### Scenario C — Early public workload (future only)

Assumption: 100,000 requests/month; 5,000 input and 1,500 output tokens per request.

```text
Input  = 500M tokens
Output = 150M tokens

Input cost  = 500 × $0.30 = $150.00
Output cost = 150 × $2.50 = $375.00
LLM total   = $525.00/month
```

Scenario C is a **future planning reference**, not proposed Sprint 3 spend.

---

## 6. Budget scenarios

### A — Minimum viable mentorship

| Component | Monthly |
|---|---:|
| Local development | $0 |
| GitHub Actions | $0 |
| Shared 2 GB compute | $10.00 |
| Neo4j Free / self-managed | $0 |
| LLM workload | $37.00 |
| Embedding allowance | $1.00 |
| Paid observability | $0 |
| **Subtotal** | **$48.00** |
| 20% contingency | **$9.60** |
| **Monthly** | **$57.60** |
| **4 months** | **$230.40** |

### B — Recommended shared mentorship environment

| Component | Monthly |
|---|---:|
| Shared 4 GB compute | $20.00 |
| AuraDB Professional 1 GB | $65.70 |
| LLM workload | $131.25 |
| Embedding / indexing allowance | $1.00 |
| GitHub Actions | $0 |
| Paid observability baseline | $0 |
| **Subtotal** | **$217.95** |
| 20% contingency | **$43.59** |
| **Monthly** | **$261.54** |
| **4 months** | **$1,046.16** |

**Gross 4-month estimate if Scenario B is approved: ~$1,050** (before any credits or sponsorship).  
**Planning envelope (upper bound): $1,500** — not a spend target.

### C — Early public deployment (future only)

| Component | Monthly |
|---|---:|
| Shared 8 GB compute | $40.00 |
| AuraDB Professional 2 GB | $131.40 |
| LLM workload | $525.00 |
| Embeddings / index updates | $2.00 |
| GitHub Actions | $0 |
| Observability allowance (2 LangSmith Plus seats) | $78.00 |
| **Subtotal** | **$776.40** |
| 20% contingency | **$155.28** |
| **Monthly** | **$931.68** |
| **4-month equivalent** | **$3,726.72** |

Not proposed Sprint 3 spend.

---

## 7. Scenario comparison

| Scenario | Monthly | 4 months | Use |
|---|---:|---:|---|
| A — Minimal | $57.60 | $230.40 | Prototype / low-cost |
| **B — Recommended** | **$261.54** | **$1,046.16** | **Shared mentorship environment** |
| C — Early public | $931.68 | $3,726.72 | Future reference only |

The **$1,500 infrastructure envelope** covers variance only. Preferred path: stay near the ~$1,050 gross estimate, control usage, and reduce cash need via free tiers and (later) credits.

---

## 8. Four-month quantity rollup (Scenario B)

Auditable consumption quantities behind the cost story (for mentors and, later, any funding narrative).

| Quantity | Monthly | × 4 months | Notes |
|---|---:|---:|---|
| LLM input tokens | 125M | **500M** | Placeholder until measured |
| LLM output tokens | 37.5M | **150M** | Placeholder until measured |
| Server hours (4 GB-class, 24×7) | ~730 h | **~2,920 h** | Continuous shared staging assumption |
| AuraDB Professional 1 GB | $65.70 | **$262.80** | List price; may drop with credits later |
| Gross infrastructure (incl. contingency) | $261.54 | **$1,046.16** | Scenario B total |

### Short budget justification narrative

Over August–November 2026, Talent Angels expects contributors to develop on **eight local workstations** (four mentees, four mentors) at **$0** project hardware cost, with **public GitHub continuous integration (CI)** at a **$0** baseline for standard runners. Coordinated integration needs **one shared staging server** in the **4 GB class** (~2,920 server-hours if left 24×7) plus a **managed Neo4j AuraDB 1 GB** instance for a shared taxonomy/skills graph. Agent work drives the largest variable cost: under Scenario B planning assumptions, about **500M input + 150M output tokens** over four months at the efficient-model **price reference**, plus a small embedding allowance. A **20% contingency** brings the gross plan to **~$1,050**. The **$1,500** figure is only an upper planning envelope.

---

## 9. Cost-control guardrails

- Set monthly budgets and alerts.
- Separate development and staging credentials.
- Track token usage per workflow.
- Measure cost per successful agent task.
- Avoid permanently oversized compute or database instances.
- Remove temporary resources after experiments.
- Review infrastructure spend monthly.
- Benchmark Neo4j-native vectors before adding another vector database.
- Mock LLM responses in CI where possible.
- Reserve live-model evaluation for integration / evaluation jobs.

Microsoft’s startup guidance similarly recommends budget alerts, tagging, model-cost measurement, and cleanup of idle resources.[^6]

---

## 10. Post-validation path: credits and residual funding

**Proposed status for mentor confirmation:** funding outreach and crowdfunding are **deferred** until mentors validate this estimate and the environment design. The sections below sketch options for the post-validation stage.

### Architectural cost reduction (do first, anytime)

```text
Public GitHub repos → $0 standard Actions
Local Neo4j / Docker → $0 for normal development
Benchmark Neo4j vectors before adding pgvector
Free / OSS tracing before paid observability
Efficient LLM class for routine development
Premium models only where measured quality requires them
```

### Credit opportunities (after validation)

| Opportunity | Potential value | Fit | Later action |
|---|---|---|---|
| **LFX Crowdfunding** | Direct project / infrastructure funding | High (when ready) | After residual gap is known |
| **Neo4j** | Up to $16K Aura credits programme[^7] | High technical fit; eligibility TBD | Confirm OSS / LF eligibility |
| **LangChain / LangGraph** | LangSmith / community support | High ecosystem; formal startup tiers may not fit | Community / partnership outreach |
| AWS Activate | Startup credit tiers[^8] | Eligibility TBD | Check LF / provider route |
| Google Cloud | MVP / startup tiers[^9][^10] | Eligibility TBD | Check OSS / LF route |
| Microsoft for Startups | Credits for eligible startups[^11][^12] | Low under normal published rules | Seek OSS-specific route if any |
| GitHub Sponsors | Community sponsorship[^13] | Medium | Confirm project governance first |

**Neo4j note:** planned four-month AuraDB list cost is only `$65.70 × 4 = $262.80`. Even a small approved credit could remove that line.

**LangChain note:** published startup tiers are often VC/startup oriented.[^14] Talent Angels should seek **open-source / community** support rather than assume startup-program eligibility.

**LFX Crowdfunding:** supports open-source projects and infrastructure funding with transparent expenses and Linux Foundation fiscal hosting.[^15][^16] **Do not treat form submission as part of this estimate deliverable.**

### Residual gap formula (when credits are known)

```text
Gross recommended 4-month budget        $1,046.16
- confirmed Neo4j credits               $XXX.XX
- confirmed cloud credits               $XXX.XX
- confirmed AI / API credits            $XXX.XX
-------------------------------------------------
= evidence-based residual funding need  $XXX.XX
```

---

## 11. Recommended next actions

| Priority | Action |
|---|---|
| **P0** | Validate Scenario B resource sizes and workload assumptions with mentors |
| **P0** | Prefer free / OSS paths already in the environment design (local Neo4j, public CI, mock LLM in CI) |
| **P1** | Measure actual model token usage once integration work starts |
| **P1** | Benchmark Neo4j-native vectors before adding pgvector |
| **P1** | After estimate validation: check LF/LFDT vendor credit relationships (Neo4j, cloud, LangChain community) |
| **P2** | Replace planning assumptions with measured usage (e.g. later sprint) |
| **Deferred** | LFX Crowdfunding draft, multi-sponsor LOIs, formal credit applications as a campaign |

---

## 12. Mentor-ready summary

> Talent Angels translated the environment design into concrete infrastructure cost categories for **August–November 2026**. Under **Scenario B (shared mentorship environment)**—one **4 GB-class** shared staging host, **AuraDB Professional 1 GB**, and LLM usage at the efficient-model **price reference**—the current planning estimate is **$261.54/month** including 20% contingency, or **$1,046.16 (~$1,050) over four months**. Contributor machines and standard public GitHub Actions remain **$0**. A **$1,500** figure is only an **upper planning envelope**, not a spend target.
>
> **This Sprint 3 deliverable stops at a mentor-reviewable cost estimate.** Crowdfunding and sponsor outreach are **deferred** until assumptions are validated. After validation, the preferred sequence is free/OSS → vendor credits → fund only the residual gap.

---

## Conclusion

```text
Provisional environment for mentor evaluation (Scenario B):
$261.54/month × 4 months = $1,046.16

Rounded gross budget: ~$1,050
Suggested planning envelope (upper bound): $1,500
```

The team should **not spend $1,500 by default**. Treat the envelope as a ceiling for variance, control consumption, pursue free tiers and (later) credits, and fund only what remains.

---

## References

[^1]: AWS, “Amazon Lightsail Pricing”:  
    https://aws.amazon.com/lightsail/pricing/

[^2]: Neo4j, “Graph database pricing”:  
    https://neo4j.com/pricing/

[^3]: Google AI for Developers, “Gemini Developer API pricing”:  
    https://ai.google.dev/gemini-api/docs/pricing

[^4]: OpenAI Developers, “text-embedding-3-small”:  
    https://developers.openai.com/api/docs/models/text-embedding-3-small

[^5]: LangChain, “LangSmith Plans and Pricing”:  
    https://www.langchain.com/pricing

[^6]: Microsoft Learn, “Best Ways to Use Startup Credits”:  
    https://learn.microsoft.com/en-us/startups/benefits/azure-credits/use-azure-credits

[^7]: Neo4j, “Neo4j Startup Program”:  
    https://neo4j.com/startup-program/

[^8]: AWS, “AWS Activate Credits”:  
    https://aws.amazon.com/startups/credits/

[^9]: Google Cloud, “Google for Startups Cloud Program”:  
    https://cloud.google.com/startup

[^10]: Google Cloud, “Startup programme eligibility and benefits”:  
    https://cloud.google.com/startup/benefits

[^11]: Microsoft Learn, “Getting Started with Microsoft for Startups”:  
    https://learn.microsoft.com/en-us/startups/microsoft-for-startups/getting-started-mfs

[^12]: Microsoft Learn, “What is Microsoft for Startups?”:  
    https://learn.microsoft.com/en-us/startups/microsoft-for-startups/overview

[^13]: GitHub Docs, “About GitHub Sponsors”:  
    https://docs.github.com/en/sponsors/getting-started-with-github-sponsors/about-github-sponsors

[^14]: LangChain, “LangSmith for Startups”:  
    https://www.langchain.com/startups

[^15]: Linux Foundation, “LFX CrowdFunding — For Projects”:  
    https://crowdfunding.linuxfoundation.org/for-projects

[^16]: Linux Foundation, “About LFX CrowdFunding”:  
    https://crowdfunding.linuxfoundation.org/about

[^17]: GitHub Docs, “GitHub Actions billing”:  
    https://docs.github.com/en/billing/concepts/product-billing/github-actions
