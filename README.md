# TaskFuel for Grok Bot

Give your Bot access to specialist paid APIs through your TaskFuel account.
The plugin connects to TaskFuel's hosted MCP server using browser OAuth.
Provider calls use your existing prepaid balance; Grok Bot usage is separate.

## Connect for testing

Ask Grok Bot:

> Add TaskFuel as a remote MCP connection at https://app.taskfuel.ai/mcp using
> OAuth. Let me complete browser sign-in. Then list the tools, check my balance,
> and find company-enrichment capabilities. Do not make paid calls yet.

Complete the TaskFuel browser sign-in and review the connection consent.
Expect seven tools: `discover`, `quote`, `call`, `get_result`, `balance`,
`rate`, and `feedback`. Only `call` spends money. The connection is available
to the Bots on your Grok Bot account. Revoke it from TaskFuel's Connected apps
page when you no longer want it to have access.

A custom MCP connection tests the server but does not install this package's
skill. For a private Grok Bot trial, add `skills/taskfuel-paid-tools/SKILL.md`
as a private skill and verify that the Bot can invoke it. Marketplace package
installation must be tested separately when that path is available.

To verify the private setup, start a new Bot and ask it to use the saved
`taskfuel-paid-tools` skill, list TaskFuel's tools, check authentication and
discover a capability without spending. This path passed in the October 27
trial. The tested Grok Bot installer accepts catalog plugin IDs; it does not
import an unpublished ZIP, folder or GitHub repository as a complete package.

## Example: company research

This is an optional example, not a built-in workflow. The skill adapts to the
task you request and discovers providers at runtime.

> Use TaskFuel to look up company information for github.com. Read the
> endpoint docs and quote first. Spend no more than $0.25 total, including any
> polling. Return a short brief and CSV with the fields actually received,
> source, available freshness information and actual cost. Leave missing fields
> blank and label provider-reported facts. Do not contact anyone or top up my balance.

Available endpoints and prices change. The Bot discovers them at runtime and
uses a quote as the maximum price for a call. You can request a smaller budget
or ask for free discovery and quotes only.

Other requests can use different inputs and outputs, such as finding a work
email or extracting structured information from a webpage. The skill does not
force a CSV or a company lookup onto those tasks. Ordinary writing and
summarizing supplied content do not require a paid tool.

## Results and longer tasks

The Bot checks the returned fields against your request and flags missing or
stale data. For requested files it saves accessible results to its workspace,
verifies them and attaches them in chat. If a download cannot be accessed, it
reports the limitation instead of repeating the purchase.

For long jobs it can save a checkpoint and, with your authorization, arrange a
later check where Grok supports it. Retrieving a TaskFuel response and polling
a provider job are different operations; paid polls count toward your budget.
Creating a recurring routine is not part of installing this plugin.

## Limits

TaskFuel enforces per-call ceilings and available balance. Tracking a total
task budget is currently the agent's responsibility. A successful provider
response can still be empty or unhelpful and is charged normally.

If a call times out, retrieve it with `get_result` before considering another
purchase. Paid results are stored for one hour. Repeating a paid call can
charge twice.

Some large responses cannot be retrieved through the OAuth-only MCP connection.
The skill cannot change transport or storage limits. Provider download links
can also expire independently of TaskFuel's one-hour recovery window.

This package has no local server, install hooks, or API-key setup field.
The MCP endpoint is `https://app.taskfuel.ai/mcp` over Streamable HTTP.

## Local package development

In the development monorepo, from this package's source directory, run
`node scripts/build.mjs`. The exported package does not include build scripts.
The build creates a
standalone `dist/taskfuel/` folder with the manifest, MCP configuration, skill,
README, license and canonical TaskFuel mark on a square Warm Cream plate. Only those files are
included; internal testing records stay outside the distributable.

Before distributing, optimize the generated icon with
`npx svgo --precision=2 --multipass -i dist/taskfuel/assets/logo.svg` and
visually compare it at the intended display sizes. Rebuilding resets that
generated file to the canonical source composition.

To test the package in Cursor, copy the generated `taskfuel` folder into
`~/.cursor/plugins/local/` without replacing an existing installation, reload
Cursor, and inspect Customize. Cursor discovery is not proof of Grok Bot
package compatibility.

Sources: [Grok Bot app connections](https://docs.x.ai/grok-bot/computer-and-apps),
[plugin format](https://cursor.com/docs/reference/plugins),
[marketplace submission](https://x.ai/bot/marketplace/submit-a-plugin).

## License

Plugin configuration and instructions are MIT licensed. TaskFuel's name and
logo remain TaskFuel brand assets; the license does not grant trademark rights.
