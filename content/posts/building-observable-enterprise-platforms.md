+++
date = '2026-03-15T16:56:19+05:30'
draft = false
title = 'Building An Observable Enterprise Platform'
+++

## The Reality of the Modern Enterprise Platform

No enterprise runs on a single, clean, uniform system. The reality is far messier — and far more interesting.

A typical enterprise platform today is a patchwork of applications built across different eras, teams, and technology choices. Some are monolithic, battle-hardened systems that have been running for a decade. Others are modern microservices, heavily distributed across dozens of containers. Some run on-premises, some on the cloud, some are SaaS products, and some are vendor solutions deployed on your own infrastructure. They're written in different languages, follow different conventions, and were built by teams who never talked to each other.

And yet — they all belong to the same platform. They all serve the same business.

This is what *true heterogeneity* looks like in the enterprise. And it is the starting point for any honest conversation about observability.

This is not a problem unique to any one industry. A telecom operator managing a billing and mediation platform deals with the same complexity — millions of raw usage records flowing in from network elements, rated, aggregated, and invoiced across a chain of systems with strict regulatory SLAs. A hospital running a patient journey platform coordinates booking, clinical workflows, pharmacy, billing, and insurance claims across systems that were never designed to talk to each other. A retail giant processing Black Friday orders is orchestrating inventory, fraud, payments, warehousing, and last-mile delivery — simultaneously, at massive scale. The technology stacks differ. The business domains differ. The underlying architectural challenge is identical.

---

## A Concrete Example: The Corporate Payment Processing Platform

To make this tangible, let's walk through a real-world scenario: a **Corporate Payment Processing Platform**.

A corporate client sends a file containing hundreds of thousands of credit transfer transactions — salaries, vendor payments, bulk disbursements. From the moment that file arrives, a complex chain of processing begins:

- **Client Acquisition** — the file is received and ingested
- **Validation** — format, completeness, and business rules are checked
- **Fraud, Sanction & Embargo Checks** — each transaction is screened
- **Limit & Eligibility Checks** — per-client and per-transaction rules are applied
- **Payment Execution** — the actual transfer is initiated
- **Account Deduction & Pricing** — the client account is debited, fees are calculated
- **Reporting & Downstream Feeds** — data flows to reporting systems, data lakes, and downstream consumers

Each of these steps may be owned by a different application, team, or even a third-party vendor system. Some interactions are synchronous — REST APIs waiting for a response. Others are asynchronous — messages flowing through a broker, processed in the background.

And here's where it gets complex: processing happens at multiple levels simultaneously. A *file* is processed as a whole. Each *transaction* within it is processed individually. Results are then aggregated back at the file level. And then split again at the transaction level for downstream systems.

---

## The SLA Question Nobody Can Answer

Now ask yourself this: **what is your end-to-end SLA for that file?**

Most enterprises have a number. "Files must be fully processed within 15 minutes of receipt." What they don't have is the ability to answer the questions that follow:

- Are we meeting that SLA today, right now, for the file currently in flight?
- If not — *which system* is the bottleneck?
- Was that delay expected or anomalous? Was it justified by load, a downstream dependency, or a genuine problem?
- How do you allocate the SLA budget across each system in the chain? If the end-to-end SLA is 15 minutes, how much time does each service get?
- When your performance testing team runs a load test, where exactly do they measure? Which system's numbers matter?

These are not theoretical questions. They are the questions that get asked in post-incident reviews, in capacity planning meetings, and by clients who missed their payroll run.

Without observability, you cannot answer any of them with confidence.

---

## Observability Is Not Monitoring

Before going further, it's worth grounding ourselves. Observability has become an overloaded term — often conflated with dashboards, alerts, or log aggregation. These are tools. Observability is a property of your system.

A system is observable if you can understand its internal state from its external outputs — without needing to deploy new code or add new instrumentation every time something goes wrong.

The three pillars of observability are well-established:

- **Logs** — the narrative of what happened, event by event
- **Metrics** — the quantitative pulse of your system over time
- **Traces** — the journey of a request as it travels across services

For a payment platform like the one described above, all three matter. But **traces and metrics** are where the real power lies for end-to-end SLA tracking and distributed system visibility.

---

## The Tracking Problem: One File, A Hundred Thousand Transactions

Here is one of the hardest problems in enterprise observability: **how do you track a single file and every transaction within it, end to end, across a dozen systems?**

A file with 100,000 transactions, each touching 8–10 services, produces an almost incomprehensible volume of events. Naively tracing every transaction through every service would flood your observability backend and make the data practically unusable.

The answer is not to abandon tracing — it's to be **strategic about what you trace and how you link it**.

## Correlating Across a Multi-App Payment Platform

In an enterprise payment platform, a single file ingestion can trigger processing across dozens of microservices spanning multiple applications. A file containing many remittances, each with multiple transactions, results in thousands of distributed traces — each isolated by default. The challenge is not just tracing within a service, but answering business questions like: *which services processed this transaction?* or *what happened to this file across the entire platform?*

The naive solution — a single trace spanning everything — does not scale. Distributed async systems break parent-child trace relationships at every queue boundary. Instead, the right pattern is **structured tag propagation**: injecting business correlation identifiers (`file.id`, `remittance.id`, `transaction.id`) into every message's envelope headers at the point of origin, extracting and re-injecting them at every consumer, and stamping every span with these identifiers as indexed tags.

In Elastic APM, these tags become searchable labels. A search for `transaction.id = "TXN123"` surfaces every trace across every application that touched that transaction — regardless of whether those traces are linked by SpanLinks or not. This gives operations teams three independent lenses into the platform: file-level, remittance-level, and transaction-level — without requiring a single monolithic trace.

The key discipline is **never dropping context**. Even when a downstream service only processes at transaction level and has no concept of the originating file, the file ID must still flow through message headers and be stamped on spans. Observability is only as complete as the weakest link in the propagation chain.

### The Right Instrumentation Strategy

The recommended approach for a platform of this scale:

- Instrument at the **service level** using spans, capturing entry, exit, and key decision points within each service
- Use **trace context propagation** to link spans across service boundaries — both synchronous (HTTP headers) and asynchronous (message envelope headers in your chosen messaging infrastructure)
- Use **tags and attributes** on spans — file ID, transaction ID, client ID, processing stage — so you can search and correlate across the entire chain without needing a single monolithic trace
- For truly high-volume transaction processing, consider **sampling strategies** that preserve full fidelity for error paths and SLA breaches, while sampling routine successful flows

This gives you the ability to answer: *where is my file right now, which service is holding it, and is it on track?*

### Why Auto-Instrumentation Is Not Enough — and Can Actually Hurt

It is tempting to enable auto-instrumentation across all services and let the framework do the work. For individual application development and debugging, this is perfectly reasonable. But at **platform observability level**, auto-instrumentation becomes a liability.

Auto-instrumentation captures everything — every HTTP call, every database query, every middleware hop. Across dozens of microservices processing hundreds of thousands of messages, this floods your APM backend with noise that drowns out the signals that actually matter. Worse, it creates a false sense of coverage: you see a lot of spans, but none of them carry the business context (file ID, transaction ID, processing stage) that makes traces meaningful at the platform level.

**Manual instrumentation is the only reliable path to meaningful platform observability.** It requires discipline — every team must instrument their service at the right points, propagate correlation headers, and stamp spans with business identifiers. But the result is a clean, queryable signal rather than an overwhelming stream of technical noise.

### Separating Platform Observability from Application Observability

These are two distinct concerns and should be treated as such:

- **Application observability** — owned by individual teams, may use auto-instrumentation, focused on debugging and performance within a single service or application. Teams are free to use whatever tooling works for them.
- **Platform observability** — owned centrally, manually instrumented, focused on business flows that span multiple applications. This feeds the main APM environment and must be kept clean and purposeful.

Mixing these two realms in the same APM environment is a common mistake. The recommended pattern is to use the **OpenTelemetry Collector pipeline** to separate them:

```
Service auto-instrumentation
        │
        ▼
OTel Collector (per app / team)
        │
        ├──► Team APM environment  (full fidelity, auto-instrumented, for dev/debug)
        │
        └──► Filter / transform
                    │
                    ▼
            Platform OTel Collector
                    │
                    ▼
            Platform APM  (manual spans only, business context tags)
```

The platform collector applies **filtering rules** — only forwarding spans that carry the required business correlation tags (`file.id`, `transaction.id` etc.) and dropping auto-instrumented noise. This can be configured using the OTel Collector's `filterprocessor` and `attributesprocessor`:

- Drop spans missing `file.id` or `transaction.id` tags
- Drop spans from known noisy auto-instrumented sources
- Enrich spans with environment metadata before forwarding

This gives each team the freedom to instrument their own services however they like, while keeping the platform observability layer clean, purposeful, and queryable.


## Observability Cannot Be an Afterthought

This is perhaps the most important strategic point in this entire piece, and it cannot be stated strongly enough:

**Observability must be designed in from day one. It cannot be bolted on later.**

The cost of retrofitting observability into a running enterprise system is enormous — not just technically, but organizationally. You are asking teams to go back into production code, agree on instrumentation standards, propagate trace context through message formats that were never designed for it, and retrofit tagging conventions across systems built years apart.

Some hard lessons from the field:

- **Decide your observability toolchain before you write your first service.** The choice of APM platform, telemetry standard (OpenTelemetry is the clear answer today), and data backend affects how you instrument from line one.
- **Establish tagging conventions as architecture decisions.** What attributes will every span carry? File ID? Correlation ID? Business transaction type? These need to be standards, not suggestions.
- **Manual instrumentation is non-negotiable.** Auto-instrumentation will give you infrastructure-level signals — HTTP calls, database queries, message publish/consume events. It will not give you business context. It will not tag your spans with the file ID or tell you which sanction check failed. That requires deliberate, manual instrumentation by developers who understand the business domain.
- **Auto-instrumentation alone will flood you.** Without a strategy, auto-instrumentation produces enormous volumes of low-value telemetry. It is a starting point, not a solution.

---

## The Pattern Repeats — Across Every Industry

The payment processing platform is one instance of a universal pattern. Once you see it, you find it everywhere.

Trade settlement in capital markets crosses multiple systems and legal entities with T+2 as a legal obligation, not a target. Logistics platforms face real demurrage costs every hour a shipment is stuck in a system nobody can see into. Retail order fulfilment stitches together in-house platforms, SaaS tools, and third-party logistics — and finds out observability was never properly built precisely when Black Friday traffic hits.

The details change. The stakes change. The core problem does not — a business process spanning heterogeneous systems, with an end-to-end SLA that nobody can see, measure, or defend.

This is why observability is not a platform engineering concern. It is a business architecture concern — and it deserves to be treated as one from the very first design conversation.
