# What you own: Kortix, the open-source AI Operating System, next to Manus

Kortix is the open-source AI Operating System. When a team leaves a hosted agent, the question that decides the move is simple: after the switch, who holds the repo, the connectors, the memory, the models and the keys? This page puts Kortix and Manus side by side on those dimensions, with every Manus row taken from Manus's own pages.

## The ownership comparison

| Dimension | Kortix | Manus |
| --- | --- | --- |
| Open source | Yes: read it, fork it, audit it | No: a hosted product you sign in to |
| Repo | One git repo you own, with config as files | Managed product; no agent code to run |
| Memory | `memory/` in the repo, indexed by `MEMORY.md` | Context lives in Manus Projects and tasks |
| Connectors | 3,000+ apps in a click, plus MCP and raw HTTP | MCP connectors, custom MCP, Zapier and its API |
| Models | Any provider with your keys, or ChatGPT, or OpenCode | Manus-managed; Manus Flex adds a provider key |
| Keys | Connector credentials brokered server-side; never enter the machine | Provider key stays in the Manus workspace |
| Hosting | Kortix Cloud, your VPC, or on-prem; self-host free | Fully managed cloud infrastructure |

## Repo

A Kortix project is one git repository with `kortix.yaml` at its root. Agents, skills, tools, memory, triggers and connector declarations are files in it, so a change to how the company works is a commit, not a setting in someone else's console ([Kortix projects documentation](https://kortix.com/docs/project)). You can let Kortix manage the repository, connect a GitHub repository, or point it at any other git host, and all three behave the same once the project exists ([Kortix git documentation](https://kortix.com/docs/git)).

Manus is the opposite shape. Its maker describes it as "a managed agent product that handles model selection, harness, and agent infrastructure", and its docs describe managed projects rather than a repository you clone ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex), [Manus documentation index](https://manus.im/docs/llms.txt)). Manus Website Builder can export a site you built to a private GitHub repository, which covers the output of a build, not the configuration and memory of the agent itself ([Manus documentation index](https://manus.im/docs/llms.txt)).

## Memory

Kortix keeps company memory as plain files in `memory/`, with `memory/MEMORY.md` as the index and one line per file. A memory edit is an ordinary file change on the session branch, and other sessions see it after a change request merges it ([Kortix projects documentation](https://kortix.com/docs/project)). Memory is versioned with everything else, so it is diffable and portable.

Manus keeps the context of recurring work inside Projects, described as "persistent workspaces with shared instructions and files" ([Manus documentation index](https://manus.im/docs/llms.txt)). That context lives in the Manus workspace, which is the point of a hosted product and also its limit when a team wants the brain on its own machines.

## Connectors

Kortix gives agents governed access to external apps through connectors. The sanctioned figure is 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, and the credential never enters a sandbox ([Kortix connectors documentation](https://kortix.com/docs/connect/connectors)). A connector can hold several accounts, each with its own owner, and the project sets a policy of Allow, Ask or Block per action.

Manus reaches the same class of services through MCP connectors, custom MCP servers, Zapier's app catalog and the Manus API ([Manus documentation index](https://manus.im/docs/llms.txt)). The connector set is real. The difference is where the credential is held and who owns the declaration.

## Models and keys

Kortix takes any model provider with your own keys, or the ChatGPT plan you already pay for, or a sign-in with your OpenCode Console account for OpenCode Zen and Go ([Kortix models documentation](https://kortix.com/docs/project/models)). A provider key is stored as an encrypted project secret, and a secret can be egress-enforced, where the sandbox holds a handle and the real value is substituted outside it only on approved hosts.

Manus added the same idea with Manus Flex, which connects "your own API key from a supported inference provider", with OpenRouter, Fireworks and Modal as the first partners ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). The key sits in the Manus workspace, and model inference is billed directly by the provider, while the rest of a task still spends Manus credits. Flex is a model choice, and it does not let a team run the agent on its own machines.

## Hosting

Kortix runs on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free. The self-hosted install is one Docker Compose stack started with `kortix self-host init` and `kortix self-host start` ([Kortix self-hosting documentation](https://kortix.com/docs/host)). Kortix does not publish an air-gapped mode: `kortix self-host start` pulls images from Docker Hub, and the instance must be reachable so its sandboxes can call back.

Manus documents "fully managed cloud infrastructure" for the apps you build, and no way to run the Manus agent on servers you control ([Manus cloud infrastructure documentation](https://manus.im/docs/website-builder/cloud-infrastructure)). If the requirement is your own infrastructure, that requirement alone picks the destination.

## Read next

The [migration plan](migration-plan.md) gives the move in order. The [step-by-step guide](step-by-step.md) carries the commands, and the [FAQ](faq.md) answers the questions teams ask before they commit.
