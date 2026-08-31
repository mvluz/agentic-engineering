# GitHub MCP Projects Write Validation

## Environment

- official `github/github-mcp-server` `v1.11.0`;
- STDIO transport with OAuth;
- 2026-08-30;
- authorized disposable repository `mvluz/agentic-engineering-mcp-lab`;
- disposable GitHub Project `Agentic Engineering MCP Validation - DELETE ME`.

## Result

| Capability | Result |
| --- | --- |
| Create Project and Issue | PASS |
| Add and remove Issue Project items | PASS |
| Read Project, fields, and items | PASS |
| Update existing Status | PASS |
| Create and read native parent/sub-issue hierarchy | PASS |
| Create custom fields and options | UNSUPPORTED |
| Assign configured `Work Type`, `Priority`, and `Status` | PASS |
| Reconstruct configured field values | PASS |

The MCP created disposable Epic, Feature, User Story, and Task Issues; added them to the disposable Project; set native Status through `Todo`, `In Progress`, and `Done`; reconstructed that state through MCP reads; and created and read native parent/sub-issue relationships.

After one-time Human/manual field setup, a disposable Issue was assigned `Work Type = Task`, `Priority = P1`, and `Status = Ready`. MCP reads reconstructed all three values. Its Status was then changed to `In Progress` and read back successfully.

## Limitation

The exposed MCP could not create arbitrary custom Project fields or their single-select options. The required `Work Type`, `Priority`, and lifecycle Status options therefore require one-time Human/manual Project setup. Assignment and reconstruction of those configured fields were subsequently validated.

## Cleanup

All disposable Issue items were removed from the Project and all disposable Issues, including the final field-validation Issue, were closed. The empty disposable Project remained because the exposed toolset had no Project deletion operation. No real GitHub work was modified.

## Conclusion

`GITHUB_WORKFLOW_MCP_READY`
