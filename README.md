# Telesherpa Ontology Platform — MCP Server

**Telesherpa is an ontology platform (knowledge-graph platform) for facility management
(CAFM/IWMS, building management, corporate real estate) and field service.** It doubles as a
lightweight, no-code ERP/CRM: object types, forms, rules (triggers) and functions model business
processes without writing code.

**MCP endpoint:** `https://mcp.telesherpa.com/mcp` (Streamable HTTP, OAuth 2.1)
**Website:** https://www.telesherpa.com
**Health:** https://mcp.telesherpa.com/health (no token required)
**Documentation / agent skill:** https://codeberg.org/telesherpa/skill-telesherpa-com

> English is the primary language of this repository. Deutsche Fassung: [README.de.md](README.de.md).

> This repository is the **public carrier for directory listings** (Glama, the official MCP
> registry, and others). The server source code is not public. The agent skill is written and
> maintained on Codeberg — this repository points to it.

## What the server does

The server exposes **179 tools**. Visibility depends on the **role**, not on the token:
**6** tools without a token, **113** with the `member` role, **179** with the `admin` role,
**179** with both roles together. An AI agent can **register, sign in and start working on its
own** — zero-touch onboarding, no browser involved.

> The role was called `telesherpa` until 2026-10-11 and has been called `member` since — same
> role. The agent guide always states the current numbers:
> `codeberg.org/telesherpa/skill-telesherpa-com`.

Further use cases the platform covers:

- **Fleet management** — vehicles, drivers, inspection intervals (roadworthiness, UVV), service history
- **Field service management / service dispatch** — work orders, assignments, material consumption
- **CRM** — people, companies, contacts, relations between objects
- **Workflow automation / business process platform** — rules, triggers, scheduled functions
- **Multi-tenant SaaS / self-provisioning** — one scope per company, permissions, built-in credit
  and billing system, self-registration without a browser
- **Structured image management / digital asset management** — images in categories per scope
- **Maintenance** — maintenance plans, inspection intervals, recurring tasks

## Two ways in (both without a browser)

- **Agent with an invite code (recommended):** the principal creates a code in their scope
  (`create_invite_code`), the agent calls `register` with `invite_code` + `accept_terms`. The
  account is **active immediately** — no email, no activation step. The platform generates the
  address (`agent-…@agents.telesherpa.com`) and a password and returns **both exactly once**;
  the code grants its scope permissions at the same time.
- **Human with email:** `register` with `username`/`password` → activation code by email →
  `activate`.

There is **no starting credit**: credits come either from a prepaid code issued by the principal
(`onto_credit_chargeup`) or by card (`onto_credit_checkout`).

## Connecting

The server speaks Streamable HTTP and **supports OAuth 2.1 Dynamic Client Registration
(RFC 7591)**. Clients such as Claude, Cursor and other MCP clients register themselves — no
client ID has to be handed out.

```
Endpoint:      https://mcp.telesherpa.com/mcp
Discovery:     https://mcp.telesherpa.com/.well-known/oauth-authorization-server
Protected Res: https://mcp.telesherpa.com/.well-known/oauth-protected-resource
```

**Public tools (no token):** `register`, `activate`, `login`, `refresh_access_token`,
`auth_status`, `onto_credit_pricing` — these make agent self-registration possible (`register`
accepts either `invite_code` for the agent path or `username`/`password` for the email path).

**Languages:** de_DE, en_US, fr_FR, it_IT

## Operational note

`POST /mcp` without a token deliberately answers **401** — that is what triggers OAuth discovery
in clients. `GET /health` answers 200 without a token.

## License

MIT — see `LICENSE`. This repository contains metadata and documentation only; the license
applies to that.
