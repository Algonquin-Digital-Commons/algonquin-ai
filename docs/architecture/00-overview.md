# System Overview

## Core principle

**No client talks to model runtimes or external providers directly.** The web and
OpenCode integrations are clients of one shared backend: the Commons AI Gateway.
Every other client (mobile, desktop, third-party SDKs, LMS integrations) uses the same
gateway. This is the single most important architectural decision in the whole
project — get this wrong and you end up with N independent integrations instead
of one platform.

```text
Web client ───┐
OpenCode ─────┤
Mobile ───────┤
Python SDK ───┤
JS SDK ───────┤──→ Commons AI Gateway → everything else
Optional LMS ─┤
IDE clients ──┘
```

## High-level layout

```text
                     COMMONS AI
                         │
              ai.<institution-domain>
                         │
            ┌────────────┴────────────┐
            │                         │
       Web / Mobile              Developer Access
            │                         │
    Open web client           OpenCode integration
            │                         │
            └────────────┬────────────┘
                         │
                  COMMONS AI API
                     / GATEWAY
                         │
       ┌─────────────────┼──────────────────┐
       │                 │                  │
   Identity          Policy/Quota       AI Services
     Layer              Layer              Layer
       │                 │                  │
Keycloak broker     RBAC / DLP       Models / RAG / Tools
 OIDC / OAuth       Rate limits      Agents / Search
 Device Login       Budgets          Embeddings / Code
       │                 │                  │
       └─────────────────┼──────────────────┘
                         │
                   MODEL ROUTER
                         │
         ┌───────────────┴──────────────┐
         │                              │
     AC / Commons Compute Fabric SELF-HOSTED COMPUTE AND USER-OWNED EDGE
                            │
             vLLM / SGLang / llama.cpp adapters
                            │
                  openly licensed models
                        │
                 DATA PLATFORM
                        │
       PostgreSQL / Valkey / Ceph Object Storage
       Vector DB / telemetry / audit / backups
```

## The two APIs

| API | Purpose |
|---|---|
| OpenAI-compatible (`/v1/...`) | Lets OpenCode and existing compatible tooling integrate without a custom protocol; Open WebUI is a test target, not a dependency |
| Commons-native (`/ac/v1/...`) | Institution-specific functionality that doesn't fit OpenAI's API shape: `me`, `usage`, `files`, `knowledge`, `tools`, `courses`, `policies`, `agents`, `projects` |

## Why not "just deploy a chat UI with a plugin"

Because the differentiator isn't the chat UI, it's what sits behind it: SSO as
the identity root, policy-aware local/cloud routing, quotas, auditability, and
an API surface students can build against without needing their own vendor API
keys. Web and coding clients are interfaces into that platform, not the platform
itself. Current Open WebUI releases are not OSI-approved and therefore cannot be
the selected platform client.
