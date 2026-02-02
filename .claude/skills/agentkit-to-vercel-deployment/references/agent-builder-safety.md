# Agent Builder safety (summary)

Use this to add security guidance in the skill.

Key risks:
- Prompt injection from untrusted inputs.
- Private data leakage via tool calls or context.

Mitigations:
- Do not place untrusted input in developer messages.
- Use structured outputs between nodes.
- Add guardrails for user inputs.
- Keep tool approvals enabled (use Human approval node).
- Use evals and trace grading to catch mistakes.

Source:
```
https://platform.openai.com/docs/guides/agent-builder-safety
```
