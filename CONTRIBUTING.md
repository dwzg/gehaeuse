# Contributing

## Layout

```
src/gehaeuse/        the library (needs build123d, numpy, scipy; render.py also the program f3d)
tests/               pytest; tests needing build123d skip themselves
skills/<skill>/      SKILL.md plus reference files, loaded on demand
.claude-plugin/      plugin manifest and its one-entry marketplace
scripts/gen_api.py   regenerates skills/printed-enclosure/api.md from the source
```

The repository root is the plugin, so an installed plugin carries the library with it.

## Rules

- **Keep the library project-agnostic.** No dimensions or part names from a particular
  housing in `src/`. Projects pass those in.
- **Measure the solid, not the intent.** A check reads the generated geometry, never the
  parameters it was built from.
- **Behaviour changes need a test** on a solid with a known answer.
- **Skills state what happened.** Each rule in a skill says what broke without it. A rule
  without a reason gets deleted by the next person who doesn't understand it.
- **After changing a public signature**, run `python3 scripts/gen_api.py`. CI fails when
  `api.md` is out of date.

## Releases

Semantic versions, in both `pyproject.toml` and `.claude-plugin/plugin.json`. Record changes
in `CHANGELOG.md`.

## Checks

```bash
ruff check . && pytest -q && claude plugin validate .
```

CI runs the same checks on every push and pull request.
