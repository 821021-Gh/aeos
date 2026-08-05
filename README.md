# AEOS

AI Enterprise Operating System.

## Repository Overview

AEOS is the Enterprise Root Repository for the AI Engineering Workspace. It is the formal home of the workspace's enterprise-level documentation and governance, covering Enterprise Architecture, Platform Governance, and Capability Management.

## Repository Purpose

AEOS provides a single, formal repository where the AI Engineering Workspace's enterprise architecture baseline, platform governance, and capability management are documented and maintained.

## Repository Responsibilities

- Maintain the formal documentation baseline of the AI Engineering Workspace.
- Provide the entry point for architecture and governance artifacts (see Referenced Documents).
- Keep repository foundation and architecture content decoupled.
- Deliver production-ready formal documents without placeholders.

## Relationship to AI Engineering Workspace

AEOS is the Enterprise Root Repository of the AI Engineering Workspace: it represents the workspace at the enterprise level and is the formal entry point for its architecture and governance documentation. AEOS does not implement engineering workflows itself; engineering delivery follows the YEOS Engineering Workflow (see CONTRIBUTING.md).

## Repository Principles

- Documentation First — formal documents are the primary deliverables.
- Specification Driven — changes are defined by specifications and work orders before implementation.
- Single Source of Truth — repository content traces to approved sources without redefining them.
- Decoupled Foundation — repository foundation and architecture are managed independently.
- Production Ready — delivered documents are complete and formal; no placeholders.

## Repository Status

| Area | Status |
|------|--------|
| Current Phase | Foundation |
| Version | 1.0.0 |

## Repository Structure

- `README.md` — Repository entry point (Overview, Purpose, Status, Structure, Workflow)
- `docs/` — Formal documents (architecture, governance, capability, organized by domain)
- `engineering/` — Engineering workflow and Engineering Work Orders (EWO)
- `templates/` — Document and work order templates
- `assets/` — Static assets

## Development Workflow

- Each change is defined by an Engineering Work Order (EWO); one EWO is completed at a time.
- Implementation happens on a feature branch; delivery is submitted as a Draft Pull Request.
- Engineering delivery follows the YEOS Engineering Workflow (see CONTRIBUTING.md).

## Referenced Documents

| Document | Type | Role |
|----------|------|------|
| [AEOS-ARCH-001 — Architecture Baseline](docs/architecture/AEOS-ARCH-001-Architecture-Baseline.md) | Architecture Entry Document | Architecture baseline and entry point |
| WA-001 — AI Engineering Workspace Architecture (Approved v1.0.0) | Architecture Source | Approved architecture source |
