# AGENTS.md

## Repo type
- Obsidian vault containing multilingual API/integration docs for a chargeback system. No code, no build/test/lint tooling.

## Structure
- `1.Enrichment/` — data enrichment flows (events sent to client, responses received via API)
- `2.Notifications/` — system notifications (status and cycle changes)
- `3.AI/` — AI agent input data spec
- `images/` — diagram PNGs referenced by docs
- `.obsidian/` — Obsidian workspace config (do not modify unless asked)

## Multilingual convention
- English is the primary language: `NAME.md`
- Spanish: `NAME.es.md`
- Portuguese: `NAME.pt-br.md`
- When editing or adding a doc, update all three language versions unless told otherwise.

## Key identifiers used across docs
- `contractDisputeId` — dispute contract ID
- `transactionIdentifier` — transaction ID
- `acquirerReferenceNumber` — acquirer reference
- `helpdeskCaseIdentifier` — helpdesk case ID
