# ha-blueprints

Home Assistant Blueprints by Timo Neumeier. Community-usable automations for energy management, climate control, and household tasks.

Each blueprint is available in German (DE) and English (EN).

## Installation

### Via HACS (Custom Repository)

1. Open HACS → **Blueprints** → ⋮ → **Custom repositories**
2. URL: `https://github.com/itsh-neumeier/ha-blueprints` · Type: **Blueprint**
3. Download the desired blueprint from HACS

### Manual Import

Click the badge next to a blueprint to open the import dialog directly in your Home Assistant instance.

---

## Blueprints

### Energy

| Blueprint | Description | Import DE | Import EN |
|-----------|-------------|-----------|-----------|
| [Victron MPPT RS450/200 PV-Überschuss Freigabe Heizstab MYPV ELWA2](blueprints/automation/energy/victron_mppt_rs450_200_pv_surplus_mypv_elwa2_de.yaml) | PV-Überschuss-Steuerung für myPV ELWA2 via Victron MPPT RS450/200. Gibt Heizstab frei wenn Batterie-SOC über Schwellwert und MPPT im LIMITED-Modus. | [![Import DE](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fitsh-neumeier%2Fha-blueprints%2Fmain%2Fblueprints%2Fautomation%2Fenergy%2Fvictron_mppt_rs450_200_pv_surplus_mypv_elwa2_de.yaml) | [![Import EN](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fitsh-neumeier%2Fha-blueprints%2Fmain%2Fblueprints%2Fautomation%2Fenergy%2Fvictron_mppt_rs450_200_pv_surplus_mypv_elwa2_en.yaml) |

### Climate

| Blueprint | Description | Import DE | Import EN |
|-----------|-------------|-----------|-----------|
| [Victron MPPT RS450/200 PV-Überschuss Wärmepumpe 1EcoDesign](blueprints/automation/climate/victron_mppt_rs450_200_pv_surplus_heatpump_de.yaml) | PV-SG-Modus-Steuerung einer 1EcoDesign/Froeling Wärmepumpe via Victron MPPT RS450/200. Mit konfigurierbarer Haltezeit nach Überschusswegfall. | [![Import DE](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fitsh-neumeier%2Fha-blueprints%2Fmain%2Fblueprints%2Fautomation%2Fclimate%2Fvictron_mppt_rs450_200_pv_surplus_heatpump_de.yaml) | [![Import EN](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fitsh-neumeier%2Fha-blueprints%2Fmain%2Fblueprints%2Fautomation%2Fclimate%2Fvictron_mppt_rs450_200_pv_surplus_heatpump_en.yaml) |

### Lighting

*Coming soon.*

### Notifications

*Coming soon.*

### Presence

*Coming soon.*

### Household

*Coming soon.*

### Security

*Coming soon.*

---

## License

[MIT](LICENSE) © 2026 Timo Neumeier
