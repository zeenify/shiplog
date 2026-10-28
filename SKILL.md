---
name: shiplog
description: |
  Maintain a versioned changelog for a codebase in the shiplog
  release-notes style: `changelog/<line>.x/<ver>.md` release docs with emoji
  domain sections, Added/Changed/Fixed bullets, a How-to-update block, and
  bundled evidence media. Use whenever the user mentions changelog, release
  notes, patch notes, update log, version history, "document this change",
  "what did we change", cutting a release or a version, or wants changes of
  the code base recorded — even if they never say the word "changelog".
---

# Shiplog

Turn codebase changes into clean, versioned release documents: line
folders, one markdown per release, emoji domain sections, honest one-line
bullets. The format was forged on a real media-render pipeline where every
release had to survive skim-over scrutiny from a non-technical producer —
demanding, scannable, and impossible to fake.

Full rules live in [references/conventions.md](references/conventions.md); a
complete worked changelog lives in [examples/changelog-repo/](examples/changelog-repo/) —
read the conventions file before your first fill, the example when the shape
of a release doc is unclear.

## The layout you maintain

```
changelog/
├── README.md                  ← index: one table of release lines, newest first
├── template.md                ← blank release skeleton (copied per release)
└── 1.0.x/                     ← one folder per minor line (feature era)
    ├── 1.0.0.md               ← one doc per release, named by version
    ├── 1.0.1.md
    └── media/                 ← bundled evidence images for this line's docs
```

## The rhythm

- **Every user-visible change batch gets a release doc.** Small change →
  small doc; never skip one because it feels minor. Related same-day
  iterations of one idea are ONE release — list the commit trail in its
  footer, don't split into version-soup.
- **Patch bumps per release** (`1.0.2.md`, `1.0.3.md`, …), all inside the
  current line folder. Cutting a NEW line folder (`1.1.x/`) marks a feature
  era — that call belongs to the user; suggest it, don't unilaterally cut.
- Release docs are append-only history. Never rewrite an old release doc;
  if information was wrong, the next release says so.

## Filling a release doc

1. Copy `assets/template.md` from this skill into
   `changelog/<line>.x/<next-version>.md`.
2. **How to update block** (top, fenced): what a teammate must do to get and
   apply the change — `git pull` plus any re-run/purge/migration commands,
   or a note that re-runs aren't needed.
3. **Fill only the domains the release touched.** Bullets are imperative
   ("Add cache eviction", "Fix captions trailing audio late in the video"),
   one line each, functions/files in backticks, and the *why* rides on the
   same line when it isn't obvious. Attribute user feedback as "(user note)".
4. **Prune or N/A.** Delete domain sections the release never touches; keep a
   kept-but-unused section with a `* N/A` bullet. Follow the project's
   existing docs if they already established a domain list — adapt the
   domain emojis/names to the project's own pipeline and stay consistent
   across releases.
5. **Full Changelog footer**: a GitHub compare range
   (`https://github.com/<owner>/<repo>/compare/<prev-ref>...<this-ref>`) plus
   the hidden `<!--Last commit: <hash>-->` tracker comment.
6. **Index row**: add the release to `changelog/README.md` (newest first) in
   the same commit as the doc.

## Bundled media

Evidence that deserves to survive (frame strips, before/after comparisons,
render stills) goes into `changelog/<line>.x/media/` with descriptive names,
linked from a `## 🧶 Bundled media 🧶` section with relative links. In this
repo tmp/ dirs are scratch — never reference or ship anything from there.
If the project's `.gitignore` excludes images repo-wide, add a narrow
exception so changelog evidence still version-control:

```gitignore
# ---- except bundled changelog evidence (small, versioned on purpose) ----
!changelog/**/*.jpg
!changelog/**/*.jpeg
!changelog/**/*.png
```

## House rules

- Changelog commits contain changelog files only; the code change itself is
  committed separately first.
- No marketing voice, no adjectives-of-pride ("powerful", "beautiful") —
  state what changed and what it fixes.
- Never delete or reorder past release docs to "clean up" — history stays
  put.
- If the user asks to bundle changes under a version, that means the *line
  folder*; release docs inside stay separate per change batch.
