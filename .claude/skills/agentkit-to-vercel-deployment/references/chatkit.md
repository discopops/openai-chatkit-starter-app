# ChatKit (OpenAI) quick reference

Use when embedding ChatKit UI or wiring API URLs.

Key points:
- ChatKit UI loads in the browser and requires domain allowlist.
- Domain allowlist uses a domain key (domain_pk_...).
- Frontend must set VITE_CHATKIT_API_DOMAIN_KEY.
- The UI will show a blue overlay if domain is not verified.

Sources:
```
https://platform.openai.com/docs/chatkit
https://platform.openai.com/settings/organization/security/domain-allowlist
```
