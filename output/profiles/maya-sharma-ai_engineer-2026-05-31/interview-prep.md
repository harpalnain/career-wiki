# Interview Prep — Staff Software Engineer (Backend / Distributed Systems)

## 1. Context
- **Candidate:** Maya Sharma ([[profile]])
- **Target role:** Staff Software Engineer, backend / distributed systems (generic — no specific JD).
- **Positioning:** lead with system scope/depth (microservices migration, Kafka order pipeline) + hard performance metrics + public visibility (PyCon, OSS). Consistent with the generic Staff SWE CV (`output/staff-engineer-generic/cv.md`).
- **Template chosen:** Minimal-Technical (ATS-friendly, monospace tooling accents).
- **Date:** 2026-05-31

> **Staff-level reality check.** The wiki gives strong *individual technical* evidence but thin *staff-scope* evidence. A staff interviewer will probe: people leadership (only "mentored 4 juniors" + code-review standards — no team-lead title or reporting line), the true scope of "led migration" (sole lead vs. one of several), missing team sizes, and the absence of RFC / tech-strategy / org-design artifacts. Sections 4–6 below are built to help you close these before the loop. Do not invent facts to fill them — frame honestly.

## 2. Revise (skills the wiki shows you already have)

### [[go]] — flagship language
- [ ] Concurrency model: goroutines, channels, `context` cancellation/timeouts, worker pools.
- [ ] Memory/perf: escape analysis, GC tuning, profiling with `pprof`.
- [ ] Service design: graceful shutdown, connection pooling, backpressure.
- [ ] Be ready to whiteboard a piece of the Flowcart microservices in Go.

### [[kafka]] — distributed-systems core
- [ ] Delivery semantics: at-most / at-least / exactly-once; idempotent producers; transactions.
- [ ] Consumer groups, partition assignment, rebalancing, offset management.
- [ ] Ordering guarantees, partitioning keys, compaction vs. retention.
- [ ] Tie directly to the [[order-pipeline-kafka]] story and the PyCon talk thesis.

### [[python]] — data-engineering strength
- [ ] Async vs. multiprocessing vs. threading trade-offs; GIL implications.
- [ ] Packaging/maintainership lessons from the [[airflow-operator-library]].

### [[airflow]] — orchestration
- [ ] DAG design, idempotency/retries, backfills, scheduler internals.
- [ ] Partitioning + parallelism strategy from [[datanimbus-etl-framework]] (the 6h→90min win).

### [[aws]] — cloud (note: SAA expired)
- [ ] Refresh EC2/S3/Lambda/Redshift; be ready to explain why SAA-C03 lapsed (Sep 2025) and renewal intent.
- [ ] Redshift tuning (distkeys/sortkeys, vacuum/analyze) — ties to the ETL project.

### Distributed-systems fundamentals (cross-cutting)
- [ ] CAP, consistency models, idempotency, outbox/saga patterns, dedup strategies.
- [ ] Latency budgets, p99 vs. mean, the migration story (800ms→120ms): *what* actually moved the number.

## 3. Learn (commonly expected at staff level; not yet evidenced in the wiki)
> Framed as commonly expected for this role — these are growth areas, not current claims.

- **System design at scale (breadth):** end-to-end design of multi-service systems, capacity estimation, failure-domain reasoning. _Resource type: book (e.g., a designing-data-intensive-applications-class text), system-design course, engineering blogs._
- **Kubernetes / Terraform depth:** wiki lists both at medium confidence with **no backing project**. Either build a small demonstrable project or be candid that these are exposure-level. _Resource type: official docs + one hands-on lab._
- **Tech strategy / RFC writing:** no RFC, design-doc, or architecture-decision-record evidence in the wiki. Staff candidates are expected to show written technical judgment. _Resource type: read public RFC/ADR examples; draft one for a past project (see §4)._
- **Org-level influence / cross-team work:** no evidence of driving decisions beyond your immediate team. _Resource type: prepare narratives; no shortcut — see gap-closing in §6._

## 4. STAR stories to polish
> The wiki has **no `stories/` entries yet** — these are built from project pages. Draft each in full STAR form; the metrics exist but scope/role detail needs tightening.

1. **Exactly-once order pipeline** (from [[order-pipeline-kafka]] + [[pycon-india-2022-talk]]).
   - Result is strong (~500k orders/day, exactly-once). **Tighten:** your specific design decisions vs. teammates', failure-handling design, peak vs. average throughput.
2. **Monolith → microservices migration** (from [[monolith-to-microservices-migration]]).
   - Result strong (p99 800ms→120ms). **Tighten:** *scope of "led"* — sole lead or one of several? number of services, team size, migration duration. This is the single most important ambiguity to resolve before a staff loop.
3. **ETL 6h→90min** (from [[datanimbus-etl-framework]]).
   - Result quantified. **Tighten:** what the bottleneck was, what partitioning/parallelism changes specifically drove it.
4. **Code-review standards → ~30% fewer incidents** (from [[flowcart-senior-backend-engineer]]).
   - Your best *leadership* signal. **Tighten:** how you got buy-in, how the 30% was measured, how many engineers adopted it. This story does double duty for the people-leadership gap.

## 5. Likely questions

### Distributed systems / Kafka
1. Walk me through achieving exactly-once in your order pipeline. Where does it actually break down?
2. How do you handle consumer rebalancing without dropping or double-processing orders?
3. Outbox vs. dual-write — how did you avoid losing events on failure?
4. How would you redesign the pipeline for 10x throughput?

### Go / service design
1. How do you propagate cancellation and deadlines across services?
2. How did you decompose the monolith — what defined a service boundary?
3. What specifically drove p99 from 800ms to 120ms?

### Data engineering / Airflow
1. How did you cut the nightly batch from 6h to 90min? What was the bottleneck?
2. How do you make a DAG idempotent and safe to backfill?

### Cloud / AWS
1. Redshift performance tuning for your ETL warehouse?
2. (Likely) Your SAA lapsed in 2025 — talk me through your current cloud depth. *(Answer honestly: hands-on services used + renewal intent.)*

### Staff-level / leadership / strategy *(the gap zone — rehearse hardest)*
1. Tell me about a technical decision you drove **across teams**, not just within yours.
2. Walk me through an RFC or design doc you authored and how you built consensus.
3. How many engineers have you led/mentored, and in what structure?
4. Describe a time you set technical direction that others followed.
5. How do you grow engineers beyond code review?

## 6. Practice plan (2 weeks)

### Week 1 — breadth, fundamentals, and gap-closing
- **Day 1–2:** Distributed-systems fundamentals refresh (delivery semantics, consistency, idempotency, outbox/saga). Re-derive the exactly-once narrative end to end.
- **Day 3:** Go concurrency + service-design drills; sketch the microservices decomposition from memory.
- **Day 4:** Airflow/ETL + Redshift tuning refresh; write up the 6h→90min bottleneck analysis.
- **Day 5 (gap day):** Resolve the **scope ambiguities** — write down (truthfully) team sizes, your "led" scope on the migration, and how the 30% incident drop was measured. If unknown, decide your honest framing now.
- **Day 6 (gap day):** Draft **one RFC/design doc retroactively** for the Kafka pipeline or migration — gives you a written-judgment artifact to talk to.
- **Day 7:** Rest / light review of AWS services + SAA-lapse talking point.

### Week 2 — mocks + deep dives
- **Day 8:** System-design mock — design an order-processing system at scale (your home turf). Get feedback on capacity estimation and failure domains.
- **Day 9:** Deep-dive mock on exactly-once / Kafka internals; pressure-test the PyCon thesis.
- **Day 10:** Behavioral/leadership mock — drill the **cross-team influence** and **mentorship** questions (§5 leadership). This is the weakest area; spend the most reps here.
- **Day 11:** Go/coding mock (concurrency, service-level problem).
- **Day 12:** Second system-design mock outside your comfort zone (e.g., a non-commerce domain) to test breadth.
- **Day 13:** Review all four STAR stories aloud; cut filler, sharpen metrics, state scope precisely.
- **Day 14:** Light review + logistics; rest before the loop.

---
*Draft prep plan generated 2026-05-31. Companion to `profile.html` and `revision-plan.html`. Canonical source of truth for the prep content — keep `revision-plan.html` consistent with it.*
