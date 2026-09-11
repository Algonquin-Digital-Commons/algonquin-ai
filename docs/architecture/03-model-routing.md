# Model Routing

## Decision tree

When a request comes in for a given alias (e.g. `AC Reasoning`), the router
works down this tree:

```text
Can one GPU run it?
   YES → single GPU
   NO ↓
Can one multi-GPU node run it?
   YES → tensor parallel
   NO ↓
Compatible fast-network compute cell available? (from fpsdc-compute)
   YES → distributed serving across the cell
   NO ↓
Heterogeneous sharding possible?
   YES → experimental runtime (exo-style)
   NO ↓
Approved cloud?
   YES → cloud
   NO → queue / reject with reason
```

Until the compute-fabric workspace has something running, this collapses to
just the first and last branches: dedicated GPU, or cloud.

## Three-tier capacity model (target end state)

```text
                    TOTAL AI CAPACITY
             ┌─────────────────────┐
             │     Cloud Burst     │   overflow, expensive, massive
             ├─────────────────────┤
             │  Campus Compute     │   elastic, cheap-ish, opportunistic
             │      Fabric         │   (fpsdc-compute)
             ├─────────────────────┤
             │ Dedicated On-Prem   │   predictable, secure, fast
             │      GPUs           │
             └─────────────────────┘
```

Dedicated GPUs are the baseline. Campus compute fabric is the burst tier once
it exists. Cloud is the final overflow. This workspace should build and
operate correctly with **only the bottom tier** — the router should treat
campus-fabric and cloud as pluggable backends it can route to later without a
redesign.

## MVP scope

For the first vertical slice: one alias (`AC Fast` or similar), one backend
(one local model, one machine), no routing logic needed yet beyond "does this
alias exist." Build the decision tree as the second and third backends come
online, not before.
