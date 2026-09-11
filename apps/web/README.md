# Commons AI Web

Implementation-neutral home of the Commons AI browser experience.

## Selected bootstrap

The preferred initial scaffold is the BSD-3-Clause Open WebUI v0.6.5 source,
subject to the legal, provenance, security, accessibility, and maintenance gates
in ADR-0009. Do not import source until the exact tag and immutable commit are
verified and recorded.

Current Open WebUI releases are not an upstream code source. Do not copy, merge,
or cherry-pick post-v0.6.5 code or assets without a new license review and ADR.

Complete [UPSTREAM_PROVENANCE.md](./UPSTREAM_PROVENANCE.md) before importing any
third-party source.

## Boundary

This application talks only to the Commons AI Fabric gateway's documented OpenAI-compatible
and native AC APIs. It does not integrate directly with model runtimes, Entra,
Brightspace, databases, storage, or Commons Compute Fabric.

## Intended evolution

The product grows from the initial landing and chat scaffold into the native
Chat, Study, Work, Code, and Campus experience. Generic inherited components stay
only while they remain secure, accessible, maintainable, and useful.

See the umbrella documents:

- [ADR-0009: Commons AI Web foundation](../../../algonquin-architecture/docs/architecture/architecture-decision-records/ADR-0009-psdc-ai-web-foundation.md)
- [Commons AI Web Foundation](../../../algonquin-architecture/docs/clients/Commons-AI-Web-Foundation.md)
