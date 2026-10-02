# Complete distribution candidate

All clients are intended to use one shared remote Complete MCP backend at the proposed production endpoint `https://mcp.completelabs.com/mcp/complete`. This endpoint is not verified live; deployment and host acceptance remain pending. Version 0.1.0 is a preparatory review candidate; integrations are not live or review-approved. No backend source or credentials are included.

The root Agent Plugins manifest targets shared ChatGPT/Codex packaging. The root Gemini CLI extension uses Streamable HTTP. Client examples are under `clients/`; Codex is disabled until the release gates pass. Claude supports the remote connector route. Gemini and Grok custom connectors can use the same URL after provider authorization is implemented and verified. Muse refers to the Meta Muse Connector Platform; its adapter and submission requirements remain pending verification.

Do not install or publish this candidate as a ready service. OAuth, host acceptance, legal URLs, listing assets, publisher verification, and data policy review must be completed. Preparatory package publication is separate from service launch or platform acceptance.

Antigravity users have a separate adapter under `adapters/antigravity/`, using its required `serverUrl` field and disabled by default. Install only that adapter directory after release approval. `assets/logo-square-draft.png` is a padded draft of the existing logo, without redesign.

The public plugin/configuration files and their accompanying package instructions listed in [license-scope.json](license-scope.json) are licensed under the standard [MIT License](LICENSE), copyright 2026 Complete Laboratories, Inc. All four PNG brand assets are excluded and reserved under [assets/NOTICE.md](assets/NOTICE.md). This license covers only the listed distribution files; it does not license the proprietary backend, hosted service, service or user data, company trademarks, or brand assets, and grants no service access. Files added later require an explicit scope decision.

Complete requires OAuth user authentication. In the OpenAI connection drawer select OAuth and complete the approved client setup after domain and server discovery are available; a default No Auth setting is not permission to expose athlete data. The portable MCP schema declares transport and URL, while MCP runtime authorization uses server discovery and token validation. No credentials or unsupported authentication fields are packaged.
