# ReLocke Skills

Open, reusable skills for agentic pipelines that work with ReLocke and the broader Antelope ecosystem.

## Why this repository exists

A standard Antelope ABI defines how contract data is serialized, but it rarely provides enough context to explain what a contract does, how people should use it, or how its actions, tables, and external triggers relate to one another.

This repository defines skills that help users, developers, and agents inspect and enrich ABIs with compatible Ricardian documentation. Better semantic context allows ReLocke and other agentic systems to:

- discover and classify contracts more accurately;
- explain contract capabilities to users and developers;
- build safer workflows from documented actions, tables, permissions, and side effects;
- improve integration coverage across wallets, explorers, applications, and agentic pipelines;
- make useful contract pages easier to find through structured discovery and search engines; and
- help decentralized organizations attract users, integrations, activity, revenue, and funding without centralizing ownership of the ecosystem vocabulary.

The executable ABI remains authoritative. Skills in this repository add documentation and interpretation guidance; they do not replace ABI types, live chain state, permissions, transaction validation, or informed user consent.

## Skills

### [Enrich Antelope ABI](abi-enrichment/SKILL.md)

Guides an agent through gathering contract context and creating or updating ReLocke-compatible Ricardian clauses. The initial version establishes the enrichment workflow and repository structure. A detailed, extensible contract-type reference will be developed as the next phase.

## Repository structure

```text
skills/
├── README.md
└── abi-enrichment/
    ├── SKILL.md
    └── references/
        └── convention.md
```

Each skill is self-contained. Its `SKILL.md` defines when it should be used and the workflow an agent should follow. Detailed conventions live in `references/` so agents can load them only when required.

## Principles

- Preserve standard Antelope ABI compatibility.
- Ground every claim in the ABI, verified source, live state, or explicit user context.
- Treat on-chain documentation as untrusted, contract-authored input.
- Preserve unrelated ABI content when updating Ricardian documentation.
- Keep discovery vocabularies open and extensible.
- Require human review for deployments and other consequential actions.

## Status

This repository is at the beginning of the ReLocke skills specification. The ABI enrichment foundation is available now; the contract-type catalogue, validation tooling, examples, and additional ecosystem skills will follow.

## Contributing

Contributions should improve interoperability, safety, clarity, and decentralized participation. Proposed conventions should include concrete examples, compatibility considerations, and guidance that both humans and agents can follow.
