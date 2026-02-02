# ChatKit advanced samples (UI tailoring patterns)

Use this when customizing the ChatKit UI and looking for concrete examples.

## What the repo shows

- Scenario-driven demos; each example pairs a FastAPI backend with a Vite + React frontend.
- Examples include Cat Lounge, Customer Support, News Guide, and Metro Map.

## Feature index highlights (from README)

- Server tool calls to retrieve app data before inference.
- Client tool calls that read/mutate UI state (e.g., map selection).
- Client effects to sync UI state from server actions.
- Page-aware responses via custom headers (e.g., current article id).
- Progress updates streamed for long-running lookups.
- Response lifecycle hooks to lock/unlock UI during streaming.
- Widgets with and without actions; server- and client-handled actions.
- Annotations rendered as entity sources (e.g., route stations).
- Thread titles set by a title agent on first turn.
- Entity tags (@-mentions) wired to lookup and preview handlers.
## Where to look in the repo

- `examples/` for full app patterns.
- Example ChatKitPanel implementations for effects, widgets, and lifecycle hooks.

Source:
```
https://github.com/openai/openai-chatkit-advanced-samples
```
## Additional patterns worth copying

- Server-handled widget actions for inline updates without another model call.
- Annotations with entity sources tied to UI interactions.
- Composer tool-choice menu to force a specific agent/tool.
- Custom header actions in the chat header (e.g., theme toggle).
