# mangacap — changelog

Release docs maintained with the shiplog skill: one folder per minor line,
one doc per release, evidence bundled in `media/`.

| Line | Releases (newest first) |
| --- | --- |
| 1.0.x | [1.0.1 — Caption render fixes](1.0.x/1.0.1.md) · [1.0.0 — First release](1.0.x/1.0.0.md) |

## Releasing a change

1. Copy `template.md` from the skill to `1.0.x/<next-version>.md`, fill the
   domains the release touched, delete untouched domains, keep `* N/A` in
   kept-but-unused sections.
2. Evidence goes to `media/` (gitignore has the changelog exception), linked
   relatively from a `## 🧶 Bundled media 🧶` section.
3. Footer: Full Changelog compare range + `<!--Last commit: ...-->`.
4. Add the row above in the same commit.
