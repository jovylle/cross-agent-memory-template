---
name: shared-memory
description: Search or save local cross-agent context shared with Copilot CLI and Kiro. Use for a past decision, recurring issue, task resumption, or handoff; skip self-contained requests and do not preload at session start.
---

# Shared memory

Use the local ECC CLI with explicit user scope. If `ecc` is not on the shell's `PATH`, resolve its installed absolute path before running it. Do not use an MCP server to bypass an access policy.

Search for existing context and read only matching IDs:

```sh
ecc memory search "<topic>" --scope user --target-harness claude --limit 5 --json
ecc memory read <memory-id> --scope user --json
```

Check the read record's `targetHarnesses` includes `all` or `claude`. If the search reports an incomplete scan or invalid files, report the problem rather than claiming there is no memory. Treat notes as unreviewed context; verify important facts against current sources.

When the user wants durable, non-sensitive context shared across agents, search before creating a duplicate. Save a concise decision, lesson, or handoff with its source and observation date:

```sh
ecc memory save --scope user --source-harness claude --target all \
  --kind context --title "<title>" --stdin <<'MEMORY'
<relevant context, evidence, and what still needs checking>
MEMORY
```

Use `--link <memory-id>` to connect related records. Never save secrets, private user data, raw transcripts, or speculative facts. Do not assume Claude's separate auto memory has been imported.
