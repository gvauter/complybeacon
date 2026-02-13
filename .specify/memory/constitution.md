<!--
Sync Impact Report (2026-02-11):
- Version change: (none) → 1.0.0 (initial ratification)
- Modified principles: N/A (all new)
- Added sections: Technology and Standards, Development Workflow
- Removed sections: None
- Templates requiring updates: ✅ constitution (this file). No changes required to
  plan-template, spec-template, tasks-template for this initial ratification.
- Follow-up TODOs: None. RATIFICATION_DATE set to first adoption date.
-->

# ComplyBeacon Constitution

## Core Principles

### I. API-First (OpenAPI)

All HTTP APIs in the project MUST be defined by OpenAPI specifications. The Compass
API is the canonical example: `api.yaml` at the repository root defines the contract;
server and client code MUST be generated from the spec where applicable (e.g.
oapi-codegen). New endpoints or services MUST add or extend the OpenAPI definition
before implementation. Rationale: contract-first design ensures interoperability,
clear documentation, and consistent validation across consumers and producers.

### II. Modularity and Composability

ComplyBeacon is a toolkit of distinct components (ProofWatch, Beacon, TruthBeam,
Compass). Each component MUST live in its own Go module with clear boundaries.
Components are not required to be used together; users MAY compose their own
pipelines. Dependencies between components MUST be explicit and minimal (e.g.
TruthBeam calls Compass via HTTP; no shared internal packages unless justified).
Rationale: modularity and composability are explicit design goals in DESIGN.md and
support flexible deployment and independent evolution.

### III. Test Coverage for New Behavior

New features and API changes MUST be accompanied by tests. Unit tests for logic and
handlers; integration or contract tests for new or changed HTTP endpoints and
cross-component behavior. Running `make test` MUST pass before merge. Rationale:
the project is under active development; tests protect refactors and document
expected behavior.

### IV. OpenTelemetry and Semantic Conventions

Compliance evidence and enrichment data MUST align with OpenTelemetry and the
project’s semantic conventions. Attribute and entity definitions live under
`model/` (e.g. `attributes.yaml`, `entities.yaml`). Changes to conventions MUST
be validated with `make weaver-check` and propagated via `make weaver-docsgen` and
`make weaver-codegen`. Rationale: standardization ensures broad compatibility and
consistent enrichment across the pipeline.

### V. Simplicity and YAGNI

Prefer the simplest design that meets requirements. Avoid adding infrastructure,
dependencies, or features “for later” unless there is a documented, near-term need.
The toolkit is not yet production-ready; complexity MUST be justified. Rationale:
reduces maintenance burden and keeps the codebase understandable as the project
matures.

## Technology and Standards

- **Language and runtime:** Go 1.24+ (toolchain as specified in each module’s
  `go.mod`). Compass, ProofWatch, and TruthBeam are Go modules.
- **API contract:** OpenAPI 3.x; Compass API is defined in the repository root
  `api.yaml`. Generated code (e.g. server stubs, types) MUST not be hand-edited
  where codegen is in use; regenerate after spec changes.
- **Observability:** OpenTelemetry for logs and metrics; semantic conventions in
  `model/` are the source of truth for attribute and entity definitions.
- **Containers and orchestration:** Containerfiles and compose are used for demo
  and deployment; optional AWS S3 and other exporters are configurable and MUST
  not block local or minimal deployments.

## Development Workflow

- **Build and test:** `make test` and `make build` MUST succeed. Use `make workspace`
  for a Go workspace that includes compass, proofwatch, and truthbeam.
- **Documentation:** Design and architecture live in `docs/DESIGN.md`; practical
  setup and development in `docs/DEVELOPMENT.md`. New features that affect behavior
  or contracts SHOULD update the relevant docs.
- **Code generation:** Follow the steps in DEVELOPMENT.md for API codegen (Compass)
  and OpenTelemetry semantic convention updates (`weaver-check`, `weaver-docsgen`,
  `weaver-codegen`). Generated files MUST be committed when they are part of the
  build.

## Governance

This constitution is the project’s governing set of principles. All contributions
and design decisions SHOULD align with it. Amendments require updating this file,
bumping the version and Last Amended date, and documenting the change (e.g. in a
PR or commit message). When in doubt, prefer the principle that favors clarity,
testability, and interoperability. Complexity or exceptions MUST be justified and
documented.

**Version**: 1.0.0 | **Ratified**: 2026-02-11 | **Last Amended**: 2026-02-11
