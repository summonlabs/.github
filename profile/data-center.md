# Data Center Control Plane

Data Center Control Plane is the open-source facility-control corpus from Summon Software Labs, focused on the physical-facility authority layer above Accelerated Systems Infrastructure and Distributed Fabric Infrastructure.

Part of the [Summon Software Labs portfolio](README.md).

## Systems Scope

The Data Center Control Plane composes compute and network infrastructure with the physical resources and operating constraints of a data center: facility identity, topology, assets, racks, physical location, dependencies, capacity, placement, electrical delivery, cooling, lifecycle, tenancy, failure domains, observability, economics, and multi-site authority.

The portfolio is intentionally cumulative.

Each runtime owns one explicit systems boundary rather than collapsing facility state, capacity, electrical control, cooling control, lifecycle, policy, failure handling, observability, and site federation into one monolithic control plane.

Physical existence does not imply operational readiness. Installed capacity does not imply allocatable capacity. Placement does not imply sufficient power. Power availability does not imply cooling headroom. Observation does not imply authority. A maintenance request does not imply drain completion. A recovered controller does not inherit stale mutation rights. A facility state does not remain authoritative after the generations, epochs, dependencies, evidence, or policy that justified it become stale.

The architecture separates these concerns so that every transition can be governed, tested, fenced, replayed, recovered, reconciled, and explained independently.

Accelerated Systems Infrastructure answers whether computation can execute under exact accelerator, memory, state, resource, and execution authority. Distributed Fabric Infrastructure answers whether those resources can communicate under exact topology, path, bandwidth, failure-isolation, and network authority. Data Center Control Plane answers whether the physical data center can sustain and authorize the resulting arrangement across space, racks, power, cooling, dependencies, capacity, tenancy, maintenance, incidents, and site-level policy.

Together, [Accelerated Systems Infrastructure](accelerators.md), [Distributed Fabric Infrastructure](fabrics.md), and Data Center Control Plane form an open-source data center operating model.

## Contents

The canonical architecture is organized into nine tranches:

- [Canonical Facility State](#canonical-facility-state)
- [Facility Capacity and Placement](#facility-capacity-and-placement)
- [Electrical Infrastructure Control](#electrical-infrastructure-control)
- [Thermal and Cooling Control](#thermal-and-cooling-control)
- [Physical Fleet Lifecycle](#physical-fleet-lifecycle)
- [Facility Policy, Tenancy, and Entitlement](#facility-policy-tenancy-and-entitlement)
- [Facility Failure and Recovery](#facility-failure-and-recovery)
- [Facility Observability and Economics](#facility-observability-and-economics)
- [Site and Multi-Site Control](#site-and-multi-site-control)

Current public infrastructure:

## Canonical Facility State
| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 1 | [Data Center Registry](https://github.com/summonlabs/Data-Center-Registry) | Canonical data-center identity, site membership, control-plane generations, lifecycle state, authoritative facility metadata, ownership, provenance, persistence, recovery, and stale-generation fencing. | Which data-center identity and site membership are authoritative now, under which generation and lifecycle state, and when must stale or superseded registry state be rejected? |
| 2 | [Facility Topology](https://github.com/summonlabs/Facility-Topology) | Generation-bound physical topology across buildings, halls, rooms, rows, racks, zones, adjacency, containment boundaries, power/cooling domains, dependencies, persistence, recovery, and authoritative publication. | What physical facility topology exists under the current generation, how are its objects related, and when must a topology snapshot or mutation be rejected as stale, invalid, or non-authoritative? |
| 3 | [Asset Registry](https://github.com/summonlabs/Asset-Registry) | Authoritative physical-asset identity, ownership, class, generation, lifecycle, capability references, installation state, replacement lineage, provenance, persistence, recovery, and stale-authority rejection. | What physical assets exist, which exact identities and generations are current, where are they in their lifecycle, and when must an asset claim be rejected, superseded, retired, or fenced? |
| 4 | [Rack Registry](https://github.com/summonlabs/Rack-Registry) | Canonical rack identity, composition, occupancy, mounting geometry, power/cooling associations, compatibility, lifecycle, generation-bound membership, persistence, recovery, and mutation fencing. | What rack exists, what occupies it, which membership and lifecycle generation are authoritative, and when must a placement or rack mutation be refused as conflicting or stale? |
| 5 | [Physical Location Registry](https://github.com/summonlabs/Physical-Location-Registry) | Stable physical addressing across sites, rooms, halls, rows, cages, racks, rack units, and zones, with hierarchy, move/replacement semantics, canonical paths, provenance, persistence, recovery, and stale-location fencing. | Where is this physical object located now, what stable identity survives movement or replacement, and when must an address or location claim be rejected as stale, conflicting, or non-canonical? |
| 6 | [Facility Dependency Registry](https://github.com/summonlabs/Facility-Dependency-Registry) | Typed dependency authority across physical assets, racks, feeds, cooling loops, facility services, and composed ASI/DFI domains, with graph invariants, impact traversal, generations, persistence, recovery, and stale-state fencing. | What depends on what across the facility, which dependency relationships are authoritative now, and what impact follows when a dependency changes, fails, or becomes stale? |
| 7 | [Facility State Ledger](https://github.com/summonlabs/Facility-State-Ledger) | Durable provenance-preserving record of authoritative facility-state transitions, accepted observations, mutations, reconciliation events, historical generations, integrity chains, replay, recovery, and writer-incarnation fencing. | What authoritative facility-state transition happened, in what committed order, under which provenance and authority, and can that history be verified and replayed after restart or failure? |
| 8 | [Control Plane Epoch](https://github.com/summonlabs/Control-Plane-Epoch) | Facility-wide incarnation and epoch authority across controller identity, advancement, revocation, stale-controller fencing, superseded observations, recovered-state qualification, durable floors, persistence, recovery, and deterministic authority resolution. | Which facility control-plane epoch and controller incarnation are authoritative now, and which prior mutation rights, observations, or recovered state must be permanently fenced? |

## Facility Capacity and Placement
| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 9 | [Facility Capacity](https://github.com/summonlabs/Facility-Capacity) | Aggregate generation-bound facility-capacity authority across space, rack, power, cooling, operational reserves, facility-service constraints, typed evidence composition, exact accounting identities, persistence, recovery, revalidation, and stale-authority fencing. | What facility capacity is actually usable now, under which physical constraints, reserves, service obligations, evidence and generation, and when must that answer be rejected as stale, incomplete, or non-authoritative? |
| 10 | [Rack Capacity](https://github.com/summonlabs/Rack-Capacity) | Per-rack capacity accounting across mounting slots, power, cooling, weight, serviceability, committed reservations, conservative bounds, three-valued fit evaluation, generation-bound evidence, persistence, recovery, and stale-authority rejection. | How much capacity does this exact rack generation have left across physical slots, power, cooling, weight and serviceability, which constraint binds first, and when is the available evidence insufficient to decide? |
| 11 | [Space Capacity](https://github.com/summonlabs/Space-Capacity) | Exact physical-space capacity accounting across facility containment, rooms, halls, rows, cages, racks, rack units, occupancy, reserved footprint, expansion zones, incompatibilities, service clearances, persistence, recovery, and mutation fencing. | What physical space is genuinely available now, at which hierarchy level and generation, after occupancy, incompatibilities, reserved footprint, clearance and expansion constraints are applied? |
| 12 | [Power Capacity](https://github.com/summonlabs/Power-Capacity) | Generation-bound electrical-capacity accounting across power domains, nominal and allocatable capacity, protected load, reserve, derating, redundancy obligations, committed loads, evidence freshness, persistence, recovery, rollback protection, and stale-authority fencing. | How much electrical capacity is allocatable now, through which power domains and redundancy assumptions, after protected load, reserve, derating and current evidence are applied, and when must that answer be refused as stale or unknown? |
| 13 | [Cooling Capacity](https://github.com/summonlabs/Cooling-Capacity) | Generation-bound cooling-capacity accounting across thermal zones, loops, plants, manifolds, equipment, shared capacity pools, redundancy, locality, compatibility, degradation, committed and observed load, persistence, recovery, and stale-authority fencing. | How much thermal-removal capacity is safely usable now, where is it, under which cooling dependencies and redundancy assumptions, and what evidence makes that answer authoritative? |
| 14 | [Facility Placement Planner](https://github.com/summonlabs/Facility-Placement-Planner) | Deterministic facility placement planning across space, power, cooling, redundancy, serviceability, dependencies, evidence completeness, policy, generation-bound snapshots, ranking, explanation, persistence, recovery, and stale-authority rejection without granting placement authority. | Given an asset or placement request and an exact authoritative facility snapshot, where may it be placed, in what deterministic order, and why is each candidate accepted, rejected, or indeterminate? |
| 15 | [Facility Capacity Reservation](https://github.com/summonlabs/Facility-Capacity-Reservation) | Generation-preconditioned facility-capacity commitments across space, racks, power, cooling, and facility services, with reservation lifecycle, exact accounting closure, idempotency, expiry, revocation, reconciliation, durable publication, recovery, and stale-authority fencing. | Can this commitment be made against the exact current capacity generations, what does it bind, and when must it expire, release, revoke, reconcile, or be fenced as stale? |
| 16 | [Capacity Reconciliation](https://github.com/summonlabs/Capacity-Reconciliation) | Deterministic reconciliation across planned, reserved, installed, observed, usable, and allocatable facility capacity, preserving evidence conflicts, unexplained residuals, precedence policy, provenance, generation-bound authority, persistence, recovery, history, and stale-state refusal. | When capacity views disagree, what can be reconciled deterministically, what remains unexplained, and which evidence is authoritative enough to drive the next decision? |

## Electrical Infrastructure Control
| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 17 | [Power Control Plane](https://github.com/summonlabs/Power-Control-Plane) | Facility-wide electrical operating authority across modes, switching permissions, interlocks, protected obligations, capacity commitments, deterministic policy, authorization and control-attempt lifecycle, generation fencing, persistence, recovery, and verified-effect semantics. | Which electrical operating state and control authority are valid now, which actions are permitted under current topology, capacity, interlock and policy evidence, and which attempts must be refused as stale, unsafe, unauthorized or inconsistent? |
| 18 | [Power Topology](https://github.com/summonlabs/Power-Topology) | Generation-bound structural electrical topology across utility feeds, switchgear, transformers, UPS systems, buses, PDUs, circuits, transfer links, redundancy groups, powered dependencies, persistence, recovery, and stale-topology fencing without asserting energization or control authority. | What electrical infrastructure exists in this generation, how is it connected and dependent, which paths and redundancy relationships are structurally possible, and when must a topology claim be rejected as invalid or stale? |
| 19 | [Feed Authority](https://github.com/summonlabs/Feed-Authority) | Generation-bound electrical feed-serving authority across admissibility, redundancy, maintenance, failure and policy constraints, explicit allow/deny/indeterminate outcomes, durable grants, evidence revalidation, epoch fencing, persistence, recovery, and rollback protection without owning switching. | Which feed may serve this load now, under which mode, redundancy, maintenance, failure and policy constraints, and exactly why must an alternate feed be allowed, denied, or left indeterminate? |
| 20 | [PDU Control](https://github.com/summonlabs/PDU-Control) | Vendor-neutral lifecycle and safe control of PDUs and branch circuits across device generations, operating state, limits, telemetry, permission and interlock references, command attempts, acknowledgement/observation/verified-effect separation, durable journaling, recovery, and stale-authority fencing. | Given authoritative permission and current evidence, what transition may be attempted, under which limits and interlocks, and how do we prove the effect occurred rather than being acknowledged? |

## Thermal and Cooling Control

This tranche treats heat removal, coolant delivery, airflow, thermal headroom, derating, failover, and thermal emergencies as explicit control-plane resources.

## Physical Fleet Lifecycle

This tranche governs physical infrastructure from commissioning through turnup, maintenance, draining, upgrades, replacement, and decommissioning.

## Facility Policy, Tenancy, and Entitlement

This tranche governs who may consume facility capability, under which physical, service-class, placement, maintenance, and operational constraints.

## Facility Failure and Recovery

This tranche contains physical-facility failures, preserves useful service under degraded conditions, and coordinates recovery across facility resources and lower-layer ASI/DFI obligations.

## Facility Observability and Economics

This tranche explains authoritative facility state, capacity, health, power, thermal and cooling behavior, resource efficiency, and operating economics from evidence rather than inference.

## Site and Multi-Site Control

This tranche composes complete facilities into larger authority domains across site control, federation, regional capacity, cross-site placement and reservation, disaster recovery, global policy, and live control-plane evolution.

## Architecture

The portfolio is designed as a cumulative physical-facility control plane rather than a collection of independent management utilities.

Lower-level DCCP runtimes establish canonical facility identity, topology, asset and rack state, physical location, dependency structure, durable facility history, and control-plane epoch authority. Higher layers consume those boundaries to govern capacity, placement, electrical infrastructure, thermal and cooling resources, fleet lifecycle, tenancy, policy, facility failure and recovery, observability, economics, and multi-site composition.

Authority is explicit throughout the architecture.

A facility decision remains valid only while the identities, generations, epochs, physical dependencies, capacity evidence, power and cooling state, lifecycle state, reservations, maintenance conditions, incidents, policy, service obligations, and lower-layer ASI/DFI authority that justified it remain current. When those conditions change, stale authority is fenced rather than silently inherited.

This allows the architecture to distinguish states that facility-management systems often conflate:

- physically present versus registered
- registered versus authoritative
- installed versus operational
- available versus allocatable
- allocatable versus reserved
- reserved versus admitted
- placed versus serviceable
- powered versus power-authorized
- cooled versus within thermal authority
- observed versus authoritative
- healthy versus eligible
- degraded versus unavailable
- acknowledged versus verified
- requested versus committed
- committed versus still current
- recovered versus revalidated
- local authority versus delegated authority
- site capacity versus globally usable capacity

The dependency direction is explicit:

**Data Center Control Plane**  
composes  
**Accelerated Systems Infrastructure + Distributed Fabric Infrastructure**  
which govern  
**compute/execution + communication**  
mapped onto  
**accelerators + memory + network + storage + power + cooling + physical plant**

Each runtime therefore exposes facility-control infrastructure that serious AI, high-performance, cloud, and large-scale systems operators would otherwise need to design, integrate, harden, validate, and maintain independently.

Together, the runtimes form a layered systems architecture spanning canonical physical facility state through capacity, power, cooling, lifecycle, policy, recovery, observability, economics, and multi-site operation.

## Data Center Systems Engineering

Primary areas of work include:

- canonical data-center identity, site membership, lifecycle, provenance, and generation control
- generation-bound facility topology, physical adjacency, containment, and dependency structure
- authoritative physical-asset identity, installation state, capability references, and replacement lineage
- canonical rack identity, composition, occupancy, compatibility, and membership authority
- stable physical addressing across rooms, rows, cages, racks, rack units, and zones
- typed dependency graphs across assets, racks, electrical feeds, cooling loops, facility services, and ASI/DFI domains
- durable provenance-preserving facility-state history, replay, reconciliation, and integrity
- facility-wide control-plane epochs, controller incarnation, stale-authority fencing, and recovery
- facility capacity across space, racks, power, cooling, operational reserves, and service constraints
- physical placement, reservation, admission, reconciliation, and serviceability
- electrical topology, feed authority, distribution, UPS, generator, load-shedding, and energy accounting
- thermal topology, cooling capacity, airflow, liquid cooling, thermal zones, failover, and emergency control
- commissioning, decommissioning, rack turnup, firmware baselines, maintenance, drain, and facility change orchestration
- facility tenancy, resource envelopes, service classes, entitlements, placement policy, maintenance policy, and admission
- physical failure domains, incident state, degraded operation, cross-domain recovery, rack evacuation, and black start
- evidence-bound facility, power, thermal, cooling, capacity, and asset-health observability
- deterministic facility-efficiency accounting and energy-cost governance
- site-level composition, data-center federation, regional capacity, cross-site placement and reservation
- disaster recovery, global policy federation, and live data-center control-plane evolution
- persistent state, integrity verification, restart recovery, replay protection, and incarnation fencing
- real multiprocess authority, crash/restart validation, adversarial persistence testing, and conservative recovery
- deterministic explanations, conflict preservation, UNKNOWN handling, and exact accounting closure
