# driver-template

The reference shape for every `driver-*` repo in the **renewvan** ecosystem: a pure-function core, a
layered `config.default.ini` / `config.ini` loader, a persistent MQTT publisher with LWT that publishes
retained Driver info before `online`, a thin publish loop, a systemd unit, a Dockerfile and CI (tests,
manifest gate, release). Contract: `hub/docs/authoring-a-driver.md`.

This repo is the single source of truth for that skeleton. `driver-tank` and the other drivers are
real devices that happened to be built first; new drivers start here.

## Layout

| Path                  | What                                                                          |
| --------------------- | ----------------------------------------------------------------------------- |
| `template/`           | The tokenized skeleton. File names and contents carry `__TOKEN__` placeholders; `.tmpl` suffixes are stripped on render. |
| `scripts/scaffold.py` | Renders `template/` into a new repo directory.                                |
| `tests/`              | Renders a driver and runs its own tests, so the template cannot rot.          |

Tokens: `__ENTITY__`, `__ENTITY_TITLE__`, `__PACKAGE__`, `__REPO__`, `__DRIVER__`, `__DESCRIPTION__`,
`__YEAR__`.

## Use

Normally through the `renewvan-create-driver` skill (`renewvan/skills`), which also runs the design round.
By hand:

```bash
python3 scripts/scaffold.py <entity-slug> "<Entity Title>" "<description>" ../driver-<entity>
```

The generated `driver.py` ships a deliberate `NotImplementedError`; design and implementation are the
next step.

## Development

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements-test.txt
pytest tests/ -v
```

A change to the skeleton (including the pinned `renewvan/hub` reusable-workflow SHA in
`template/.github/workflows/`) lands here, not in the skill.
