# Weekly digest

Copy the block below into your scheduler, to run on Sundays. Fill in `{{VAULT}}`.

---

Generate the weekly digest for the vault at `{{VAULT}}`.

1. Compute the current ISO week (`date +%G-W%V`).
2. Read `journal/AAAA-MM-DD.md` from the last 7 days (Monday to Sunday) and, if it exists, the previous week's digest in `weekly/`.
3. Write `weekly/AAAA-Www.md` (ISO week) with frontmatter (`tags: [weekly]`, `week: AAAA-Www`, `date:` Sunday's date) and sections: "What was done" (grouped by area), "Decisions and why", "Learnings", "Pending and next steps". Concise bullets; link the relevant `knowledge/` and `projects/` notes with wikilinks.
4. If a learning from the week is durable, extend or create the corresponding `knowledge/` note and link it from the digest.
5. Update the index (`Home.md`) only if there are new important notes.
6. If the vault uses a sync backend, verify its status before finishing.

The digest synthesizes; it never duplicates the journal. Link notes instead of copying their text.

Your final answer (if delivered to the user) is a 5-8 line summary of the week, not the full digest.
