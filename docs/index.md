# mMoveR Governance Docs

This site contains the public governance documentation for the **mMoveR Governance Copilot** agent.

This documentation is designed to support:
- governance-first requirement handling
- REQ Dashboard continuity
- RTG → BA → ARCH → DEV → QA → GOV workflow execution
- public website grounding for the agent

---

## Core Governance

- [Workflow](./workflow)
- [Handover](./handover)
- [REQ Register](./req-register)
- [REQ State](./req-state)
- [Quality Gates](./quality-gates)
- [Code Quality Checklist](./code-quality-checklist)
- [Code Generation Rules](./code-generation-rules)
- [Logging Contract](./logging-contract)
- [Requirement Matching Policy](./requirement-matching-policy)
- [Team Roster](./team-roster)
- [Context Primer](./context-primer)

---

## Agent Governance

- [Agent Governance](./agent-governance)
- [Agent Build Path](./agent-build-path)
- [Agent Instructions Policy](./agent-instructions-policy)
- [Agent Knowledge Policy](./agent-knowledge-policy)
- [Agent Publish Checklist](./agent-publish-checklist)
- [Agent Share and Access Policy](./agent-share-and-access-policy)

---

## Supporting Governance

- [Decisions](./decisions)
- [Decisions Append Only](./decisions-append-only)
- [Documentation Formatting Rules](./doc-formatting-rules)
- [REQ ID Rules](./req-id-rules)
- [REQ Register Fields](./req-register-fields)
- [New Chat Bootstrap](./new-chat-bootstrap)
- [New Requirement Intake](./new-requirement-intake)
- [Canonical Delivery Policy](./canonical-delivery-policy)
- [Prompt Pack Template](./prompt-pack-template)

---

## Active Requirement Pages

- [REQ-002 Intake](./req-002-intake)
- [REQ-002 Prompt Pack](./req-002-prompt-pack)
- [REQ-002 Closure Note](./req-002-closure-note)
- [REQ-002 Artifact Index](./req-002-artifact-index)

---

## Archived / Historical Requirement Pages

- [REQ-001 Intake](./req-001-intake)
- [REQ-001 Closure Note](./req-001-closure-note)
- [REQ-001 Artifact Index](./req-001-artifact-index)

---

## Usage Notes

- Use the **REQ State** page as the continuity source for the REQ Dashboard when it is available.
- Use the **REQ Register** page as the primary requirement-state register.
- Use the **Handover** page as the resume-context source.
- If a new requirement is introduced, update the REQ state content and republish the site.

---

## Recommended Agent Builder Public Website URLs

If you are adding only a few URLs to Agent Builder, start with:

- `https://ananthkum.github.io/mmover-governance/`
- `https://ananthkum.github.io/mmover-governance/workflow`
- `https://ananthkum.github.io/mmover-governance/req-register`
- `https://ananthkum.github.io/mmover-governance/req-state`

---

## Maintenance Rule

Keep filenames:
- lowercase
- hyphen-separated
- flat under `docs/`

Example:
- `workflow.md`
- `req-register.md`
- `req-state.md`

Avoid uppercase names and deep nested public page paths.
