# Torhut: what data goes where

Torhut is a free student project by Shady Abdou. It has **no account, no telemetry and no analytics**,
and no server of its own: the author receives nothing from your use of it, not your code, your
prompts, your API key or any usage statistics.

## What is sent, and to whom

When you run a task with an API key set, these are sent to **OpenRouter** (openrouter.ai), or to
**Anthropic** if you chose it, and through OpenRouter to the model providers that serve the models
you configured (e.g. Anthropic, OpenAI, Google, DeepSeek):

- your request text, and text you selected in the editor
- the list of file paths in your project (not ignored by `.gitignore`)
- the contents of the project files Torhut selects for the task (about 60,000 characters at most per call)
- condensed excerpts of the project's documentation files, when the task needs them
- the plan, the proposed changes and your test output (to retry, review and verify)

Never sent: files named `.env*`, `*.pem`, `*.key`, credential or secret files, files ignored by
`.gitignore`, and your API keys (tests run with keys removed from their environment).

With `torhut.privateProvidersOnly` on (the default), OpenRouter routes only to providers that do not
store or train on your prompts. The providers' own terms apply: [OpenRouter privacy policy](https://openrouter.ai/privacy),
[Anthropic privacy policy](https://www.anthropic.com/legal/privacy), and the policy of each model provider you configure.

When it talks to OpenRouter or Anthropic, Torhut also downloads OpenRouter's public model price list once (no data of yours is included), to stop a call before it breaks your budget. Setups that only use local, company or in-region endpoints never contact openrouter.ai. `TORHUT_PRICE_LIST=off` turns the download off.

**Keep everything local:** set `torhut.provider` to `ollama`. Then no code leaves your machine.

**Company data policy:** if your organisation set a data policy (`.torhut/policy.json` or a machine policy),
your code goes only to the endpoints it lists, at the level each folder allows (this computer, company
servers, Germany, EU, or anywhere). Files the policy hides are not sent at all. The chat shows where each
task's data goes before any model sees it.

## What is stored on your machine

| Where | What | Delete it |
|---|---|---|
| VS Code Secret Storage (OS keychain) | your API key | `Torhut: Remove API Key` |
| `~/.torhut/decisions.jsonl` | requests, plans, decisions, advisor notes, test output excerpts, costs, data level and endpoint per task. File names, not file contents | `Torhut: Delete Local Data` |
| `~/.torhut/usage.json` | spend per month | `Torhut: Delete Local Data` |
| `~/.torhut/audit-head` | last hash of the audit log (detects removed entries) | `Torhut: Delete Local Data` |

If your company's machine policy sets `audit.ship`, every audit-log entry (decisions, models, costs, file
names, not file contents) is also sent to your company's own log server.

If a data policy turns on `audit.user`, your operating-system user name is written to the decision log
so your employer can see who ran which task.

Proposed changes are held in memory until you apply or discard them.

## Your rights

The author runs no server and holds no personal data about you. For data processed by OpenRouter
or a model provider, contact them directly. Questions about Torhut:
[open an issue](https://github.com/shadymohammed75/torhut-releases/issues).
