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
