# gehaeuse

3D-printed enclosures for circuit boards as code, for Claude Code, GitHub Copilot CLI and
people. (*Gehäuse* is German for housing.)

- **A Python library, `gehaeuse`**, that measures housings built in
  [build123d](https://github.com/gumyr/build123d): what is inside what, how far it is to the
  next wall, how hard a gasket is squeezed, what overhangs, what a print weighs and how long
  it takes.
- **A plugin for Claude Code and GitHub Copilot CLI** with the skill `printed-enclosure`: how
  to generate a housing from a board's geometry and prove it with measured checks, and the
  mistakes those checks were built to prevent.

It is the mechanical half of a board-plus-housing workflow; the board half is
[leiterbahn](https://github.com/dwzg/leiterbahn), which writes the `board.json` and STEP a
housing is generated from. Until leiterbahn 0.10 the two were one project (`hw-design`).

## What's in it

| Part | What it does |
|---|---|
| `gehaeuse.probe` | Measure solids: point in solid, distance along a ray, vertical ray casts on a grid, a board STEP split into board and parts |
| `gehaeuse.gasket` | Shore A to modulus, force and squeeze of a round cord or a flat TPU bead (estimates) |
| `gehaeuse.printing` | Printed volume, mass and time for given walls and infill; overhang areas by location |
| `gehaeuse.render` | Headless renders of build123d scenes with `f3d` |
| skill `printed-enclosure` | Board-driven housings with measured checks; sealing, fasteners, printing |

## Requirements

build123d, numpy and scipy (installed with the package); the program `f3d` for renders.

## Install

**Plugin (Claude Code):**

```
/plugin marketplace add dwzg/gehaeuse
/plugin install gehaeuse@gehaeuse
```

**Plugin (GitHub Copilot CLI):**

```
copilot plugin marketplace add dwzg/gehaeuse
copilot plugin install gehaeuse@gehaeuse
```

**The library in a project:** `pip install -e path/to/gehaeuse`, or pin it as a submodule so
every build uses the same version:

```bash
git submodule add https://github.com/dwzg/gehaeuse tools/gehaeuse
```

```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "..", "tools", "gehaeuse", "src"))
from gehaeuse.probe import inside, edge_along
```

## Development

```bash
python3 -m venv .venv
.venv/bin/pip install -e ".[dev]"
.venv/bin/pytest -q
ruff check .
python3 scripts/gen_api.py        # after changing a public signature
claude plugin validate .
```

## Licence

MIT. See `LICENSE`.
