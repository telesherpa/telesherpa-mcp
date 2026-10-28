# Telesherpa Ontology Platform — MCP Server

> Deutsche Fassung. English version: [README.md](README.md).

**Telesherpa ist eine Ontologie-Plattform (Knowledge-Graph-Plattform) für Facility Management
(CAFM/IWMS, Gebäudemanagement, Corporate Real Estate Management) und Field Service.** Sie ist
zugleich als leichtgewichtiges, No-Code-ERP/CRM nutzbar: Objekttypen, Formulare, Regeln (Trigger)
und Functions bilden Geschäftsprozesse ab, ohne Code zu schreiben.

**MCP-Endpoint:** `https://mcp.telesherpa.com/mcp` (Streamable HTTP, OAuth 2.1)
**Website:** https://www.telesherpa.com
**Health:** https://mcp.telesherpa.com/health (ohne Token)
**Dokumentation / Agent-Skill:** https://codeberg.org/telesherpa/skill-telesherpa-com

> Dieses Repository ist der **Öffentlichkeits-Träger für die Verzeichnisse** (Glama, offizielle
> MCP-Registry). Der Quellcode des Servers liegt nicht öffentlich. Inhalt und Pflege des
> Agent-Skills liegen auf Codeberg — dieses Repository verweist darauf.

## Was der Server kann

Der Server stellt **177 Tools** bereit. Die Sichtbarkeit hängt an der **Rolle**, nicht
am Token: **6** Tools ohne Token, **110** mit der Rolle `member`, **177** mit der Rolle
`admin`, **177** mit beiden Rollen. Ein KI-Agent kann sich **selbstständig registrieren,
anmelden und arbeiten** — Zero-Touch-Onboarding ohne Browser.

> Die Rolle hieß bis 2026-10-11 `telesherpa` und heißt seither `member` — gemeint ist dieselbe
> Rolle. Die Agenten-Anleitung nennt die jeweils aktuellen Zahlen:
> `codeberg.org/telesherpa/skill-telesherpa-com`.

Weitere Einsatzfelder, die die Plattform abdeckt:

- **Fleet-/Flottenmanagement** — Fahrzeuge, Fahrer, Prüffristen (HU, UVV), Wartungshistorie
- **Field Service Management / Service Dispatch** — Aufträge, Einsätze, Materialverbrauch
- **CRM** — Personen, Firmen, Kontakte, Beziehungen zwischen Objekten
- **Workflow-Automation / Business Process Platform** — Regeln, Trigger, zeitgesteuerte Funktionen
- **Multi-Tenant SaaS / Self-Provisioning** — Scopes je Firma, Rechte, eingebautes Credit- und
  Abrechnungssystem, Selbstregistrierung ohne Browser
- **Structured Image Management / Digital Asset Management** — Bilder in Kategorien je Scope
- **Maintenance / Instandhaltung** — Wartungspläne, Prüffristen, wiederkehrende Aufgaben

## Zwei Wege zum Zugang (beide ohne Browser)

- **Agent mit Einladungscode (empfohlen):** Der Auftraggeber legt in seinem Scope einen Code an
  (`create_invite_code`), der Agent ruft `register` mit `invite_code` + `accept_terms` auf. Das
  Konto ist **sofort aktiv** — keine Mail, keine Aktivierung. Die Plattform erzeugt Adresse
  (`agent-…@agents.telesherpa.com`) und Passwort und gibt **beide einmalig** zurück; der Code
  vergibt beim Anlegen gleich seine Scope-Rechte.
- **Mensch mit E-Mail:** `register` mit `username`/`password` → Aktivierungscode per Mail →
  `activate`.

Ein **Startguthaben gibt es nicht**: Credits kommen über einen prepaid-Code des Auftraggebers
(`onto_credit_chargeup`) oder per Karte (`onto_credit_checkout`).

## Verbinden

Der Server spricht Streamable HTTP und **unterstützt OAuth 2.1 Dynamic Client Registration
(RFC 7591)**. Clients wie Claude, Cursor und andere MCP-Clients registrieren sich selbst —
es muss keine Client-ID verteilt werden.

```
Endpoint:      https://mcp.telesherpa.com/mcp
Discovery:     https://mcp.telesherpa.com/.well-known/oauth-authorization-server
Protected Res: https://mcp.telesherpa.com/.well-known/oauth-protected-resource
```

**Öffentliche Tools (ohne Token):** `register`, `activate`, `login`, `refresh_access_token`,
`auth_status`, `onto_credit_pricing` — damit ist Selbstregistrierung durch einen Agenten möglich
(`register` nimmt entweder `invite_code` für den Agenten-Weg oder `username`/`password` für den
E-Mail-Weg).

**Sprachen:** de_DE, en_US, fr_FR, it_IT

## Betriebshinweis

`POST /mcp` ohne Token antwortet bewusst mit **401** — das ist der Auslöser für die
OAuth-Discovery der Clients. `GET /health` antwortet ohne Token mit 200.

## Lizenz

MIT — siehe `LICENSE`. Dieses Repository enthält nur Metadaten und Dokumentation;
die Lizenz bezieht sich darauf.
