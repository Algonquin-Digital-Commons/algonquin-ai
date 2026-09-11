# Ecosystem Integration

- Commons Cloud Fabric provides shared identity, events, edge, observability, and platform
  services.
- Commons Compute Fabric becomes a routable compute backend only at the documented Phase 9 readiness
  gate; repositories remain separate and integrate through versioned contracts.
- Commons Media and Spatial Fabric owns generated and transformed media assets, provenance, and
  delivery.
- Commons Social Fabric owns ActivityPub publication, remote federation, and social-product
  moderation.
- Shared spatial contracts allow AI responses, campus knowledge, media scenes, and
  federated objects to refer to places without sharing precise private location by
  default.
- The Commons core is institution-neutral. Algonquin configuration stays in a
  deployment overlay, and cross-institution routing follows the workload envelope
  and institution-first locality ladder.

Commons AI Fabric integrates through authenticated APIs and versioned events. It must not
couple model routing or agents to another repository's implementation details.

OpenWork desktop and Happy mobile are replaceable presentation and interaction
layers. Both use the shared Agent Session Contract; only the Session Host may
mediate local workspace tools, the Session Relay transports E2EE ciphertext, and
all inference routes through the Commons AI Gateway. Neither OpenWork Den nor a hosted
Happy service is part of an ecosystem dependency.
