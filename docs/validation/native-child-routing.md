# Native Child Model and Reasoning Routing Validation

## Environment

- Codex CLI `0.150.1`;
- Windows 11 x86_64;
- 2026-08-30;
- native `codex exec --json` collaboration in a disposable non-Git laboratory.

## Result

| Test | Requested Child | Actual Child | Result |
| --- | --- | --- | --- |
| Model override | `gpt-5.6-luna` / `low` | `gpt-5.6-luna` / `low` | PASS |
| Reasoning override | `gpt-5.6-terra` / `high` | `gpt-5.6-terra` / `high` | PASS |

Native `spawn_agent` exposed `model` and `reasoning_effort`. Child rollout records corroborated the requested model and reasoning values. The tested control direction was Root/Copilot -> Lead -> Specialist.

## Conclusion

`DIRECT_NATIVE_ROUTING`

`ROUTING_COMPATIBLE`

The Lead can directly configure a Specialist's model and reasoning effort without a configured role or external routing adapter.

## Limitations

Argument names, child configuration records, and parent/child links are current CLI behavior and use version-sensitive local runtime data. Actual configuration must be verified again when the Codex runtime changes. This validation did not implement routing tooling.
