# Commons AI Gateway

The gateway is the first implementation target and the shared backend for every
client. Begin as a modular monolith: authentication, model registry, routing,
policy, usage, and telemetry live here until independent deployment or ownership
requirements justify extracting a service.

The first vertical slice exposes `/v1/models` and `/v1/chat/completions`, uses
development authentication, resolves one stable model alias, and records basic
request telemetry.
