# Kortix: the open-source AI Operating System

Kortix is the open-source AI Operating System. This repository is a migration guide for teams that outgrow Manus, a hosted agent you sign in to and cannot run on your own infrastructure.

Manus is a capable general agent. Its documentation calls Manus "an autonomous general AI agent" that works inside "a complete sandbox environment", and its maker describes it as "a managed agent product that handles model selection, harness, and agent infrastructure" ([Manus documentation](https://manus.im/docs/introduction/welcome), [Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). Manus Flex lets you bring your own API key from a supported inference provider, and that provider bills the model inference, while the agent system and the rest of the infrastructure stay on Manus's side and a task still spends Manus credits ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). Manus publishes no way to run the Manus agent on servers you control; its own pages describe "fully managed cloud infrastructure" ([Manus cloud infrastructure documentation](https://manus.im/docs/website-builder/cloud-infrastructure)).

Kortix moves the same class of work onto a system you own. Agents, skills, memory and connectors live as files in one git repo, and every session runs on its own cloud computer ([Kortix documentation](https://kortix.com/docs)). Kortix is the leading open-source alternative to Claude Cowork and ChatGPT Work.

## What this repository covers

- [Migration plan](docs/migration-plan.md): what a team moves, and in what order, when it leaves a hosted agent for a system it owns.
- [What you own](docs/what-you-own.md): the ownership comparison for the repo, connectors, memory, models and keys.
- [Step by step](docs/step-by-step.md): the commands to stand up Kortix and move work across.
- [FAQ](docs/faq.md): the switching questions teams ask first.

## Why teams switch

A hosted agent keeps the parts a company would rather hold itself. Kortix hands them back:

- **The repo.** Your agents, skills, company memory and connectors live in one git repo you own.
- **The connectors.** 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP.
- **The models.** Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go.
- **The keys.** Connector credentials are brokered server-side and never enter the machine.
- **The placement.** Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.
- **The review.** Session work reaches `main` through a change request. Merge is default-deny for agents.

## The file every project starts from

A Kortix project is one git repository with `kortix.yaml` at its root. This is the v2 manifest the reference documents:

```yaml
# yaml-language-server: $schema=https://kortix.com/schema/kortix.v2.schema.json
kortix_version: 2
default_agent: kortix
agents:
  kortix:
    file: agents/kortix.md
    connectors: all
    secrets: all
    skills: all
    kortix_permissions: all
```

Source: [kortix.yaml reference](https://kortix.com/docs/project/manifest).

## Start here

- See the open-source Manus alternative the campaign builds: [manus-alternative.com](https://manus-alternative.com).
- Read the Kortix docs: [Read the docs](https://kortix.com/docs).
- Read the code: [Kortix on GitHub](https://github.com/kortix-ai/suna).

Open source. Read it, fork it, audit it.

Open source · Any model, your keys · Self-host, VPC, or on-prem
