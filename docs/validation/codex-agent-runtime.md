# Codex Agent Runtime Validation

## Environment

- Codex CLI `0.150.1`;
- Windows 11 x86_64;
- 2026-08-30;
- native `codex exec --json` collaboration in a disposable non-Git laboratory.

## Result

| Capability | Result |
| --- | --- |
| Isolated Agent Runs | PASS |
| Nested `Copilot -> Lead -> Specialist` delegation | PASS |
| Per-run model selection | PASS |
| Per-run reasoning selection | PASS |
| Reclassification as a new run | PASS |
| Artifact-based continuity | PASS |

## Evidence Summary

The child run received only its explicit prompt and did not know a parent-only marker. A Lead created a nested Specialist that implemented `multiply(a, b)` and passed three deterministic tests. Separate runs accepted different models and reasoning efforts. A Specialist returning `NEEDS_RECLASSIFICATION` ended before a replacement run began with different configuration. Continuity was provided by a small durable handoff artifact, not private conversation.

## Limitations

The experiment used local CLI native collaboration, not Desktop UI presentation. Runtime records and rollout metadata used for corroboration are local and version-sensitive. PowerShell shell-snapshot limitations and missing inherited client metadata warnings did not prevent the tested behavior.

## Conclusion

`RUNTIME_COMPATIBLE_WITH_LIMITATIONS`
