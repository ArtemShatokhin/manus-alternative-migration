# Migration plan: moving from Manus to Kortix, the open-source AI Operating System

Kortix is the open-source AI Operating System. This plan gives the order in which a team moves its work off a hosted agent and onto a system it owns, so nothing is left behind in an account you cannot host yourself.

## The two things that change in the move

A move off a hosted agent changes ownership and placement at the same time. Ownership decides who holds the agents, the memory and the connectors. Placement decides where the system runs. Manus keeps both on its side: its maker calls it "a managed agent product that handles model selection, harness, and agent infrastructure", and its own Website Builder documentation describes "fully managed cloud infrastructure" ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex), [Manus cloud infrastructure documentation](https://manus.im/docs/website-builder/cloud-infrastructure)). Kortix puts both in your hands: a project is one git repository with `kortix.yaml` at its root, and you can run the platform on Kortix Cloud, in your VPC, or on your own on-prem network ([Kortix projects documentation](https://kortix.com/docs/project), [Kortix self-hosting documentation](https://kortix.com/docs/host)).

## What Manus gives you, and what it does not

Manus is a general agent built for finished work. Its documentation says it "operates in a complete sandbox environment" with "a persistent file system", and it ships Projects, Skills, Automations, MCP connectors and a Manus API ([Manus documentation](https://manus.im/docs/introduction/welcome), [Manus documentation index](https://manus.im/docs/llms.txt)). Its plans meter usage in credits, and its Team and Enterprise tiers add SSO ([Manus pricing](https://manus.im/pricing)).

Bring-your-own-key exists on Manus too. Manus Flex connects "your own API key from a supported inference provider", with OpenRouter, Fireworks and Modal as the first partners, and "model inference is billed directly by that provider" ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). Two limits remain, and both matter for a migration:

- Flex covers inference only. The agent system that plans the work, the harness and the surrounding infrastructure stay on Manus's side, and Manus still charges credits for them.
- No self-host. Manus documents managed hosting for the apps you build and no way to run the Manus agent on servers you control ([Manus cloud infrastructure documentation](https://manus.im/docs/website-builder/cloud-infrastructure)).

Kortix covers both. It takes any model provider with your own keys, and it is a platform you can self-host for free ([Kortix models documentation](https://kortix.com/docs/project/models), [Kortix self-hosting documentation](https://kortix.com/docs/host)).

## What a team moves, and in what order

Move in this order. Each item depends on the one before it, because the repo is the destination for everything else.

| Order | What moves | Where it lands in Kortix |
| --- | --- | --- |
| 1 | Agents and their prompts | `agents/<name>.md`, one file per agent |
| 2 | Skills and reusable know-how | `skills/<name>/SKILL.md` folders |
| 3 | Company memory and context | `memory/`, indexed by `memory/MEMORY.md` |
| 4 | Connectors and their policies | `connectors:` blocks in `kortix.yaml` |
| 5 | Model access and provider keys | Provider logins and per-project secrets |
| 6 | Schedules and automations | `triggers:` blocks, cron and webhook |

The Kortix repository layout holds each of these as a file: `kortix.yaml`, `agents/`, `skills/`, `memory/`, `tools/` and `harnesses/` ([Kortix projects documentation](https://kortix.com/docs/project)). Connectors are declared in `kortix.yaml`, and the manifest never holds a credential ([Kortix connectors documentation](https://kortix.com/docs/connect/connectors)).

## The order in practice

1. **Stand up the repo first.** Create a Kortix project so there is a destination for the files. Run `kortix init my-agent`, then `kortix ship` ([Kortix quickstart](https://kortix.com/docs/quickstart)).
2. **Bring the agents over.** Manus instructions live inside tasks and Projects. In Kortix an agent is `agents/<name>.md` plus an `agents.<name>` block in `kortix.yaml` ([Kortix projects documentation](https://kortix.com/docs/project)).
3. **Bring the skills over.** Rewrite each recurring Manus workflow as a skill under `skills/<name>/SKILL.md`, so the know-how is versioned with the repo.
4. **Move the memory.** Put the context a team repeats into `memory/`, with `memory/MEMORY.md` as the index. A memory edit is an ordinary file change that reaches the default branch through a change request.
5. **Re-add the connectors.** Declare each app under `connectors:` and authorize an account. Connector credentials are brokered server-side and never enter the machine, so no key is copied onto a developer laptop during the move.
6. **Connect the models.** Use `kortix providers set` for a provider key or `kortix providers login` for a subscription. The credential is stored as an encrypted project secret ([Kortix models documentation](https://kortix.com/docs/project/models)).
7. **Recreate the automation.** Manus Automations become Kortix triggers, declared in `kortix.yaml` and run on the default branch.

## Set up the destination

The destination is one command away from a working project:

```bash
curl -fsSL https://kortix.com/install | bash
kortix login
kortix init my-agent
cd my-agent
kortix ship
```

`kortix init` writes `kortix.yaml`, `agents/`, `skills/`, `memory/` and `harnesses/opencode/`, and `kortix ship` validates the manifest, commits, asks for missing secrets, pushes, and asks you to connect the connectors you declared ([Kortix quickstart](https://kortix.com/docs/quickstart)). Run `kortix doctor` to check the login and the project ([Kortix CLI documentation](https://kortix.com/docs/cli)).

## Where to run it

Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host for free. The self-hosted path installs the full platform as one Docker Compose stack with `kortix self-host init` and `kortix self-host start` ([Kortix self-hosting documentation](https://kortix.com/docs/host)). The first run asks six things and no model key; models are added in the app afterward.

## Compare the surfaces

The [ownership comparison](what-you-own.md) puts the repo, connectors, memory, models and keys side by side. The [step-by-step guide](step-by-step.md) carries the exact commands.
