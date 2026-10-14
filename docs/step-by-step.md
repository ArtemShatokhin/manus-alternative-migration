# Step by step: standing up Kortix, the open-source AI Operating System, and moving work over

Kortix is the open-source AI Operating System. These are the concrete steps to create a project, bring work across from Manus, and run the review loop. Every command comes from the Kortix documentation, and every step is a git operation on your project's repository.

## 1. Install the CLI and sign in

Install `kortix`, then sign in. The install script supports macOS and Linux on x86_64 and arm64, with WSL on Windows.

```bash
curl -fsSL https://kortix.com/install | bash
kortix login
```

`kortix login` opens a browser and returns a token on a local callback. On a machine without a browser, sign in with a token by running `kortix login --token kortix_pat_…` ([Kortix CLI documentation](https://kortix.com/docs/cli)).

## 2. Create the project

A project is a git repository with `kortix.yaml` at its root. Create one from the starter, then ship it. The first `kortix ship` creates the cloud project and pushes the repository.

```bash
kortix init my-agent
cd my-agent
kortix ship
```

`kortix init` writes `kortix.yaml`, `agents/`, `skills/`, `memory/` and `harnesses/opencode/` and runs `git init -b main`. `kortix ship` validates `kortix.yaml`, commits every change, asks for missing secrets, pushes the current branch, and asks you to connect the connectors you declared ([Kortix quickstart](https://kortix.com/docs/quickstart)).

If a teammate already created the project, clone it instead:

```bash
kortix projects clone <project_id> my-agent
cd my-agent
```

## 3. Add the agents and skills

Bring each Manus agent or recurring workflow across as a file. An agent is `agents/<name>.md` plus an `agents.<name>` block in `kortix.yaml`. A skill is a `skills/<name>/SKILL.md` folder. Validate the manifest before you push.

```bash
kortix validate
git add -A
git commit -m "Add migrated agents and skills"
kortix ship
```

The project layout holds `kortix.yaml`, `AGENTS.md`, `agents/`, `skills/`, `memory/`, `tools/` and `harnesses/` ([Kortix projects documentation](https://kortix.com/docs/project)).

## 4. Connect the models and keys

Use a provider key, or a subscription login, or the platform default. A provider key is stored as an encrypted project secret.

```bash
kortix providers set anthropic        # reads the key from stdin
kortix providers set openai sk-...    # key as an argument
kortix providers login openai         # ChatGPT Plus or Pro
kortix models default deepseek-v4.1-flash
```

Kortix takes any model provider with your own keys, or the ChatGPT plan you already pay for, or a sign-in with your OpenCode Console account for OpenCode Zen and Go ([Kortix models documentation](https://kortix.com/docs/project/models)). A self-hosted instance has no managed models, so connect your own provider there.

## 5. Re-add the connectors

Declare each app under `connectors:` in `kortix.yaml`, then authorize an account. Kortix holds the credential server-side, and the manifest never holds one.

```bash
kortix connectors add gmail --provider composio --app gmail --apply
kortix connectors connect gmail
```

A connector gives agents governed access to an external app, MCP server or API, and the project sets a policy of Allow, Ask or Block per action ([Kortix connectors documentation](https://kortix.com/docs/connect/connectors)).

## 6. Recreate the automation

Manus Automations become Kortix triggers. A trigger is declared in `kortix.yaml` and is either a cron schedule or a signed webhook. Kortix ships a `harness-reflector` cron trigger that runs daily at 03 UTC, disabled until you enable it ([Kortix projects documentation](https://kortix.com/docs/project)).

## 7. Run a session and review the work

Start a session to test the move. The agent works in its own sandbox, on its own branch.

```bash
kortix sessions new --prompt "Draft the weekly status report and open a change request"
kortix cr ls
kortix cr diff 1
kortix cr merge 1
git pull
```

A session runs in its own sandbox on its own branch, named `<session_id>`. The agent commits, pushes the session branch, and opens a change request against the default branch. A person merges it, and `kortix cr merge 1` merges change request number 1 into the base branch ([Kortix quickstart](https://kortix.com/docs/quickstart), [Kortix sessions documentation](https://kortix.com/docs/work/sessions)).

## 8. Optionally, self-host

Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host for free ([Kortix self-hosting documentation](https://kortix.com/docs/host)).

```bash
kortix self-host init --domain kortix.example.com --admin-email you@example.com
kortix self-host start
```

Point DNS at the server, open ports 80 and 443, and add a wildcard record for previews before the first start. The first run asks six things and no model key; add a model key in the app after signing in.

## 9. Check the setup

Run the doctor to confirm the login and the project, and to start a throwaway session end to end.

```bash
kortix doctor
```

`kortix doctor` starts a session, waits up to 180 seconds for its sandbox, sends a ping, waits for the reply, and deletes the session. It exits 0 when every check passes ([Kortix quickstart](https://kortix.com/docs/quickstart)).

## Read next

The [migration plan](migration-plan.md) gives the order of the move, and [What you own](what-you-own.md) sets out the ownership comparison behind each step.
