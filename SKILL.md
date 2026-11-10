---
name: dreaming-wiki
description: >
  Build and maintain a living, self-maintaining second brain: a Karpathy-style
  LLM Wiki (markdown + Obsidian) kept alive by a nightly "dreaming" pass and a
  weekly digest. Use when asked to set up a second brain or knowledge vault, to
  install dreaming/digest jobs, or to run the maintenance pass.
---

# Dreaming Wiki

Karpathy's LLM Wiki: a folder of markdown that an LLM writes and maintains, instead of re-deriving answers from raw documents on every query (anti-RAG). What this skill adds on top is a **nightly dreaming pass**: the wiki is not a folder you update when you remember. It consolidates, lints and summarizes itself while you sleep.

## When to use

- **Setup** — the user asks to build or install a second brain, a knowledge vault, or "set up my dreaming wiki".
- **Maintenance** — the user asks to run a dreaming / digest / lint on an existing vault, or a scheduled job invokes this skill.

## Vault layout

| Path | Holds | Written by |
|---|---|---|
| `sources/` | Raw captures: articles, PDFs, gists, data. `AAAA-MM-DD-slug.md`, frontmatter: `source_url`, `title`, `ingested_at`, `content_type` | Agent captures; **nobody edits afterwards** |
| `knowledge/` | Durable knowledge, one note per topic (kebab-case) | Agent writes; human reads and curates |
| `projects/` | Active projects: state, decisions, next steps, how to resume | Agent writes |
| `journal/` | One file per day, `AAAA-MM-DD.md`, append-only | Agent appends |
| `weekly/` | Weekly digest `AAAA-Www.md` (ISO week) | Agent, Sundays |
| `Home.md` | Index / MOC: structure table + links to important notes | Agent updates |

Obsidian is the IDE: wikilinks `[[note-name]]`, graph view, backlinks. The agent works with plain files and needs nothing else.

## The schema layer

The conventions above are the schema: they turn the agent into a disciplined maintainer instead of a free writer. Keep them in one place (this skill, or a section in `Home.md`) and reference that place; do not scatter rules across the notes.

## Operations

- **Capture (ingest)** — user shares a URL or file: store it verbatim in `sources/` (immutable), update or create the `knowledge/`/`projects/` notes it affects, each citing the source with a wikilink, and log it in today's journal.
- **Query** — answer from the notes. Any answer worth keeping is archived as a new note, not left in chat.
- **Lint** — broken wikilinks, orphan notes, contradictions between notes and sources, claims made stale by new sources, important concepts mentioned without a note, unprocessed sources.
- **Digest** — weekly synthesis from the journal, delivered short.

## The two jobs

### 1. Dreaming (nightly, default 02:00)

Full spec: [references/dreaming-prompt.md](references/dreaming-prompt.md). It finds new material since the last pass (agent sessions + journal), consolidates durable knowledge, lints the vault, logs to `journal/`, and ends with a 2-4 line report.

Scheduling:

- **Hermes Agent**:
  `hermes cron create "0 2 * * *" --name "Dreaming" --deliver local --skill dreaming-wiki --prompt "$(cat references/dreaming-prompt.md)"` (fill in the vault path first)
- **Claude Code / any CLI agent** (system crontab):
  `0 2 * * * cd {{VAULT}} && claude -p "$(cat ~/.claude/skills/dreaming-wiki/references/dreaming-prompt.md)" >> ~/.dreaming.log`
- **Manual** — paste the prompt into the agent whenever.

### 2. Weekly digest (Sunday)

Full spec: [references/digest-prompt.md](references/digest-prompt.md). Reads the 7-day journal, writes `weekly/AAAA-Www.md`, delivers a short summary. Same scheduling options.

## Ground rules (non-negotiable)

1. **`sources/` is immutable.** Never edit a capture. If the original changes, capture a new version with a new date.
2. **Never silently resolve a contradiction.** Surface it: an explicit "Contradiction: ..." line in the note plus a journal entry. The human decides.
3. **No invented facts.** Every consolidation must be grounded in a session, a journal entry or a source.
4. **List before creating.** Know what notes exist before creating new ones. No duplicates.
5. **The journal is append-only.** One file per day, dated sections.
6. **The digest summarizes, never duplicates** the journal: link notes, do not copy text.
7. **No secrets in the vault.** No passwords, tokens or API keys in any note.
8. **Verify after bulk writes** (sync status, or a file check) before reporting success.
9. **Keep job reports short.** The human reads the 2-4 line log, not the transcript.

## Setup walkthrough

When asked to build a vault:

1. Ask for the vault location (or propose one) and confirm the session source: Hermes uses `session_search`; Claude Code uses its transcripts; anything else falls back to journal-only.
2. Create the structure: five folders + `Home.md` with the structure table and an empty index.
3. Persist the schema: this file's conventions, in a place the agent re-reads (a skill file or a `Home.md` section).
4. Schedule both jobs (see above). Confirm the schedule with the user.
5. First capture: ingest one or two real sources from the user's current interests so the vault does not start empty.
6. Log the setup in `journal/`.

## Credits

- The method: [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- Explainer: askglitch, "Build a Second Brain"
