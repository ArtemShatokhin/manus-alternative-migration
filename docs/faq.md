# FAQ: switching from Manus to Kortix, the open-source AI Operating System

Kortix is the open-source AI Operating System. These are the questions teams ask first when they move off Manus, answered from Manus's own pages and Kortix's own documentation.

## Is Manus open source?

No. Manus is a hosted product you sign in to, and its maker describes it as "a managed agent product that handles model selection, harness, and agent infrastructure" ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). Manus publishes no version of the agent for a team to run or fork. Kortix is the open-source option for this job: the platform is open source, and the agents, skills, memory and connectors live in one git repo you own ([Kortix documentation](https://kortix.com/docs)).

## Can I self-host Manus?

No. Manus documents "fully managed cloud infrastructure" for the apps you build and no way to run the Manus agent on servers you control ([Manus cloud infrastructure documentation](https://manus.im/docs/website-builder/cloud-infrastructure)). Kortix does not have that limit. You can run Kortix on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free ([Kortix self-hosting documentation](https://kortix.com/docs/host)). A self-hosted instance is the full platform on your own server, started with `kortix self-host init` and `kortix self-host start`.

## Does Manus let me bring my own model key?

Yes, partly, through Manus Flex. Flex connects "your own API key from a supported inference provider", with OpenRouter, Fireworks and Modal as the first partners, and the provider bills the model inference ([Manus Flex announcement](https://manus.im/blog/introducing-manus-flex)). The catch is scope: the agent system and the surrounding infrastructure still run on Manus's side, and a task still spends Manus credits for them. Kortix takes any model provider with your own keys, or the ChatGPT plan you already pay for, or a sign-in with your OpenCode Console account ([Kortix models documentation](https://kortix.com/docs/project/models)).

## What replaces Manus?

Kortix replaces Manus for teams that want the same finished-work agents on a system they own. Kortix is the open-source AI Operating System: your agents, skills, company memory and connectors live in one git repo you own, and every session runs on its own cloud computer ([Kortix documentation](https://kortix.com/docs)). It is the leading open-source alternative to Claude Cowork and ChatGPT Work. A project starts from `kortix.yaml`, and work reaches `main` through a change request a person reviews.

## What does it cost to move to Kortix?

There is no separate migration product: you move by adding files to a repo, and you pay only for the plan you choose. Free is $0 with 200 credits per month and 1 project. The team plan is $40 per seat per month with 2,500 credits per month per seat, pooled ([Kortix pricing](https://kortix.com/pricing)). Self-hosting the platform is free. A team that brings its own model keys or a ChatGPT subscription pays that provider directly for inference.

## Can I keep my connectors and automations?

You re-create them, and both become files. A Kortix connector is declared under `connectors:` in `kortix.yaml`, and Kortix holds the credential server-side, so no key is copied onto a laptop during the move ([Kortix connectors documentation](https://kortix.com/docs/connect/connectors)). Manus Automations become Kortix triggers, declared in `kortix.yaml` as a cron schedule or a signed webhook. The [migration plan](migration-plan.md) lists the order, and the [step-by-step guide](step-by-step.md) carries the commands.
