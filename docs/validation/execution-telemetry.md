# Codex Execution Telemetry Validation

## Environment

- Codex CLI `0.150.1`;
- Windows 11 x86_64;
- 2026-08-30;
- native `codex exec --json` with collaboration in a disposable non-Git laboratory.

## Result

Four distinct Agent Runs completed: Root/Copilot, Lead, Specialist A, and Specialist B. The runtime exposed identities, parent relationships, actual model and reasoning metadata, completion records, and final cumulative token counters for each run.

| Finding | Result |
| --- | --- |
| Per-Agent-Run measurement | PASS |
| Parent / child relationship reconstruction | PASS |
| Child usage excluded from parent total | PASS |
| Reclassification measured as distinct runs | PASS |
| Workflow Execution aggregation | PASS |
| Telemetry quality | `FULL` |

## Accounting Evidence

The tested Workflow Execution total was `208,701` tokens: `56,772 + 95,563 + 18,715 + 37,651`. The valid rule was to sum the final cumulative total once for each distinct run. Summing intermediate token events would double count, and adding cached-input or reasoning-output fields separately would be incorrect because they are already represented in observed totals.

## Limitations

Parent relationships and detailed token records came from local rollout/session JSONL, an internal version-sensitive runtime format. A future collector must check the current Codex version and fail visibly when expected fields or semantics change.

## Conclusion

`THIN_TOOLING_RECOMMENDED`

The runtime provides the required measurements; deterministic extraction and deduplication remain implementation work.
