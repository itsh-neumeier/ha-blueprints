# Changelog

## v2026.05.08

### Added
- Initial repository scaffold with CI validation, conventions, and contributing guide
- `energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2`: PV surplus control for myPV ELWA2 via Victron MPPT RS450/200

## Unreleased

### Added
- `climate/victron_mppt_rs450_200_pv_surplus_heatpump`: PV-SG mode control for 1EcoDesign/Froeling heat pump via Victron MPPT RS450/200 (DE + EN)

### Changed
- `energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2` v1.2.0: add `check_interval` input — periodic state correction via time_pattern trigger (default every 30s)
- `energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2` v1.1.2: fix manual trigger check — use `trigger.platform is none` (trigger.id is Undefined not None on null trigger)
- `energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2` v1.1.1: fix manual/startup trigger (platform=null) not enabling device despite conditions met
- `energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2` v1.1.0: add `trigger_mode` input (both / limited_only / soc_only)

### Changed
- All blueprints now ship as `_de` and `_en` variants; README shows dual import badges
- Renamed `victron_mppt_rs450_200_pv_surplus_mypv_elwa2.yaml` → `_de.yaml`, added `_en.yaml`
- Updated CONVENTIONS.md with dual-language rule
