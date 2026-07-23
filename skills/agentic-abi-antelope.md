---
name: agentic-abi-antelope
description: Interpret, explain, validate, or enrich an Antelope smart-contract ABI with ReLocke-compatible Ricardian documentation. Use when an agent needs to gather contract context, assign one or more contract types, create or update ui.contract metadata, document actions or tables, describe externally triggered behavior, or prepare an ABI for discovery and integration by humans and agentic pipelines.
---

# Agentic Antelope ABI

Use this skill to make a standard Antelope ABI understandable to humans, ReLocke, search and discovery systems, and other agents without changing the contract's executable interface.

## Outcomes

An enriched ABI should help a user or agent:

- understand what the contract is and who should use it;
- discover the contract by one or more substantial types;
- understand the intent, authorization, state changes, side effects, and failures of actions;
- understand table meaning, scope, units, lifecycle, and relationships;
- identify actions on other contracts that trigger documented behavior;
- construct safer integration and agentic workflows; and
- improve structured discovery across ReLocke, the Antelope ecosystem, and search engines.

Enrichment is documentation, not authority. Standard ABI fields, deployed code, permissions, live chain state, transaction validation, and explicit user intent remain authoritative.

## ReLocke convention

Use `relocke.ui/1`. Clause IDs and frontmatter keys are case-sensitive. Preserve unknown metadata and unrelated ABI content unless the user explicitly requests its removal.

The overview format is:

```yaml
---
schema: relocke.ui/1
types: token,bridge
title: Example bridged token
---
Issues and manages a fungible token and supports transfers between documented networks.
```

Use the overview as the body of exactly one `ricardian_clauses` entry with the ID `ui.contract`.

The obsolete singular `type` and `categories` fields are not part of this convention. Use only `types`.

## Context required before interpreting or updating an ABI

Establish or request:

1. The exact chain and contract account.
2. The complete current ABI and its ABI version.
3. The contract's verified purpose and intended users.
4. Every substantial capability implemented by the contract.
5. Assets and external contracts it issues, reads, holds, or transfers.
6. Actions, required authorizations, preconditions, state changes, inline actions, and failure conditions.
7. Tables, scopes, units, row lifecycles, indexes, and relationships.
8. Notification handlers or other external actions that cause contract behavior.
9. Existing Ricardian clauses and custom metadata that must be preserved.
10. Available source code, deployed-code verification, and live state needed to validate documentation claims.

Do not infer missing facts from contract, action, table, or field names alone. Clearly distinguish:

- structural facts verified from the ABI;
- behavior verified from source or live state;
- contract-authored documentation claims; and
- context supplied by the user.

## Assigning contract types

`types` is an unordered, comma-separated set of lowercase kebab-case values:

```yaml
types: token,bridge
```

Assign every type that independently describes a substantial behavior implemented by the contract. The first value is not primary and has no additional authority.

Do not assign a type merely because the contract calls, reads, accepts an asset from, or depends on another contract of that type. For example, a token contract that reads an external oracle is not automatically an `oracle`.

Use a predefined type when it accurately describes the behavior. When none is suitable, create a precise lowercase kebab-case type such as `insurance-pool` or `prediction-market`. Avoid synonyms and unnecessary composite values. Prefer `token,bridge` over `token-bridge` when the independent types accurately express the behavior.

### Multiple-type examples

```yaml
types: token,bridge
```

Use when the same contract manages fungible supply or balances and implements cross-network transfers. Use only `bridge` when it moves assets issued and managed by other contracts.

```yaml
types: dao,governance,treasury
```

Use when the contract substantially operates an organization, implements collective decisions, and controls pooled assets.

```yaml
types: marketplace,nft
```

Use when the contract both runs a market and issues or manages NFTs. A marketplace that only trades NFTs issued elsewhere may use only `marketplace`.

```yaml
types: payments,account-creation
```

Use when documented payments or deposits are a substantial interface that creates or funds accounts.

## Predefined contract types

### `account-creation`

Creates, provisions, funds, or configures blockchain accounts. Do not assign it merely because ordinary actions require an existing account.

### `bridge`

Transfers assets or messages between chains, networks, or trust domains. Combine with `token` only when the same contract also issues or manages the fungible asset.

### `dao`

Operates a decentralized organization, including organizational membership, proposals, working groups, or coordinated activity. Add `governance` or `treasury` only when those are also substantial interfaces.

### `exchange`

Provides swaps, trading, order matching, liquidity markets, price execution, or other asset-exchange mechanisms. A simple payment transfer is not an exchange.

### `game`

Implements game rules, player state, progression, outcomes, or an on-chain game economy. Do not assign it solely because an external game uses the contract's asset.

### `governance`

Manages proposals, voting, approvals, elections, protocol configuration, or other collective decision-making processes.

### `marketplace`

Manages listings, offers, auctions, purchases, rentals, royalties, or seller settlement. Add an asset type only when the contract also implements that asset behavior.

### `membership`

Manages enrollment, member records, roles, subscriptions, credentials, or access rights tied to membership.

### `nft`

Issues or manages unique or semi-fungible assets and their ownership. A contract that only accepts an external NFT is not necessarily an NFT contract.

### `oracle`

Publishes, aggregates, verifies, or delivers externally sourced data for other contracts.

### `payments`

Handles checkout, billing, invoicing, payment routing, transfers, subscriptions, or settlement where payment processing is a substantial purpose.

### `registry`

Maintains an authoritative directory, mapping, identity record, name service, certification record, or status registry queried by users or contracts.

### `staking`

Locks or delegates assets or resources for rewards, participation, allocation, or network or protocol security.

### `token`

Issues or manages a fungible asset, including supply, balances, and transfers.

### `treasury`

Holds, budgets, allocates, invests, or disburses pooled assets according to defined controls.

### `utility`

Provides reusable infrastructure or a general contract service that does not fit a more specific type. Prefer a precise custom type when `utility` would conceal the main purpose.

## Contract overview clause

Use exactly one `ui.contract` clause:

```json
{
  "id": "ui.contract",
  "body": "---\nschema: relocke.ui/1\ntypes: token,bridge\ntitle: Example bridged token\n---\nIssues a fungible token and supports transfers between documented networks."
}
```

The Markdown body should explain:

- purpose and intended users;
- how the assigned types apply;
- important behavior and operating model;
- significant assets or dependencies;
- limitations, trust assumptions, and integration guidance; and
- important behavior that is not visible from the standard ABI.

## Action Ricardian contracts

Use the standard `ricardian_contract` property on every documented ABI action:

```json
{
  "name": "claim",
  "type": "claim",
  "ricardian_contract": "---\ntitle: Claim available rewards\n---\nClaims accrued rewards for the authorized owner. On success, the contract updates the rewards table and sends the documented inline token transfer."
}
```

Describe:

- action intent;
- required authorization;
- important preconditions and accepted values;
- state and affected-table changes;
- inline actions and external effects; and
- known failure conditions.

Optional `title` frontmatter provides a clearer interaction label. Never move serialization rules out of the standard ABI struct.

## Table clauses

Use `table.<table_name>`:

```json
{
  "id": "table.accounts",
  "body": "Stores token balances by owner and symbol. Scope is the token owner. Rows are created on first receipt and removed when the balance reaches zero."
}
```

Explain:

- what one row represents;
- scope and key conventions;
- units and value interpretation;
- row lifecycle;
- relationships with other tables; and
- actions or external triggers that create, update, and remove rows.

Use the first exact, case-sensitive match. Normalize duplicate documentation IDs before saving.

## External-trigger clauses

Use `external.<stable-id>` when an action on another contract notifies or otherwise causes behavior in the documented contract:

```md
---
contract: eosio.token
action: transfer
title: Fund an account
tables: deposits,accounts
---
Send a supported token with the documented memo to begin account creation.
The notification validates the token and amount, records the deposit, and
starts account creation.
```

Multiple clauses may reference the same source action when memo formats, conditions, or outcomes differ. Legacy `callable.<stable-id>` clauses may be read for compatibility, but new documentation should use `external.` because the behavior is not directly invoked on the displayed contract.

External-trigger documentation does not prove that a payment is accepted, grant authority, generate a transaction, or guarantee an outcome.

## Safe enrichment workflow

1. Confirm chain and contract identity.
2. Fetch and retain the complete current ABI.
3. Parse standard types, structs, variants, actions, and tables before interpreting documentation.
4. Inventory `ui.contract`, action Ricardian contracts, `table.*`, `external.*`, and legacy `callable.*` clauses.
5. Gather the required verified context and identify uncertainty.
6. Choose all suitable types and explain why each applies.
7. Draft the smallest overview, action, table, and external-trigger changes needed.
8. Preserve every unrelated ABI section, clause, extension, and unknown metadata key.
9. Remove obsolete overview `type` and `categories` fields.
10. Validate JSON, frontmatter, clause identifiers, table references, type formatting, and duplicates.
11. Show the complete resulting ABI or `setabi` transaction for human review before deployment.
12. After deployment, fetch the ABI again and verify the parsed and rendered result.

Never create an empty replacement ABI after a failed fetch. Never serialize a transaction from prose. Revalidate the live ABI, permissions, chain state, token contracts, quantities, and user intent immediately before acting.

## Agent interpretation output

When explaining an enriched ABI, prefer:

- **Identity:** chain, contract account, and ABI version.
- **Purpose:** title, every assigned type, and overview.
- **Actions:** serialized interface, documented intent, authorization, state changes, side effects, and failures.
- **Tables:** row type, scope, units, lifecycle, and relationships.
- **External behavior:** source contract and action, trigger conditions, affected tables, and expected outcome.
- **Warnings:** missing, malformed, duplicate, deprecated, contradictory, or unverifiable documentation.
- **Safety:** live facts that must be revalidated before a transaction.

## User briefing template

```text
Inspect and enrich the ABI for <account> on <chain>.

The contract's verified purpose and intended users are:
<purpose and audience>

Its substantial capabilities are:
<capabilities>

Its assets, actions, tables, permissions, external triggers, side effects,
and failure conditions are:
<verified context>

Choose every suitable predefined contract type. Add a custom lowercase
kebab-case type only when the predefined set cannot accurately express a
substantial behavior. Explain why each type applies.

Preserve unrelated ABI content. Do not invent missing facts. Show the proposed
Ricardian changes and complete resulting ABI before deployment.
```

## Validation checklist

Before presenting or deploying an enrichment, confirm:

- exact chain and account identity;
- complete ABI retrieval succeeded;
- assigned types use lowercase kebab-case and contain no duplicates;
- each assigned type represents substantial contract behavior;
- there is at most one normalized `ui.contract` clause;
- action documentation maps to real actions;
- table clauses map to real tables;
- external-trigger table references are valid;
- unknown metadata and unrelated ABI content are preserved;
- Markdown is treated as untrusted content and sanitized before rendering; and
- consequential deployment or transaction steps remain subject to human review.
