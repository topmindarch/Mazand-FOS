# Mazand FOS — Project Specification (Agent Entry Point)

**Canonical project name:** Mazand FOS  
**Repository slug:** `mazand-fos` (recommended; verify actual GitHub repo before acting)  
**Business context:** Mazandasht citrus sorting, packaging, purchasing, selling, settlements and finance  
**Specification status:** approved product baseline; implementation status must be established by repository evidence.

## Product purpose

Build an installable, highly usable, Persian RTL, full-workspace operational and accounting system for citrus operations. The product must support local/offline work, optional shared LAN server mode, controlled inter-device synchronization, robust Excel/CSV interoperability, double-entry accounting, auditability, reports, printing, scale adapters, backup/restore and a polished visual dashboard.

## Non-negotiable decisions

1. Inspect the real repository, branch, code, data, docs and tests before modifying anything.
2. Read the listed legacy/core files if present, then inventory every relevant document. Report missing files as missing; never pretend they were read.
3. Preserve prior requirements and history. Use a mapping table before merging, renaming, superseding or deleting any document.
4. RTL Persian UI; full-height workspace; fixed shell and primary actions; internal scrolling for long tables; no vertically stacked wizard that forces whole-page scrolling.
5. Quick-create for party/account/product/basket opens in the same work context, preserves parent draft state and returns focus to the originating control.
6. Gross weight, tare, fruit net, contractual loss, true waste and billable weight are distinct data points. Do not double-deduct tare or loss.
7. A balanced posted journal is mandatory. Drafts may be incomplete; posted transactions cannot be silently edited.
8. Rial/toman switch is display-only with 10:1 scaling. Persisted amounts never change because a user toggles display unit.
9. A bank import is source evidence, not automatic proof of an accounting posting. Reconciliation and posting are distinct but linked workflows.
10. Inventory movements must be traceable. Do not pretend sales are mapped to specific lots if the process does not capture lot-level identity.
11. Independent-device offline mode and central LAN shared-database mode are distinct deployment modes. Avoid file-based multi-writer DB sharing over a network share; use an API/service for central multiuser access.
12. Backup must include exit-triggered and scheduled/protected-event snapshots, health verification and tested restoration; a ZIP that exists is not proof of recoverability.
13. Excel output must remain usable with Microsoft Office 2013: avoid newer-only dynamic-array functions, XLOOKUP/LET/FILTER dependencies and unsupported formula features unless an alternative is provided.
14. Every meaningful control needs Persian tooltip or contextual help. Help must be available offline and explain purpose, input, formula/impact, permission and common error where relevant.
15. Every change requires impact analysis, focused tests, cross-module regression, evidence and a report of limitations.

## Required initial read set

Read in this order when present:

1. `PROJECT_SPEC.md`
2. `docs/10-master-ai-coding-directive.md`
3. `docs/01-business-workflows.md`
4. `docs/02-data-model.md`
5. `docs/03-accounting-settlement-rules.md`
6. `docs/05-offline-windows7-architecture.md`
7. `docs/08-testing-acceptance.md`

Then inventory and read all relevant specs, workflows, schemas, migrations, tests, READMEs, design assets, build files and release instructions. These paths come from the earlier project baseline; if absent in the actual repository, create a gap item and map to current equivalents rather than fabricating their presence.

## Expected deliverables (documentation phase)

- repository map and baseline status
- document inventory with canonical/superseded/duplicate/missing labels
- requirement-to-file-to-code-to-test traceability matrix
- architecture decision record, data model and migration plan
- page/screen catalog, UI tokens and visual references
- financial/operational posting contracts and worked examples
- security, backup/restore, synchronization and import/export contracts
- phased implementation plan with acceptance gates

## Expected agent behavior

- No coding during read-only audit.
- No destructive change or main-branch push without explicit approval.
- No vague claims like “all tests passed” without commands, exit codes and test counts.
- No guessed technology, existing schema, branch, repository URL or scale protocol.
- Prefer evidence, small changes, testable vertical slices and rollback plans.
- Stop after every phase and report: summary, paths changed, decisions needed (only if necessary), tests, evidence, risks and next recommended prompt.

## Current scope order

Audit → document/requirement reconciliation → architecture/UX sign-off → approved implementation foundation → full-height shell/dashboard → master data → citrus purchase/sale weight workflow → inventory/sorting → accounting/ledger/period close → banking/barforoosh/checks → import/export → reports/print → security/backup → offline sync → scale adapter → controlled data migration → installer, restore test and production acceptance.
