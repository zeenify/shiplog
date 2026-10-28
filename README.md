# Shiplog 🗒️

A [ZCode](https://zcode.ai) folder skill that gives every codebase a
release-notes system of its own: one markdown per release inside
minor-version line folders, a "How to update" block up top, emoji domain
sections with `Added / Changed / Fixed`, honest one-line bullets, `* N/A`
where nothing landed, and a Full-Changelog compare footer.

```
changelog/
├── README.md                  ← index: release table, newest first
├── template.md                ← blank release skeleton
└── 1.0.x/                     ← one folder per minor line (feature era)
    ├── 1.0.0.md               ← one doc per release, named by version
    ├── 1.0.1.md
    └── media/                 ← bundled evidence (frame strips, comparisons)
```

## Install

Clone into your ZCode personal skills directory so it triggers in every
project:

```
git clone https://github.com/<you>/shiplog ~/.agents/skills/shiplog
```

(On ZCode, `~/.agents/skills/` is the standard cross-tool skill location;
`~/.zcode/skills/` also works and wins over `.agents` when a name exists in
both.)

## What it does

Once installed, ask for things like *"document this change"*, *"release
notes for what we just did"*, *"cut the changelog for the caption fix"* —
the skill:

1. Bootstraps `changelog/` with a template and index if the project has none,
   picking a domain menu that matches the project's own pipeline anatomy.
2. Copies the template into the next patch version, fills only the touched
   domain sections (imperative one-liners, code identifiers in backticks,
   the *why* inline, `(user note)` attribution), prunes the untouched ones.
3. Writes the teammate-facing **How to update** block (re-runs, purges,
   migrations), bundles evidence into `media/` with a narrow `.gitignore`
   exception so images version even in media-heavy repos, and closes with a
   **Full Changelog** compare range + tracker comment.
4. Adds the release row to the index and commits the changelog by itself.

Versioning: patch per release, all inside the current minor line folder;
cutting a new line (`1.1.x/`) marks a feature era and is the user's call.

## Layout

```
shiplog/
├── SKILL.md                   ← the skill (loaded when triggered)
├── references/
│   └── conventions.md         ← deep rules: naming, phrasing, domain menus,
│                                media/gitignore, compare links, anti-patterns
├── assets/
│   └── template.md            ← the blank release skeleton
├── examples/
│   └── mangacap/              ← a complete worked changelog (2 releases,
│       └── 1.0.x/                index, bundled media)
└── LICENSE
```

## License

MIT — see [LICENSE](LICENSE).
