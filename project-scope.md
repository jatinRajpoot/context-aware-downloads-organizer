# Context-aware Downloads Organizer - Project Scope

Build a local organizer that routes new browser downloads using understandable rules and a preview-first workflow. Start with browser-managed destinations; unrestricted filesystem cleanup belongs in a later desktop companion.

## Users and problem

Students, freelancers, job seekers, and office workers accumulate invoices, course files, resumes, and reports in a single Downloads folder. They need organization based on a file's purpose and source without maintaining complicated regular expressions.

## MVP workflow

1. Create a rule from a guided sentence such as "Vendor invoices go to Finance/2026".
2. Review the exact source-domain, filename, type, and destination conditions generated from that rule.
3. Preview where representative upcoming downloads would go and resolve competing rules.
4. Enable an approved rule and route future downloads to supported subfolders.
5. Inspect decisions in an audit log, disable rules, and review correction options.

## Included in the MVP

- Chrome/Edge extension for new downloads, with rules stored locally and an explicit dry-run default.
- Guided human-readable rule templates compiled into deterministic conditions; no paid language-model dependency.
- Source-domain, filename keyword, extension, and MIME-type matching where browser metadata supplies them.
- Destination paths relative to the browser's Downloads root, rule priority, conflict explanations, and unmatched-file fallback.
- Example-based rule previews and explicit approval before a rule affects future download destinations.
- Decision audit trail containing matched rule, proposed destination, browser-reported result, and errors.
- Collision-safe filenames, pause controls, and versioned rule export/import.
- Rule-edit undo; clearly explain that the browser-only release cannot undo arbitrary completed file moves.

## Deferred features

A Windows-first Tauri/Rust companion may later scan existing folders, inspect document contents, hash duplicates, move completed files, and implement reversible move journals. Arbitrary free-form language interpretation, automatic destructive cleanup, multi-folder watching, cloud classification, and paid licensing are outside the browser MVP.

## Proposed architecture and data

Use TypeScript, Manifest V3, the browser downloads API, local extension storage, and a small rule editor. Separate the rule compiler, metadata normalization, deterministic matcher, destination validator, audit store, and browser adapter. Unsupported metadata must produce an explanation instead of an invented classification.

Persist Rule, RuleCondition, RulePriority, DownloadDecision, and AuditEntry entities with schema versions. Keep preview evaluation and active evaluation on the same deterministic engine. A future companion adds FileRecord and MoveJournal with preflight checks, hashes where needed, collision handling, and rollback records.

## Safety, privacy, and cost

Validate destinations to reject absolute paths, parent traversal, and invalid names. Never overwrite existing files. Browser download behavior must continue normally if the organizer cannot classify an item; a missing companion must never block a download. Redact query strings and tokens from recorded source metadata. Never upload filenames, paths, or documents.

The browser release needs no server, account, paid inference, or desktop service. Later content inspection must remain local. Store fees and optional one-time licensing are separate release decisions.

## Acceptance criteria

- The same rule and metadata yield the same explainable decision in preview and active modes.
- Dry-run mode does not change download destinations; only explicitly enabled rules affect future downloads.
- Rule conflicts show the winning priority and let the user correct it before activation.
- Missing metadata, extension restart, or classification errors leave browser downloads functional.
- Path traversal and absolute-path test cases are rejected; collisions never overwrite a file.
- Interrupted or failed downloads are not presented as successfully organized.
- Rule export/import preserves order and behavior; invalid imports leave existing rules intact.
- The browser UI does not promise completed-file undo; a future desktop release must prove rollback before enabling moves.

## Delivery and validation

| Phase | Deliverable | Exit condition |
|---|---|---|
| 1 | Rule templates and dry-run engine | Users understand and approve predicted destinations. |
| 2 | Browser routing and audit log | Enabled rules route new downloads with safe fallbacks. |
| 3 | Pilot and companion decision | Classification acceptance justifies deeper filesystem work. |

Budget 3-5 solo-developer weeks for the browser MVP, with another 3-5 weeks estimated for a later companion. During research, run a separately consented read-only local scan on approximately 30 real Downloads folders; do not include broad scanning in browser permissions. Aim for at least 80% of proposed classifications accepted unchanged before expanding automation. A one-time Pro license for richer local rules or multi-folder workflows is a monetization hypothesis.

## Scope basis

This scope is derived from the supplied report, "Recent Verified User Pain Points Suitable for Zero-Backend SaaS, Mobile Apps, and Browser Extensions" (research window ending November 3, 2026). Market demand, estimates, and monetization are hypotheses to validate; the report's internal citation markers are not independently verified references. This repository starts with this scope document only; implementation has not started.
