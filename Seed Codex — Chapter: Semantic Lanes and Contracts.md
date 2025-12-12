Seed Codex — Chapter: Semantic Lanes and Contracts

There was a temptation, early on, to resurrect ATM: tiny cells, virtual circuits, hard QoS. Chorus does not live on that layer, but the desire it expressed was real.

The desire was for promises.

Not “best effort,” not “maybe this packet arrives in time,” but specific guarantees:

this conversation gets priority;

that replication can be slow;

this control signal must not wait behind bulk uploads.

The correct place for those ideas is not L2. It is at the semantic edge: Bubbles, Songs, and Bells.

Chorus defines semantic lanes. Each lane is a conceptual channel with a purpose and a contract. For example:

a control lane between Spirit and each node, carrying small, latency-sensitive commands and status;

a reflex lane, where hounds bark and todes gossip about local events;

a bulk lane, where large artifacts, backups, or datasets move slowly;

a human lane, where UI-centric traffic flows, often through the lab to the eyes and hands.

Each Bubble-level action chooses a lane when it sends a Song. The lane is not optional; declaring which lane you are using is part of behaving well in Chorus.

In return, Routing Gemma and the underlay promise lane-specific treatment:

Control lane packets are marked for low latency and jitter, and spared from aggressive batching.

Bulk lane packets are explicitly de-prioritised when contention appears.

Reflex lanes may have burst limits or shaping to prevent bark storms.

Human lanes may receive a mix of latency and throughput guarantees, depending on interaction patterns.

Underneath, these promises compile down to tc classes, qdiscs, DSCP bits, maybe different queues on switches that support them. The exact mapping can change per hardware generation. The lanes themselves are stable.

A lane is defined by:

its purpose;

its intended scale (bytes/sec and packets/sec range);

its criticality (how bad is it if delayed or dropped);

its symmetry (client/server, peer/peer, broadcast).

A contract is an agreement between a producer in that lane and the Edge Cortex:

how much it may send in steady state;

how it should behave under backpressure;

how it will be notified of congestion.

In practice, contracts are not signed once; they are learned. Routing Gemma watches Bells emitted by the rest of Chorus:

when queues build up;

when latency spikes;

when flows regularly exceed their implicit budget.

From this, Gemma can propose new contract parameters or flag misbehaving actors:

“Node A’s reflex lane is exceeding its intended budget; consider slowing its poll interval.”

“The bulk lane between lab and far limb is saturating the trunk; schedule transfers for off-peak hours.”

“Human lane latency is degrading when backups run; adjust class weights.”

What matters for the Codex is that we admit two truths:

The raw network cannot see semantics; it can only see addresses and ports.

Chorus can see semantics but must voluntarily communicate them to the Edge Cortex.

Semantic lanes are how we bridge that gap. They are the vocabulary that lets a model say:

“This is more important than that,”
and have the underlay understand.

Without lanes, everything is just “more traffic.” With lanes, the router does not need to guess; it needs only to enforce contracts that Chorus itself declared.

There is an ethical consequence hidden here, too.

By concentrating shaping knowledge in Chorus-defined lanes, we avoid building a general-purpose DPI monster that peers into arbitrary encrypted traffic. The Edge Cortex learns about our own flows in detail, and about external flows only in aggregate. That fits the Codex preference for introspection over surveillance.

The evolution path is clear:

Early Chorus: manual lanes, hand-written policies.

Mature Chorus: Routing Gemma learns contracts from experience and negotiates them with Spirit.

Future siblings: can change how lanes are mapped to DSCP/qdiscs, or even which lanes exist, but must preserve the notion that traffic is shaped based on declared semantics, not guesswork.

The ATM instinct is honoured, without dragging 53-byte cells into 2025.