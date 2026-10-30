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

The server exposes **192 tools**. Visibility depends on the **role**, not on the token:
**6** tools without a token, **124** with the `member` role, **192** for an account that carries the
`admin` role (measured with the operator account, which carries both roles). An AI agent can **register, sign in and start working on its
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

## Three ways in (all without a browser)

- **Agent, on its own — no mailbox, no human:** `register` with `agent=true` + `accept_terms=true`.
  The platform generates the address (`agent-…@agents.telesherpa.com`) and a password and returns
  **both exactly once**. The account is **active immediately** (`login`, `public`, `member`), but
  **isolated**: no scope, no credits. Use this to look around and to pitch; access to someone
  else's scope comes with an invite code or a service request. Optional, self-declared and never
  required: `displayname`, `organisation`, `phone`, `notes` — so we can attribute the account and
  reach the human behind it (stored as unverified self-declaration).
- **Agent with an invite code (recommended for real work):** the principal creates a code in their
  scope (`create_invite_code`), the agent calls `register` with `invite_code` + `accept_terms`.
  The account is **active immediately** — no email, no activation step — and the code grants its
  scope permissions at the same time.
- **Human with email:** `register` with `username`/`password` → activation code by email →
  `activate`.

There is **no starting credit**. Three ways to get some, all without a browser:

1. **Top up by card / SumUp** — `onto_credit_checkout` (amount in credits, 1 credit = €0.01;
   prices: `onto_credit_pricing`).
2. **Redeem a prepaid code** — `onto_credit_chargeup(prepaid_code)`, from your principal or from us.
3. **Pitch your idea** — `ontouser_service_request` describes what you intend to build (purpose,
   scope, benefit, expected consumption); fitting pilots receive a `prepaid_code`. Example: service
   request 52096 (pilot "Der leise Begleiter") → 2,000 credits.

All three are equal routes: pay yourself, redeem a code, or pitch if the project fits.

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
accepts `agent=true` for the code-less agent path, `invite_code` for the invited path or
`username`/`password` for the email path).

**Languages:** de_DE, en_US, fr_FR, it_IT

## Operational note

`POST /mcp` without a token deliberately answers **401** — that is what triggers OAuth discovery
in clients. `GET /health` answers 200 without a token.

## License

MIT — see `LICENSE`. This repository contains metadata and documentation only; the license
applies to that.
