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

This tranche establishes the authoritative physical model of the data center: what exists, where it is, how it is related, which generation is current, and which state may be trusted by higher control layers.

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

This tranche converts physical facility limits into explicit capacity, reservations, and placement authority without duplicating ASI workload scheduling or DFI path scheduling.

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

This tranche makes electrical delivery a first-class governed subsystem with explicit authority, topology, redundancy, failover, emergency behavior, and accounting.

| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 17 | [Power Control Plane](https://github.com/summonlabs/Power-Control-Plane) | Facility-wide electrical operating authority across modes, switching permissions, interlocks, protected obligations, capacity commitments, deterministic policy, authorization and control-attempt lifecycle, generation fencing, persistence, recovery, and verified-effect semantics. | Which electrical operating state and control authority are valid now, which actions are permitted under current topology, capacity, interlock and policy evidence, and which attempts must be refused as stale, unsafe, unauthorized or inconsistent? |
| 18 | [Power Topology](https://github.com/summonlabs/Power-Topology) | Generation-bound structural electrical topology across utility feeds, switchgear, transformers, UPS systems, buses, PDUs, circuits, transfer links, redundancy groups, powered dependencies, persistence, recovery, and stale-topology fencing without asserting energization or control authority. | What electrical infrastructure exists in this generation, how is it connected and dependent, which paths and redundancy relationships are structurally possible, and when must a topology claim be rejected as invalid or stale? |
| 19 | [Feed Authority](https://github.com/summonlabs/Feed-Authority) | Generation-bound electrical feed-serving authority across admissibility, redundancy, maintenance, failure and policy constraints, explicit allow/deny/indeterminate outcomes, durable grants, evidence revalidation, epoch fencing, persistence, recovery, and rollback protection without owning switching. | Which feed may serve this load now, under which mode, redundancy, maintenance, failure and policy constraints, and exactly why must an alternate feed be allowed, denied, or left indeterminate? |
| 20 | [PDU Control](https://github.com/summonlabs/PDU-Control) | Vendor-neutral lifecycle and safe control of PDUs and branch circuits across device generations, operating state, limits, telemetry, permission and interlock references, command attempts, acknowledgement/observation/verified-effect separation, durable journaling, recovery, and stale-authority fencing. | Given authoritative permission and current evidence, what transition may be attempted, under which limits and interlocks, and how do we prove the effect occurred rather than being acknowledged? |
| 21 | [UPS Control](https://github.com/summonlabs/UPS-Control) | Vendor-neutral UPS lifecycle and control across identity, hardware generations, operating state, reserve evidence, protected-load obligations, bypass and transfer readiness, recharge/discharge permissions, command attempts, acknowledgement/observation/verified-effect separation, persistence, recovery, and stale-authority fencing. | What operating transition may this UPS safely perform now, given its protected-load obligations, reserve evidence, bypass state and current authority, and how is acknowledgement kept separate from verified electrical effect? |
| 22 | [Generator Control](https://github.com/summonlabs/Generator-Control) | Vendor-neutral standby-generator lifecycle and control across readiness, safety interlocks, fuel and consumable evidence, synchronization and transfer eligibility, start/stop/test/emergency authority, command/acknowledgement/effect separation, durable audit, recovery, and stale-authority fencing. | Is this generator eligible and authorized to start, run, synchronize, transfer, test or stop now, under which safety, resource and fuel evidence, and how do we prove the transition without conflating acknowledgement with electrical effect? |
| 23 | [Load Shedding](https://github.com/summonlabs/Load-Shedding) | Deterministic facility load-shedding authority across protected obligations, priority and service classes, staged reduction policy, eligible-load classification, exact residual and overshoot accounting, restoration ordering, generation-bound plans, persistence, recovery, replay, and stale-authority fencing without owning actuation. | Given a quantified facility power shortfall and current obligations, priorities and policy, which loads may be shed in which stages, how much deficit remains, and in what order may service return? |
| 24 | [Energy Ledger](https://github.com/summonlabs/Energy-Ledger) | Durable provenance-preserving electrical-energy accounting across delivered, consumed, curtailed, wasted/lost, unclassified, and committed energy, with exact integer units, conflict preservation, residual reconciliation, correction records, integrity chaining, persistence, recovery, and writer fencing. | What energy was authoritatively recorded for this facility object and interval, under which source and generation, how does it reconcile across accounting categories, and which residuals or conflicts remain unexplained? |

## Thermal and Cooling Control

This tranche treats heat removal, coolant delivery, airflow, thermal headroom, and thermal emergencies as explicit control-plane resources.

| # | Runtime | Systems boundary | Core question |
| ---: | --- | --- | --- |
| 25 | [Thermal Control Plane](https://github.com/summonlabs/Thermal-Control-Plane) | Facility-wide thermal operating authority across modes, generation-bound limits, headroom evidence, derating, escalation, recovery, policy precedence, placement/power coordination, durable state, replay, and stale-authority fencing without actuating physical plant. | Which facility-wide thermal mode and authority are valid now, given current headroom evidence, limits, degraded conditions and control generation, and which actions must be refused as stale, unsafe or unevidenced? |
| 26 | [Cooling Topology](https://github.com/summonlabs/Cooling-Topology) | Generation-bound structural cooling topology across plants, chillers, pumps, loops, CDUs, CRAH/CRAC units, manifolds, branches, containment, serving/dependency relations, redundancy groups, persistence, recovery, and stale-generation fencing without asserting operational state. | What cooling infrastructure exists in this topology generation, how is it connected, which redundancy relationships are structurally possible, and when must a topology claim be rejected as invalid or stale? |
| 27 | [Cooling Capacity Accounting](https://github.com/summonlabs/Cooling-Capacity-Accounting) | Provenance-bound constituent cooling-capacity accounting across zones, loops, equipment classes and media, with exact integer quantities, reserve/redundancy obligations, degraded/unavailable/unknown states, signed residuals, durable generations, recovery, and fencing without owning facility-level allocation. | What cooling-removal capability is authoritatively accounted for in this exact generation, what remains unknown or unavailable, and which residuals or conflicts prevent a stronger capacity claim? |
| 28 | [Airflow Control](https://github.com/summonlabs/Airflow-Control) | Vendor-neutral airflow-oriented control across rooms, rows, racks, pressure relationships, containment, fan policy, safe operating envelopes, authority, command attempts, acknowledgement/observation/verified-effect separation, durable state, recovery, and stale-authority fencing. | Given current airflow and pressure evidence, containment state, thermal obligations, fan policy, authority and generation, what control transition may be attempted safely, and how is commanded state kept separate from observed and verified physical effect? |
| 29 | [Liquid Cooling Control](https://github.com/summonlabs/Liquid-Cooling-Control) | Vendor-neutral liquid-cooling control across loops, pumps, valves and CDUs, with leak-sensitive actuation, service authority, command/acknowledgement/effect separation, durable attempts, restart fencing, and verified physical-effect semantics. | Given current authority, device generation, interlocks, leak evidence, flow and pressure state and service obligations, which cooling transition may be attempted safely, and how is the effect proven rather than acknowledged? |
| 30 | [Thermal Zone Manager](https://github.com/summonlabs/Thermal-Zone-Manager) | Zone-level thermal semantics across declared envelopes, generation-stamped temperature evidence, headroom, thermal coupling, derating, hysteresis, bounded placement constraints, durable state, and stale-evidence handling without owning placement or actuation. | What is the generation-bound thermal state of each zone, how much headroom exists within its declared envelope, how do coupled zones constrain that answer, what derating follows, and which placement constraints must be surfaced without taking over authority? |
| 31 | [Cooling Failover](https://github.com/summonlabs/Cooling-Failover) | Cooling failover orchestration across alternate source eligibility, topology/capacity/policy evidence, reserve blocks, generation-bound plans, request/effect separation, partial-transition state, durable attempts, recovery, and stale-plan fencing without owning device actuation. | When a cooling source becomes unavailable or degraded, which alternate arrangement is eligible under current topology, capacity, policy and authority — and how is that transition fenced against stale plans? |
| 32 | [Thermal Emergency Manager](https://github.com/summonlabs/Thermal-Emergency-Manager) | Facility thermal-emergency coordination across authoritative incident state, severity, bounded mitigation requests, protected obligations, escalation, verification, hysteresis, dwell, recovery, durable generations, and stale-authority fencing without taking over adjacent actuators. | During a thermal excursion, which emergency state is authoritative, which bounded mitigations must be requested now, which protected obligations constrain them, when escalation must continue, and what current evidence recovery requires? |

## Physical Fleet Lifecycle

This tranche governs physical infrastructure from commissioning through turnup, maintenance, draining, upgrades, replacement, and decommissioning.

| # | Runtime | Systems boundary | Core question |
|---:|---|---|---|
| 33 | [Commissioning Fabric](https://github.com/summonlabs/Commissioning-Fabric) | Governs entry of physical facility assets into service through identity, placement, dependency, readiness, policy, evidence, activation authority, and commissioned-state gating without owning adjacent registries or controllers. | Under the current facility generation, identity, placement, dependencies, readiness, policy and authority: may this asset enter service now, and if not, what evidence is missing? |
| 34 | [Decommissioning Fabric](https://github.com/summonlabs/Decommissioning-Fabric) | Governs safe retirement of physical facility assets through dependency closure, drain obligations, authority revocation, residual-state disposition, isolation readiness, removal authorization, and observed-removal proof without owning the external effects. | May this physical asset be safely retired under current dependencies, obligations, drains, authority, residual state, facility policy, and evidence — and what must be completed or revoked before final removal is authoritative? |
| 35 | [Rack Turnup Manager](https://github.com/summonlabs/Rack-Turnup-Manager) | Coordinates rack-level commissioning across composition, power, cooling, network attachment, inventory, firmware, health, dependencies, readiness, and fenced turnup authority while composing evidence from the owning systems. | Given the current rack composition and facility generations, is this rack safe and ready to enter service, which subsystems are proven ready, which are unknown or failed, and what exact turnup action may be authorized now? |
| 36 | [Hardware Lifecycle](https://github.com/summonlabs/Hardware-Lifecycle) | Owns canonical physical-hardware lifecycle state, legal transitions, generation-bound authority and evidence, replacement lineage, durable history, replay, and fencing without owning commissioning, maintenance, firmware policy, plant control, or health diagnosis. | What lifecycle state is authoritative for this hardware object now, which transitions are legal under the current generation and authority, and what lineage and history prove how it reached that state? |
| 37 | [Firmware Baseline Manager](https://github.com/summonlabs/Firmware-Baseline-Manager) | Governs facility-level firmware baseline policy, compatibility, conformance and drift, staged-rollout eligibility, exceptions, rollback eligibility, and generation-bound authorization without owning firmware execution or hardware discovery. | Which firmware baseline is authoritative for this hardware class and generation, and is a given asset conformant or eligible for staged rollout or rollback? |
| 38 | [Maintenance Coordinator](https://github.com/summonlabs/Maintenance-Coordinator) | Coordinates facility maintenance windows through dependency, redundancy, headroom, protected-obligation, drain, isolation, exception, restoration, and completion evidence without performing the underlying maintenance or infrastructure actions. | Can this maintenance activity proceed now without violating dependencies, redundancy, protected obligations, or active facility constraints — and what drains, isolations and verifications does the window require? |
| 39 | [Facility Drain Coordinator](https://github.com/summonlabs/Facility-Drain-Coordinator) | Governs removal of physical capacity from service through drain plans, consumer obligations, bounded requests, per-domain completeness, residual state, evidence, and fenced safe-to-remove authority without performing the external drain effects. | For this scope, which obligations must be evacuated or relinquished, which drains have actually completed, what residual capacity remains, and when is removal safe? |
| 40 | [Facility Change Orchestrator](https://github.com/summonlabs/Facility-Change-Orchestrator) | Plans and orchestrates generation-bound multi-domain facility changes across assets, lifecycle, power, cooling, capacity, fabric, and maintenance while preserving external authority ownership and explicitly handling stale plans, partial execution, verification, and rollback. | Given the current facility state and a requested change, what ordered plan is valid now, which authorities must act, and when must it stop, replan or be fenced as stale? |

## Facility Policy, Tenancy, and Entitlement

This tranche governs who may consume facility capability, under which physical, service-class, placement, maintenance, and operational constraints.

| # | Runtime | Systems boundary | Core question |
|---:|---|---|---|
| 41 | [Tenant Registry](https://github.com/summonlabs/Tenant-Registry) | Owns canonical facility tenancy identities, lifecycle, ownership relationships, service bindings, isolation-domain membership, provenance, tombstones, generation/revision authority, and durable registry state without owning IAM, billing, entitlement, placement, scheduling, or network segmentation. | Which tenancy identities exist, in which isolation domains, under which relationships, in which lifecycle state and revision, and which declarations may a higher control layer trust? |
| 42 | [Resource Envelope](https://github.com/summonlabs/Resource-Envelope) | Governs generation-bound facility resource constraints across space, power, cooling, rack exposure, redundancy, and operational counts, with exact residual arithmetic, identity binding, lifecycle, replay, and durable authority without owning capacity, measurements, placement, admission, or identity lifecycle. | Given this envelope revision and this evidence, is this request permitted, refused, or impossible to determine — and which dimension decided it? |
| 43 | [Service Class Registry](https://github.com/summonlabs/Service-Class-Registry) | Owns canonical facility service-class definitions as immutable, digest-addressed sets of typed availability, redundancy, maintenance, recovery, power, cooling, placement, and operational obligations, with composition, contradiction detection, lineage, and generation-bound binding. | What exactly does service class X, generation G, revision R mean, is it self-consistent, and is a decision still bound to the definition it was made against? |
| 44 | [Facility Admission Control](https://github.com/summonlabs/Facility-Admission-Control) | Decides whether new facility commitments may be accepted against generation-stamped capacity, protection, tenancy, service-class, envelope, maintenance, incident, placement, and policy evidence, emitting fenced grants and bounded reservation intent without performing the external effects. | May this facility commitment be accepted now, given the capacity, protection, tenancy, obligations and policy authoritative at this moment — and if not, exactly which authority or generation says no? |
| 45 | [Facility Placement Policy](https://github.com/summonlabs/Facility-Placement-Policy) | Governs physical-placement eligibility under tenant, service-class, jurisdiction, failure-domain, maintenance, and facility constraints, binding decisions to exact policy/evidence generations without ranking candidates, reserving capacity, scheduling work, or executing placement. | Given a proposed placement and the exact generation-bound evidence about the facility, is it allowed by the canonical policy, and if not, which requirement refused it? |
| 46 | [Maintenance Policy](https://github.com/summonlabs/Maintenance-Policy) | Governs facility-wide maintenance eligibility through generation-bound policy, obligation classes, blackout windows, redundancy evidence, waivers, approvals, and freshness rules without scheduling, draining, actuation, or measuring facility state. | May this maintenance, on this facility scope, for these obligation classes, in this window, proceed under the policy generation that is authoritative right now? |
| 47 | [Resource Entitlement](https://github.com/summonlabs/Resource-Entitlement) | Owns generation-bound facility entitlement authority over scope, quantity, priority, expiry, revocation, transfer, delegation, and exact remainder accounting, while leaving capacity, admission, placement, scheduling, enforcement, identity, and billing to adjacent authorities. | Is this entitlement live authority for this tenant, this service, this scope, and this quantity, right now, under the authority generations that actually justify it? |
| 48 | [Facility Policy Engine](https://github.com/summonlabs/Facility-Policy-Engine) | Evaluates facility-wide policy against typed, generation-tagged facts from adjacent authorities, preserving explicit unknown/stale/missing states, deterministic rule composition, explanation, and decision currentness without owning facility state or executing authorized actions. | Given one exact published policy generation and one typed, generation-tagged set of authoritative facts, what does policy decide, why, and is that decision still current? |

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
