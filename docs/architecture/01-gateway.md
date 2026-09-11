# The Gateway

The gateway is the first service to build and the one nothing else can be
meaningfully prototyped without. Everything else — web-client theming, mobile
apps, Brightspace integration — can wait. The gateway can't.

## Responsibilities

- Authentication (who is this)
- Authorization / RBAC (what are they allowed to do)
- Quotas and rate limits (how much can they use)
- Model permissions (which aliases can this role reach)
- Resource and cost accounting
- Routing among self-hosted campus, Commons Compute Fabric, and user-owned edge capacity
- Logging / audit trail
- Model health and failover
- Service discovery for downstream inference backends

## Routing policy

Every request is tagged with one of three policies before it reaches the model
router:

```text
CAMPUS_ONLY       — must stay on institution-controlled infrastructure
SELF_HOSTED       — may use approved Commons Compute Fabric or partner-operated open infrastructure
USER_EDGE_ALLOWED — may use a user-controlled local runtime
```

Routing then considers: model availability, GPU utilization, queue depth,
expected latency, context length, required capabilities, data classification,
user role, course/research policy, resource budget, and runtime availability.
This must enforce the institution's responsible-use rules (no personal,
health, payment-card, or Confidential/Restricted data into AI tools) rather
than assuming "on-prem = anything goes."

## Model aliases, not model names

Don't expose vendor model names as the primary abstraction — they change too
often and every rename breaks student projects that hardcoded a model id.

```text
AC Fast
AC General
AC Reasoning
AC Code
AC Research
AC Vision
AC Private
```

Behind each alias sits the model router, which resolves it to whatever the
currently-approved backing model is. Advanced users can still drill into
`Local Models`, `Research Models`, and approved experimental models if they want.

## MVP scope for this service

Build only what's needed for the first vertical slice: dev auth, one model
alias, `/v1/chat/completions`, `/v1/models`, and basic request logging. Quotas,
resource accounting and placement policy can come
once there's a second backend to route between.
