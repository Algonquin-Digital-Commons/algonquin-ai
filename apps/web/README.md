# Web Client Migration Notice

The Algonquin browser client is owned by `algonquin-web`, the thin institution
fork of `Post-Secondary-Digital-Commons/psdc-web`. This directory contains no web
source and SHALL NOT receive new browser-client implementation.

`algonquin-ai` owns the institution-configured AI gateway, routing, policy
integration, inference adapters, evaluation, and service contracts consumed by
clients. It does not own the browser product lifecycle. Clients use documented
APIs and never call model runtimes, identity directories, Brightspace, databases,
storage backends, or worker agents directly.

Canonical references:

- [Algonquin Web](https://github.com/Algonquin-Digital-Commons/algonquin-web)
- [PSDC Web upstream](https://github.com/Post-Secondary-Digital-Commons/psdc-web)
- [ADR-0025: Independent Web Client Repository](https://github.com/Algonquin-Digital-Commons/algonquin-architecture/blob/main/docs/architecture/architecture-decision-records/ADR-0025-independent-web-client-repository.md)
- [Algonquin Web deployment profile](https://github.com/Algonquin-Digital-Commons/algonquin-web/blob/main/docs/Algonquin-Web-Deployment-Profile.md)

The eligible Open WebUI v0.6.5 bootstrap and its provenance gate are owned by
`psdc-web`. Current Open WebUI releases are not an authorized source import.
