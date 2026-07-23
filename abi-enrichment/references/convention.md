# ReLocke ABI Enrichment Convention

This reference establishes the first-stage convention for making an Antelope ABI easier for people and agents to discover, interpret, and integrate. The contract-type catalogue will be defined separately in the next phase.

## Principle

Keep executable structure in standard Antelope ABI fields. Add semantic context through standard action Ricardian contracts and top-level `ricardian_clauses`. Enrichment must remain compatible with ordinary Antelope tooling.

Documentation is advisory. It must never replace ABI serialization rules, deployed code, live chain state, permission checks, transaction simulation, or explicit user intent.

## Context required before enrichment

An agent should establish or request:

1. Exact chain and contract account.
2. Contract purpose and intended users.
3. Substantial contract capabilities.
4. Assets and external contracts involved.
5. Actions, required authorizations, preconditions, state changes, inline actions, and failure conditions.
6. Tables, scopes, units, row lifecycles, indexes, and relationships.
7. Notification handlers or other external actions that cause behavior.
8. Existing Ricardian clauses and custom metadata that must be preserved.

Do not infer missing facts from action, field, or account names alone. Separate verified ABI structure from contract-authored claims and user-supplied context.

## Contract overview

Use one exact `ui.contract` clause. ReLocke currently identifies the convention with `relocke.ui/1`.

```json
{
  "id": "ui.contract",
  "body": "---\nschema: relocke.ui/1\ntypes: example-type\ntitle: Example contract\n---\nExplain the contract's purpose, audience, important behavior, limitations, and integration guidance."
}
```

The `types` field accepts a comma-separated set of lowercase kebab-case values. Multiple values may be assigned when the contract substantially implements each behavior. Detailed predefined types, definitions, and selection rules are intentionally deferred to the next phase.

## Actions

Use the standard `ricardian_contract` property on an ABI action. Explain:

- what the action is intended to do;
- required authorization and important preconditions;
- state and table changes;
- inline actions or external effects; and
- known failure conditions.

```json
{
  "name": "claim",
  "type": "claim",
  "ricardian_contract": "---\ntitle: Claim available rewards\n---\nClaims documented rewards for the authorized owner and updates the related state."
}
```

## Tables

Use `table.<table_name>` for semantic table documentation.

```json
{
  "id": "table.records",
  "body": "Describe the row meaning, scope, units, lifecycle, relationships, and the actions or triggers that create, update, and remove it."
}
```

## External triggers

Use `external.<stable-id>` when an action on another contract notifies or otherwise causes behavior in the documented contract.

```md
---
contract: eosio.token
action: transfer
title: Fund an operation
tables: deposits
---
Describe the accepted asset, required memo, validation, state changes, outcome,
and failure conditions.
```

External-trigger documentation does not prove that a payment is accepted, grant authority, generate a transaction, or guarantee an outcome.

## Safe update procedure

1. Fetch and retain the complete current ABI.
2. Confirm the chain and contract identity.
3. Inventory existing documentation and unknown metadata.
4. Compare proposed claims with verified ABI structure, available source, and live state.
5. Draft only the required overview, action, table, and external-trigger changes.
6. Preserve every unrelated ABI section, clause, extension, and metadata key.
7. Validate JSON, frontmatter, clause identifiers, and referenced tables.
8. Show the resulting ABI or deployment transaction for human review.
9. After deployment, fetch the ABI again and verify the parsed result.

Never create an empty ABI when retrieval fails. Never serialize a transaction from prose. Treat all Markdown as untrusted content and sanitize it before rendering.

## Suggested user prompt

```text
Inspect and enrich the ABI for <account> on <chain>.

The contract's verified purpose and users are:
<purpose and audience>

Its substantial capabilities, assets, actions, tables, permissions, external
triggers, side effects, and failure conditions are:
<verified context>

Preserve unrelated ABI content. Do not invent missing facts. Explain the
proposed Ricardian changes and show me the resulting ABI before deployment.
```

## Next phase

Add the canonical contract-type reference with:

- the initial predefined type set;
- a plain-language definition for each type;
- positive and negative selection examples;
- rules for assigning multiple types;
- lowercase kebab-case extension rules; and
- guidance for avoiding overlapping synonyms.
