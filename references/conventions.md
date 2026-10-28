# Shiplog conventions

The deep rules behind the release-doc format. The format was forged on a live content pipeline and hardened release after
release. On top of a bare changelog it adds two things: bundled evidence
media, and a tell-the-truth attribution style. A
complete worked example lives in [../examples/mangacap/](../examples/mangacap/).

## Naming and versioning

- Line folder = **minor version line**. `1.0.x/` holds every release of the
  1.0 line. Open the next line (`1.1.x/`) only on a feature-era boundary the
  user signs off on — "first release of the 1.1 line" is the milestone, not
  every change.
- Release file = **patch version**. One doc per user-visible change batch:
  `1.0.2.md`, `1.0.3.md`, … Patch numbers are chronological within the line
  and never reused.
- Doc content is append-only history. Wrong or missing information is
  corrected in the NEXT release ("Fix 1.0.1 note: truncation also affects
  RGBA exports"), not by editing the old doc.

## The skeleton, section by section

The `assets/template.md` in this skill is the canonical skeleton. Filled
docs keep only the domain sections they use; inside a kept domain, an unused
subsection carries a `* N/A` bullet — that is the shiplog signal of
"checked, nothing to report". Never leave an empty section silent: an absent
section reads as "unknown", an explicit N/A reads as "verified none".

- **How to update** (top, fenced block): the teammate-facing apply steps.
  Always starts with `git pull origin main` (or the project's fetch
  equivalent), then any step that makes bytes take effect: re-render
  commands, cache-purge globs, cache rebuilds, a migration script (mark
 those bullets with the 📜 glyph — the shiplog "uses a migration
  script" marker). If the change only matters to fresh clones, say so.
- **Domain sections**: `## <emoji> Domain <emoji>` headers, `### Added /
  ### Changed / ### Fixed` subsections. Removed functionality is written as
  a `Changed` bullet starting with "Remove …" — shiplog keeps
  three verbs, removals live inside Changed.
- **Full Changelog footer**: a GitHub compare range URL plus the hidden
  tracker comment. When several same-day commits form one release, the
  compare's base is the commit just before the release's first commit, and
  the tracker comment keeps the last one:

  ```
  **Full Changelog**: https://github.com/<owner>/<repo>/compare/31fc53e...2a882d8

  <!--Last commit: 2a882d8-->
  <!--Used to keep track of the last commit merged in case new ones come in before the changelog is done.-->
  ```

  On repos with no remote yet, drop the URL and list the hashes plainly;
  add the URL when the remote lands.

## Domain menus

Domains are the project's own anatomy, not a fixed list. Shiplog's default
menu suits a media/render pipeline:

`🌋 Refactors 🌋` (📜 = needs a re-run), `💥 Breakage / Render-fail fixes 💥`,
`🔍 Research 🔍`, `🧬 General 🧬`, `🎬 Shorts renderer 🎬`, `🎞️ Film lane 🎞️`,
`📚 Documentation 📚`, `🧹 Other cleanup 🧹`, `🧶 Bundled media 🧶`.

On other domains: a feature-heavy codebase might split its menu into
`🧑 Plasma UI (feature branch)`-style buckets — e.g. a web app could use
`🎨 Frontend`, `🔌 API`, `🗃️ Data & Migrations`, `🔐 Auth`, `🧪 Tests`;
keep bucket names anchored to the project's own anatomy.

When bootstrapping a changelog for a NEW project, read its top-level dirs and
pick 5–9 domains that split its surface area into easily-predictable buckets;
write the menu into the project's `changelog/template.md` and never shuffle
it between releases (stability is what makes the index readable).

## Bullet style

- Imperative, one line, one change. "Add x-start/x-end crop windows to
  toonkit crop", not "Added support for the new cropping system".
- Identifiers in backticks: files, functions, flags (`\fsp`,
  `tools/assemble_shorts.py`).
- The *why* rides on the same line after an em dash or parenthetical when
  the change isn't self-evident: "Fix cumulative A/V rounding drift (up to
  ~0.2s by end of short) that made captions trail the narration".
- Attribute provenance: "(user note)" for feedback-driven fixes; borrowed
  format studies name the source ("60fps frame study of @AkielRunes").
- No marketing voice. "Powerful new export pipeline" is nothing; "Export
  pipeline writes EXIF-preserving PNGs 3× faster" is a line.
- User-facing effect beats internal mechanics as the bullet's subject. Lead
  with what a viewer/user notices, mechanics second.

## Media bundling

- Evidence goes to `changelog/<line>.x/media/<descriptive-name>.<ext>` —
  names like `captions-before-after.jpg`, not `Screenshot 2026-10-28.png`.
  Frame strips (a dozen labelled consecutive frames) are the gold standard
  for any motion change: they defend the change better than the sentence.
- Link relatively from the release doc (`media/captions-before-after.jpg`),
  typically under a `## 🧶 Bundled media 🧶` section, one italic caption per
  image telling the reader what to look for.
- If images are gitignored repo-wide (common for media projects), add the
  narrow exception — changelog evidence is versioned on purpose:

  ```gitignore
  # ---- except bundled changelog evidence (small, versioned on purpose) ----
  !changelog/**/*.jpg
  !changelog/**/*.jpeg
  !changelog/**/*.png
  ```

## Index (changelog/README.md)

One table, newest line first, newest release first within a line:

```md
| Line | Releases (newest first) |
| --- | --- |
| 1.0.x | [1.0.1 — Shorts captions rebuild](1.0.x/1.0.1.md) · [1.0.0 — Film lane](1.0.x/1.0.0.md) |
```

Keep a short "Releasing a change" recipe in the index too — the next agent
(or you, six months from now) reads only the index and produces the right
thing. The recipe names: the next-version file rule, the template copy step,
the prune-or-N/A rule, the media rule, and the commit-together rule.

## Version-soup anti-pattern

Do not label releases "v2", "v3" of a feature. Versions belong to the
template's `<major.minor.patch>`, not to prose. The caption rebuild that
took three same-day passes is ONE release (`1.0.1`), and its footer's
compare range carries all three commits. Prose version numbers multiply
ambiguity ("house style v4") — date the style instead ("the 2026-10-28
house caption style") or name it ("the dead-center block style").
