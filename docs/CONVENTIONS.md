# Blueprint Conventions

## File Naming

`lowercase_snake_case.yaml`, pattern: `<vendor_or_source>_<function>_<target>.yaml`

Example: `victron_mppt_rs450_200_pv_surplus_mypv_elwa2.yaml`

## Required Fields in `blueprint:` Block

| Field | Value |
|-------|-------|
| `name` | Schema: `<Quelle> <Funktion> <Ziel>` (German) |
| `description` | `>-` multiline, German, explains both trigger conditions AND actions |
| `domain` | Matches subdirectory (`automation` / `script` / `template`) |
| `source_url` | Raw GitHub URL to file on main branch |
| `author` | `Timo Neumeier` |

## Input Style

- Use a fitting `selector` (entity with `domain:` filter, `number`, `duration`, `select`)
- Add `description` for every non-trivial input
- Set sensible `default` values wherever possible
- Order: required entities → thresholds/mode → timing parameters

## Versioning

First line of every file: `# version: X.Y.Z`

- Breaking change → bump major
- New input or behavior change → bump minor
- Fix → bump patch

## Active Categories

| Directory | Topics |
|-----------|--------|
| `automation/energy/` | PV surplus control, Victron MPPT, battery SOC logic, load management |
| `automation/climate/` | Heat pump control (PV-SG mode), heating rod variants, HVAC, ventilation |
| `automation/lighting/` | Light automations |
| `automation/notifications/` | Telegram/push notification patterns |
| `automation/presence/` | Presence detection automations |
| `automation/household/` | Family briefing, calendar triggers, waste collection |
| `automation/security/` | Security automations |
| `script/` | Reusable script blueprints |
| `template/` | Template blueprints |

Add new categories here and in `README.md` when needed.
