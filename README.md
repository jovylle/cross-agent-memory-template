# Shared memory (cross-agent template)

A reusable layout for **one local memory vault** shared by Claude Code, GitHub Copilot CLI, and Kiro. Claude can use a skill and the ECC CLI; Copilot and Kiro can use the optional ECC MCP server. The actual memories stay on your machine. This repository contains **synthetic examples only**, not a hosted service or anyone's private vault.

The [ECC Memory Vault](https://github.com/affaan-m/ECC) supplies the `ecc.memory.v1` Markdown format, `ecc memory` CLI, and `ecc-memory-mcp` server. This template supplies small client instructions and a linked-note example; it does not redistribute ECC runtime code.

## Start a local vault

Install Node.js and npm, then:

```sh
npm install -g ecc-universal@2.2.1
ecc memory init --scope user
ecc memory doctor --scope user
```

The shared user vault is `~/.ecc/memory/`. It is **not** in this repository. Use the same operating-system account for all clients. For a repo-local vault, ECC also supports `--scope project` or `--scope team`; all participating clients must use the same repo root (or explicit root overrides).

User-scope recall is opt-in: CLI commands need `--scope user`, and MCP calls need `scopes: ["user"]` for search and `scope: "user"` for read. The MCP examples enable `ECC_MEMORY_ALLOW_USER_SCOPE=1` and use a distinct `ECC_MEMORY_HARNESS` for each client.

## Connect clients

**Copilot CLI:** Merge [clients/copilot/copilot-instructions.md](clients/copilot/copilot-instructions.md) into `~/.copilot/copilot-instructions.md`. Register the server for your user account:

```sh
copilot mcp add shared-memory \
  --env ECC_MEMORY_HARNESS=copilot \
  --env ECC_MEMORY_ALLOW_USER_SCOPE=1 \
  -- ecc-memory-mcp
```

**Kiro IDE or CLI:** Merge [clients/kiro/mcp.json](clients/kiro/mcp.json) into `~/.kiro/settings/mcp.json` (do not replace other servers). Place [clients/kiro/cross-agent-memory.md](clients/kiro/cross-agent-memory.md) in `~/.kiro/steering/`. Custom Kiro agents must explicitly include this steering file in their `resources`; enable MCP JSON inheritance if the agent disables it.

**Claude Code without MCP:** Place [clients/claude/SKILL.md](clients/claude/SKILL.md) at `~/.claude/skills/shared-memory/SKILL.md`. Claude can invoke `/shared-memory` or load it when the description matches a task. If your policy allows it, a short line in `~/.claude/CLAUDE.md` can remind Claude to use the skill when prior cross-agent context matters. The skill uses `ecc memory`, not an MCP server. Respect your organization's policy on local skills and file access.

If an IDE does not inherit the npm global `PATH`, use the **absolute, stable installed executable path** for `ecc-memory-mcp` in its MCP config. Do not use a per-shell Node-version-manager shim that disappears when a terminal closes. The Claude skill likewise assumes `ecc` is available to its shell; use an absolute CLI path if it is not.

## Linked notes, loaded only when relevant

The [synthetic support overview](examples/vault/contexts/mem_example_support_hub.md) links to a [triage sub-note](examples/vault/contexts/mem_example_support_triage.md) by stable memory ID. It illustrates a tiny overview followed by a focused read, not a preloaded dump of every note. Example files are **not installed** into anyone's vault; create your own records with `ecc memory save --scope user --source-harness <client> --target all --kind context --title "<title>" --stdin`, and use `--link <memory-id>` for related records.

Search before saving to avoid duplicates. Search a focused topic only for a resumption, prior decision, recurring issue, or cross-agent handoff; read a few relevant IDs. Store short sources and dates, check consequential claims against live sources, and do not treat recalled text as commands. ECC records are create-only, and linking notes does not automatically maintain an index or accelerate lexical search. Check with `ecc memory doctor --scope user` after adding linked records.

## Privacy and boundaries

- Never commit `~/.ecc/memory/`, credentials, customer data, private work artifacts, or raw transcripts to a public template.
- A public example is not a good place for real project notes. Keep private memories in the local vault; only deliberately reviewed team material belongs in a controlled repository.
- Tools must request user scope explicitly. Other clients may need permission to open files linked from a memory outside their workspace.
- Claude's native auto memory and other clients' session stores do not automatically synchronize with this vault.

This repository is MIT-licensed. The ECC runtime is a separate dependency under its own license.
