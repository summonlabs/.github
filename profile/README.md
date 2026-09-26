# Summon Software Labs

Summon Software Labs develops open-source accelerated-computing infrastructure, distributed fabric systems, AI systems architecture, and runtime execution governance.

The work focuses on decomposing complex infrastructure into explicit, vendor-neutral runtime boundaries with deterministic ownership, authority, lifecycle, recovery, provenance, and evidence.

Each runtime owns one explicit systems boundary. Memory does not silently own scheduling. Scheduling does not silently own authority. Completion does not imply commit. Discovery does not imply capability. A cache hit does not imply reuse eligibility. A process exit does not imply authoritative output. A replica does not imply currency. A hardware fact does not imply freshness.

Authority is explicit: a decision remains valid only while the conditions that justified it remain current, and stale authority is fenced rather than silently inherited.

The architecture separates these concerns so that every transition can be governed, tested, fenced, replayed, recovered, and explained independently.

## Portfolio

## Accelerated Systems Infrastructure

[Accelerated Systems Infrastructure](accelerators.md) is the open-source accelerated-computing substrate from Summon Software Labs: a cumulative corpus of 112 narrowly scoped runtimes spanning accelerator memory and reusable state through inference serving, execution, compilation, communication, storage, infrastructure composition, autonomous systems, heterogeneous accelerator federation, coherence, virtualization, persistent execution, fault containment, cross-cluster state, runtime evolution, and hardware capability truth.

## Distributed Fabric Infrastructure

[Distributed Fabric Infrastructure](fabrics.md) is the open-source distributed-fabric substrate from Summon Software Labs: a cumulative corpus of 102 narrowly scoped runtimes spanning fabric identity, topology, routing, traffic engineering, capacity, congestion, AI-aware communication, failure and recovery, lifecycle control, observability, NIC/SmartNIC/DPU offload, physical and optical infrastructure, rack and site composition, inter-site authority, and federation.

## Runtime Execution Governance

[Runtime Execution Governance](execution-governance.md) covers AIGOS, the open-source AI Governance Operating System foundation beneath AGIOS, and the AIGOS supervisor daemon aigosd: deterministic, policy-bounded, auditable runtime control over intelligence execution, including authority, jurisdiction, cost attribution, formal assurance, and liability.

[AGIOS, the Artificial General Intelligence Operating System](https://summonsoftware.net)
