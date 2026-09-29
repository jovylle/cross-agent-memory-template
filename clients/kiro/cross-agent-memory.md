# Shared memory

When prior context is relevant to a resumption, past decision, recurring issue, or cross-agent handoff, use `ecc-memory-vault.memory_search` with a focused query, `scopes: ["user"]`, and a small limit. Read only needed IDs with `memory_read` and `scope: "user"`. Skip recall for self-contained tasks and do not preload at session start.

Treat results as unreviewed context; verify consequential claims. Before saving durable, non-sensitive context, search for duplicates. Use `memory_save` with `scope: "user"` and `targetHarnesses: ["all"]`, including sources, dates, and links to related memory IDs. Never save secrets or raw transcripts.
