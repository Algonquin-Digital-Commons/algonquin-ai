# AC Session Host

The Session Host runs beside an authorized workspace and agent engine. It is the
authority for session lifecycle, workspace grants, tool execution, event order,
permission enforcement, and device control leases.

It adapts OpenCode first, and later other engines, to the shared Agent Session
Contract. Agent-specific protocols never become the public platform contract.
Inference and model credentials flow through the Commons AI Gateway.

Initial implementation gates: schemas and fixtures, loopback transport, explicit
workspace grants, idempotent command handling, permission receipts, cursor resume,
handoff races, audit events, and safe host-disconnect behavior.

