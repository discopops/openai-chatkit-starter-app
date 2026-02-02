# Custom ChatKit (advanced integration)

Use this for self-hosted ChatKit guidance.

Core steps:
- Install openai-chatkit (server package).
- Implement ChatKitServer.respond to stream events.
- Expose a /chatkit endpoint (FastAPI example).
- Implement Store and FileStore contracts (threads, messages, files).
- Use progress events for long-running tools.
- Use widgets and actions for interactive UI.

Source:
```
https://platform.openai.com/docs/guides/custom-chatkit
```
