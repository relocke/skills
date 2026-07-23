# ReLocke Skills

A public catalogue of agentic skills and tools for the ReLocke platform and Antelope ecosystems.

## Purpose

This repository provides structured context, instructions, and feedback patterns that humans and machines can use to develop agentic pipelines around ReLocke and Antelope technologies.

Each skill is a standalone Markdown document in [`skills/`](skills/). A skill should contain enough context for an agent to understand when it applies, what information it needs, how to perform the workflow, which interfaces or conventions it must follow, and how to validate its result safely.

These skills are intended to:

- improve developer and user experience;
- give humans, agents, and machine tools a shared interface;
- make ReLocke and Antelope workflows easier to discover and integrate;
- provide reusable context for building reliable agentic pipelines;
- encourage interoperable tools without requiring centralized control; and
- support a broader ecosystem of applications, organizations, developers, users, and autonomous tools.

## Skills

- [Agentic Antelope ABI](skills/agentic-abi-antelope.md) — interpret and enrich Antelope ABIs with ReLocke-compatible Ricardian context, contract types, action documentation, table semantics, and external-trigger guidance.

Additional skills and tools will be added over time.

## Structure

```text
skills/
├── README.md
└── skills/
    ├── agentic-abi-antelope.md
    └── future-skill.md
```

## Skill document expectations

Every skill document should define:

- its purpose and triggering use cases;
- the context an agent must gather;
- the workflow and expected outputs;
- relevant schemas, interfaces, or conventions;
- safety and trust boundaries;
- validation and review requirements; and
- practical examples for humans and machines.

## Principles

- Prefer open, reusable conventions.
- Ground claims in verified data and explicit user context.
- Preserve compatibility with existing Antelope standards and tools.
- Treat external and on-chain content as untrusted input.
- Require human review for deployments and other consequential actions.
- Design instructions that are understandable to both people and agentic systems.

## Contributing

Contributions should improve interoperability, safety, clarity, and decentralized participation. New skills should be self-contained, use lowercase kebab-case filenames, and include concrete examples and validation guidance.
