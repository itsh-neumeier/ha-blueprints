# Blueprint Conventions

## File Naming

`lowercase_snake_case_<lang>.yaml`, pattern: `<vendor_or_source>_<function>_<target>_<lang>.yaml`

Every blueprint ships as two files — one German (`_de`) and one English (`_en`):

```
victron_mppt_rs450_200_pv_surplus_mypv_elwa2_de.yaml
victron_mppt_rs450_200_pv_surplus_mypv_elwa2_en.yaml
```

## Dual-Language Rule

- `_de` file: `name`, `description`, and all input `name`/`description` fields in **German**
- `_en` file: same fields translated to **English**
- All logic (templates, triggers, conditions, actions) is **identical** between both files
- Version number must be **in sync** between both files — bump both together

## Required Fields in `blueprint:` Block

| Field | Value |
|-------|-------|
| `name` | Schema: `<Source> <Function> <Target>` (language-specific) |
| `description` | `>-` multiline, language-specific, explains trigger conditions AND actions |
| `domain` | Matches subdirectory (`automation` / `script` / `template`) |
| `source_url` | Raw GitHub URL to **this specific file** (including `_de`/`_en` suffix) on main branch |
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
- Both `_de` and `_en` always carry the same version

## README Table Format

Each blueprint gets one row with two import badge columns:

```markdown
| [Name DE](path_de.yaml) | Description | [![Import DE](...badge...)](... _de url ...) | [![Import EN](...badge...)](... _en url ...) |
```

Column headers: `| Blueprint | Description | Import DE | Import EN |`

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
