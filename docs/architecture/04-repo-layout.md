# PSDC AI Repository Layout

This is the product-local workspace for the PSDC AI gateway and closely coupled
services. It is one repository in the ADR-0022 polyrepo ecosystem, not the home
for independently released desktop and mobile products.

```text
psdc-ai/
│
├── apps/
│   ├── web/                 # Commons AI Web; gated v0.6.5 BSD scaffold
│   ├── code/                # thin OpenCode integration
│   └── admin/
│
├── services/
│   ├── gateway/              # MAIN SERVICE — build first
│   ├── identity/             # build second
│   ├── model-router/         # build third
│   ├── session-host/         # agent-neutral local session authority
│   ├── session-relay/        # content-blind E2EE cross-device transport
│   ├── policy/
│   ├── usage/
│   ├── files/
│   ├── rag/
│   └── telemetry/
│
├── inference/
│   ├── general/
│   ├── code/
│   ├── embeddings/
│   └── rerank/
│
├── sdk/
│   ├── python/
│   ├── typescript/
│   └── cli/
│
├── infrastructure/
│   ├── containers/
│   ├── kubernetes/
│   ├── opentofu/
│   └── monitoring/
│
├── policies/
│   ├── model-registry/
│   ├── routing/
│   ├── quotas/
│   └── data-classification/
│
└── docs/
    ├── architecture/
    ├── security/
    ├── privacy/
    ├── governance/
    └── operations/
```

## What to build first

Only three of these folders matter for the first vertical slice:

1. `services/gateway/` — the gateway itself
2. `apps/web/` — Commons AI Web, initially scaffolded from the verified
   Open WebUI v0.6.5 BSD source and pointed only at the gateway
3. `apps/code/` — a thin OpenCode integration pointed at the gateway

`services/identity/` and `services/model-router/` can start as code *inside*
the gateway service and get extracted later once there's a real reason to run
them independently (separate scaling needs, separate team ownership). Don't
pre-split services that don't have a concrete reason to be separate yet — it
just adds deployment and networking overhead for no benefit at this stage.

`sdk/`, `inference/`, `rag/`, `files/`, `telemetry/`, and everything under
`policies/` are Phase 4+ concerns (see `05-roadmap-phases.md`). Creating empty
folders for them now is fine; putting real code in them now is premature.

Desktop and mobile move to `psdc-desktop` and `psdc-mobile` so
their upstream provenance, permissions, releases and app-store/package pipelines
remain independent. Build the Session Host contract before remote control and the
content-blind Session Relay before connecting mobile.

## Note on web and coding clients

Use supported extension points or thin downstream forks:

```text
open-webui/v0.6.5    →  psdc-ai/apps/web (one-time verified import)
upstream/opencode    →  psdc-ai/apps/code
different-ai/openwork (MIT core only) → psdc-desktop
slopus/happy (verified MIT baseline)  → psdc-mobile
```

OpenCode follows the thin-upstream strategy. Commons AI Web follows ADR-0009:
v0.6.5 is a frozen BSD scaffold, later Open WebUI code is not merged without a new
license decision, and Downstream maintainers assume security, dependency, browser, and
accessibility maintenance. Avoid deep restyling before gateway, identity, and
routing contracts settle because those contracts determine the durable client
boundary.

ADR-0018 excludes OpenWork `ee/`, Den, hosted MCP, and hosted inference. ADR-0019
requires Happy mobile to use the institution-controlled E2EE relay and the
provider-neutral Agent Session Contract. Neither client may become a provider,
identity, policy, or hosted-control-plane dependency.
