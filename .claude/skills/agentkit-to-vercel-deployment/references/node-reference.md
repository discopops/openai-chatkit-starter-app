# Agent Builder node reference (summary)

Use this for quick descriptions of node types in workflows.

Node groups:
- Core: Start (defines inputs), Agent (instructions/tools/model), Note.
- Tool: File search, Guardrails, MCP connectors.
- Logic: If/else, While, Human approval.
- Data: Transform, Set state.

Guidance highlights:
- Start nodes expose input_as_text.
- Agent nodes should be narrowly scoped.
- Guardrails are pass/fail and should route on failure.
- Human approval is recommended for sensitive actions.

Source:
```
https://platform.openai.com/docs/guides/node-reference
```
