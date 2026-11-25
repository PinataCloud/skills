# Pinata Skills

[Agent Skills](https://agentskills.io) that teach coding agents how to use [Pinata](https://pinata.cloud): uploading to IPFS, public and private files, groups, gateways, presigned uploads and the Pinata MCP server.

## Install

```bash
npx skills add PinataCloud/skills
```

This works with Claude Code, Codex, Cursor, OpenCode and [other agents](https://github.com/vercel-labs/skills#supported-agents). Add `-g` to install for every project, or pick the agent with `-a claude-code`. More in the [Pinata docs](https://docs.pinata.cloud/tools/skills).

### Plugins

The plugins bundle the `pinata` skill with the [Pinata MCP server](https://mcp.pinata.cloud), so the agent can upload, share and manage files on your account.

**Claude** (Claude Code, claude.ai and Cowork) signs in with your Pinata account: the first time a Pinata tool runs, Claude opens Pinata's sign-in page, you choose the workspace to connect, and no API key is needed. Install it in Claude Code with:

```bash
claude plugin install pinata --marketplace PinataCloud/skills
```

Or from inside a session: `/plugin install pinata --marketplace PinataCloud/skills`. Update with `claude plugin update pinata@pinata`. Sign-in creates a Pinata API key named `MCP OAuth · <client>` on that workspace, with your role's rights to files, groups and vectors, read-only gateways and analytics, and never admin or billing; revoke it in [API Keys](https://app.pinata.cloud/developers/api-keys) to disconnect.

**What the Claude plugin runs and sends.** The plugin has two parts, both from Pinata:

- **The Pinata MCP server**, `https://mcp.pinata.cloud`, a remote server; the plugin starts no local process. Claude sends it the tool calls you ask for (file and folder uploads, names, metadata, group and gateway operations), and it acts on your Pinata account with the key that sign-in created. It reads your account's email address and your files' metadata, and stores what you upload until you delete it.
- **The `pinata` skill**, instructions only. When you ask Claude to write or run code that uses Pinata, the code it writes calls Pinata's own services with your own API key: the REST API at `api.pinata.cloud` and `uploads.pinata.cloud`, and your gateway at `<your-gateway>.mypinata.cloud`. The skill links to `docs.pinata.cloud` for reference.

Nothing is sent anywhere other than Pinata. Pinata's [privacy policy](https://pinata.cloud/terms#privacy-policy) covers the data.

**Codex** needs a [Pinata API key](https://app.pinata.cloud/developers/api-keys) (JWT). It reads the key from `PINATA_JWT`, which must be exported before Codex starts. Without it the Pinata tools don't load:

```bash
codex plugin marketplace add PinataCloud/skills
codex plugin add pinata@pinata
export PINATA_JWT=<your JWT>
```

Update with `codex plugin marketplace upgrade pinata`.

**Gemini CLI** signs in with your Pinata account, like Claude. Install the extension, then run `/mcp auth pinata` in a Gemini CLI session: it opens Pinata's sign-in page in your browser (Gemini CLI receives the callback on `localhost:7777`):

```bash
gemini extensions install https://github.com/PinataCloud/skills
```

Update with `gemini extensions update pinata`.

Other agents: install the skill with `npx skills add PinataCloud/skills` and add the MCP server by hand, as in the [MCP server docs](https://docs.pinata.cloud/tools/mcp).

Prefer an API key to signing in? The MCP server still accepts a [Pinata API key](https://app.pinata.cloud/developers/api-keys) (JWT) sent as `Authorization: Bearer <JWT>`. Add it yourself instead of the plugin's sign-in, for example in Claude Code: `claude mcp add --transport http pinata https://mcp.pinata.cloud --header "Authorization: Bearer <your JWT>"`. The [MCP server docs](https://docs.pinata.cloud/tools/mcp) show the same for other clients.

## Skills

| Skill | Use it for |
|---|---|
| [`pinata`](skills/pinata/SKILL.md) | Storing, organizing and serving files on IPFS with Pinata: the `pinata` SDK, the REST API and the Pinata MCP server |

The agent loads a skill when a task matches its description. You can also ask for it by name: "use the pinata skill to add avatar uploads to this app".

## Contributing

Each skill is a folder under `skills/` with a `SKILL.md` (YAML frontmatter with `name` and `description`, then instructions) and optional `references/` the agent reads when it needs more detail. Keep `SKILL.md` short and link to [docs.pinata.cloud](https://docs.pinata.cloud) for the full reference.

The `pinata` skill is also served from [docs.pinata.cloud](https://docs.pinata.cloud/.well-known/agent-skills/index.json), so `npx skills add https://docs.pinata.cloud` installs the same skill. That copy lives in [PinataCloud/docs](https://github.com/PinataCloud/docs) at `.mintlify/skills/pinata/SKILL.md`. It's identical except that its `references/` links point to the raw files here, because the docs site serves only `SKILL.md`. Update it whenever `skills/pinata/SKILL.md` changes; CI warns when the two differ.

Test a change locally before opening a pull request:

```bash
npx skills add ./ --list
claude plugin validate .
claude --plugin-dir .
```

## License

[MIT](LICENSE)
