# Distributed Fabric Infrastructure

Distributed Fabric Infrastructure is the open-source fabric-systems corpus from Summon Software Labs, focused on programmable, distributed network infrastructure for high-performance, AI, and large-scale systems.

Part of the [Summon Software Labs portfolio](README.md).

## Systems Scope

The architecture is concerned with the state, authority, topology, routing, capacity, congestion, failure, recovery, offload, observability, and lifecycle of distributed fabric infrastructure.

Each runtime owns one explicit systems boundary rather than collapsing topology, path selection, traffic engineering, queueing, congestion control, failure handling, offload, and physical-fabric state into one monolithic control plane.

The architecture will be published incrementally as individual runtime boundaries are released.

Current public infrastructure:

| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 1 | [Fabric Registry](https://github.com/summonlabs/Fabric-Registry) | Canonical identity, registration authority, lifecycle, generations, provenance, supersession, fencing, retirement, and revalidation for fabric entities including fabrics, sites, switches, routers, NICs, SmartNICs, DPUs, ports, and links. | What entities exist under this authority domain, what are their canonical identities and generations, and when must a registration be rejected, superseded, fenced, retired, or revalidated? |
| 2 | [Fabric Topology](https://github.com/summonlabs/Fabric-Topology) | Authoritative topology structure, relationship governance, generations, provenance, currentness, reconciliation, snapshots, diffs, explanations, and structural validation across fabric entities. | What is connected to what, through which current relationships, under which generation, provenance, and control-plane authority, and when must a relationship or snapshot be rejected as stale, conflicting, invalid, or non-authoritative? |
