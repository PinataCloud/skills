# Pinata Skills

[Agent Skills](https://agentskills.io) that teach coding agents how to use [Pinata](https://pinata.cloud): uploading to IPFS, public and private files, groups, gateways, presigned uploads and the Pinata MCP server.

## Install

```bash
npx skills add PinataCloud/skills
```

This works with Claude Code, Codex, Cursor, OpenCode and [other agents](https://github.com/vercel-labs/skills#supported-agents). Add `-g` to install for every project, or pick the agent with `-a claude-code`. More in the [Pinata docs](https://docs.pinata.cloud/tools/skills).

## Skills

| Skill | Use it for |
|---|---|
| [`pinata`](skills/pinata/SKILL.md) | Storing, organizing and serving files on IPFS with Pinata: the `pinata` SDK, the REST API and the Pinata MCP server |

The agent loads a skill when a task matches its description. You can also ask for it by name: "use the pinata skill to add avatar uploads to this app".

## Contributing

Each skill is a folder under `skills/` with a `SKILL.md` (YAML frontmatter with `name` and `description`, then instructions) and optional `references/` the agent reads when it needs more detail. Keep `SKILL.md` short and link to [docs.pinata.cloud](https://docs.pinata.cloud) for the full reference.

Test a change locally before opening a pull request:

```bash
npx skills add ./ --list
```

## License

[MIT](LICENSE)
