# Kaksos notes for this fork (not implemented yet)

Prepared 9 Oct 2026 for Brandon's morning review. Nothing has been deployed or run.

## What's ready
- `server/index.ts`: upstream OB1 MCP server (Supabase edge function, TypeScript, 643 lines), unchanged.
- `setup/01_thoughts_schema.sql`: the five SQL blocks from `docs/01-getting-started.md` extracted verbatim, in order (thoughts table, indexes, updated_at trigger, match_thoughts search function, and the remaining setup blocks).
- `setup/server.env.example`: the env vars the server reads.

## Kaksos ideas to decide on later (do NOT implement yet)
1. **Approved-only filter.** Only human-approved seeds should be searchable. Mirror Kaksos's `training_status` / `is_boundary` flags as columns (or metadata keys) and put the filter inside `match_thoughts` itself, so no caller can skip it. This is also the fix for the open Kaksos seed-leak defect (public draw ignores training status / boundary) in `kaksos-api/src/utils/prompt-assembly.js` `getMemoryContext`.
2. **Per-circle meaning search.** Kaksos currently returns the newest ~50 seeds allowed by the viewer's circle, without semantic ranking. Add a `circle` column and make `match_thoughts` filter by allowed circles before ranking by similarity.
3. **Provenance.** Tag every thought with which vessel agent wrote it (OB1's agent-memory add-on has per-agent keys and recall logging worth borrowing).
4. **Security.** OB1 core uses one shared key and the service-role key bypasses row-level security, so the server is the only boundary. Don't copy that model for Kaksos; keep Kaksos circle gating as the authority.
5. **Hosting.** Candidates: Supabase cloud as upstream intends, or locally on the Mac mini (pickup 26 Oct) with Postgres + pgvector. Nitro 5 can run it but sleeps/travels.

Licence: FSL-1.1-MIT (converts to MIT later); check terms before any commercial use.
