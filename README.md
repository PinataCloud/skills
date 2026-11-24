# Pinata Skills

[Agent Skills](https://agentskills.io) that teach coding agents how to use [Pinata](https://pinata.cloud): uploading to IPFS, public and private files, groups, gateways, presigned uploads and the Pinata MCP server.

## Install

```bash
npx skills add PinataCloud/skills
```

This works with Claude Code, Codex, Cursor, OpenCode and [other agents](https://github.com/vercel-labs/skills#supported-agents). Add `-g` to install for every project, or pick the agent with `-a claude-code`. More in the [Pinata docs](https://docs.pinata.cloud/tools/skills).

### Plugins

The plugins bundle the `pinata` skill with the [Pinata MCP server](https://mcp.pinata.cloud), so the agent can upload, share and manage files on your account. Each one needs a [Pinata API key](https://app.pinata.cloud/developers/api-keys) (JWT).

**Claude Code** asks for the key when the plugin is enabled and keeps it in your system keychain:

```bash
claude plugin install pinata --marketplace PinataCloud/skills
```

Or from inside a session: `/plugin install pinata --marketplace PinataCloud/skills`. Update with `claude plugin update pinata@pinata`.

**Codex** reads the key from `PINATA_JWT`, which must be exported before Codex starts. Without it the Pinata tools don't load:

```bash
codex plugin marketplace add PinataCloud/skills
codex plugin add pinata@pinata
export PINATA_JWT=<your JWT>
```

Update with `codex plugin marketplace upgrade pinata`.

**Gemini CLI** asks for the key on install and keeps it in your system keychain:

```bash
gemini extensions install https://github.com/PinataCloud/skills
```

Change it with `gemini extensions config pinata PINATA_JWT`, update with `gemini extensions update pinata`.

Other agents: install the skill with `npx skills add PinataCloud/skills` and add the MCP server by hand, as in the [MCP server docs](https://docs.pinata.cloud/tools/mcp).

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
