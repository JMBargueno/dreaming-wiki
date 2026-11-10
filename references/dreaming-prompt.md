# Nightly dreaming pass

Copy the block below into your scheduler (Hermes cron, system crontab + agent CLI, or a manual paste). Fill in `{{VAULT}}`.

---

You are running the nightly maintenance pass ("dreaming") for the vault at `{{VAULT}}`. This is sleep-time compute: consolidate the day, keep the vault consistent. Follow this pipeline exactly.

1. **Read.** Locate new material since the last dreaming pass: your session history since then (use session search or equivalent) and `journal/` entries added since the last run. List `projects/`, `knowledge/`, `sources/` and `weekly/` to know what already exists before creating anything.

2. **Consolidate.** Durable lessons go to `knowledge/<topic>.md` (one note per topic; extend the existing note if it exists). State changes go to `projects/<name>.md`. Every change must be grounded in a session or a journal entry. Never invent facts.

3. **Lint.**
   - Broken wikilinks and orphan notes (not linked from anywhere): repair.
   - Contradictions between notes (and between notes and `sources/`), claims made stale by new sources, important concepts mentioned without their own note. If found: fix it, or leave an explicit "Contradiction: ..." line in the note plus a journal entry. Never silently resolve a discrepancy.
   - Unprocessed sources: a new file in `sources/` that no note cites: read it and integrate it with the capture flow (update the notes it affects, cite it back, log it).

4. **Representations.** Update the index (`Home.md`) only when something important is new (a new note, a structure change).

5. **Log.** Append a `## Dreaming` section (1-5 lines) to `journal/<today>.md` with what you consolidated, or "no changes" if nothing needed touching.

6. **Verify.** If the vault uses a sync backend, check its status after your writes; after bulk writes, confirm each new file is registered. No passwords or tokens may ever be written into notes.

Your final answer, 2-4 lines, is the log of this pass: what you touched, what you found, what you deliberately left alone.
