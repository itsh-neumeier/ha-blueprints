# Prompt: HA Integration myPV ELWA2

Verwende diesen Prompt um die vollständige Custom Component zu generieren.

---

Erstelle eine HACS-kompatible Home Assistant Custom Component für den myPV ELWA2 Heizstab.

=== GERÄT ===

Name: myPV ELWA2
Typ: Elektrischer Heizstab zur Warmwasserbereitung mit PV-Überschusssteuerung
Kommunikation: HTTP REST, lokal, kein Cloud-Zwang
Python-Abhängigkeiten: keine (aiohttp ist HA-intern vorhanden)

HTTP API des Geräts:

  Status lesen:
    GET http://<IP>/data.jsn
    Antwort (JSON):
      {
        "power":    <int>    aktuell gesetzte Leistung in W (0–3500),
        "temp1":    <float>  Temperatur Sensor 1 in °C,
        "temp2":    <float>  Temperatur Sensor 2 in °C (falls vorhanden, sonst null),
        "status":   <int>    Gerätestatus (0 = aus, 1 = läuft, 2 = Fehler),
        "boost":    <int>    Boost-Modus (0 = inaktiv, 1 = aktiv),
        "energy":   <float>  Tagesenergie in kWh
      }
    Hinweis: Feldnamen im echten Gerät ggf. per Wireshark/Browser verifizieren.
    Verwende im Code ELWA2_DATA_KEYS-Konstanten, damit Korrekturen an einer Stelle erfolgen.

  Leistung setzen:
    GET http://<IP>/control.html?power=<0-3500>
    Antwort: HTTP 200 bei Erfolg, kein Body nötig
    Gültig: power 0 (aus) bis 3500 (Maximalleistung in W)

=== REPO-STRUKTUR ===

custom_components/mypv_elwa2/
├── __init__.py
├── manifest.json
├── const.py
├── config_flow.py
├── coordinator.py
├── entity.py
├── sensor.py
├── binary_sensor.py
├── number.py
├── strings.json
└── translations/
    ├── de.json
    └── en.json
hacs.json
README.md
CHANGELOG.md
LICENSE          (MIT, Copyright 2026 Timo Neumeier)
.gitignore

Keine switch.py, keine select.py – nicht benötigt.

=== MANIFEST.JSON ===

{
  "domain": "mypv_elwa2",
  "name": "myPV ELWA2",
  "version": "1.0.0",
  "documentation": "https://github.com/itsh-neumeier/ha-mypv-elwa2",
  "issue_tracker": "https://github.com/itsh-neumeier/ha-mypv-elwa2/issues",
  "requirements": [],
  "config_flow": true,
  "codeowners": ["@itsh-neumeier"],
  "integration_type": "device",
  "iot_class": "local_polling"
}

=== HACS.JSON ===

{
  "name": "myPV ELWA2",
  "render_readme": true
}

=== ENTITIES ===

Sensors (sensor.py):
  power_setpoint   | sensor  | W     | DIAGNOSTIC  | aktuell gesetzte Leistung
  temperature_1    | sensor  | °C    | —           | Temperatur Sensor 1
  temperature_2    | sensor  | °C    | —           | Temperatur Sensor 2 (unavailable wenn null)
  energy_today     | sensor  | kWh   | —           | Tagesenergie

Binary Sensors (binary_sensor.py):
  boost_active     | binary_sensor | — | DIAGNOSTIC | Boost-Modus aktiv
  error            | binary_sensor | problem | DIAGNOSTIC | Fehler (status == 2)

Number (number.py):
  target_power     | number  | W     | —           | Leistungsvorgabe 0–3500 W
    min: 0, max: 3500, step: 50, mode: slider
    set: GET /control.html?power=<value>
    native_unit: W, device_class: power_factor (oder None)

=== COORDINATOR ===

- Klasse: ElwaCoordinator(DataUpdateCoordinator)
- Update-Intervall: konfigurierbar via OptionsFlow (default 30s, min 10s, max 300s)
- Methode _async_update_data:
    async GET /data.jsn via aiohttp.ClientSession (Timeout 10s)
    Bei Fehler: raise UpdateFailed
- Verbindungstest in config_flow: GET /data.jsn, Timeout 5s, Erfolg = HTTP 200 + valides JSON

=== CONFIG FLOW ===

Step "user":
  - host (str): IP-Adresse oder Hostname des ELWA2
  - scan_interval (int, optional, default 30): Abfrageintervall in Sekunden

Validierung:
  - Verbindungstest GET /data.jsn
  - Bei Fehler: errors["base"] = "cannot_connect"
  - Unique ID: host (IP) → _abort_if_unique_id_configured()

OptionsFlow:
  - scan_interval (int, min 10, max 300, default 30)

=== DEVICE INFO ===

  identifiers: {(DOMAIN, entry.entry_id)}
  name: "myPV ELWA2"
  manufacturer: "myPV"
  model: "ELWA2"
  configuration_url: f"http://{host}"

=== VERSIONING ===

Semantic Versioning: MAJOR.MINOR.PATCH
- manifest.json: "version": "1.0.0"
- Git Tag: v1.0.0
- CHANGELOG.md führt alle Versionen

=== CHANGELOG.MD FORMAT ===

# Changelog

## [1.0.0] - 2026-05-08
### Added
- Initial release
- Sensors: power_setpoint, temperature_1, temperature_2, energy_today
- Binary sensors: boost_active, error
- Number: target_power (0–3500 W)
- Config Flow mit Verbindungstest und OptionsFlow für Scan-Intervall

=== README.MD STRUKTUR ===

# myPV ELWA2 — Home Assistant Integration

Lokale Home Assistant Integration für den myPV ELWA2 Heizstab.
Liest Gerätedaten per HTTP-Polling und erlaubt die Leistungsvorgabe (0–3500 W) direkt aus HA.

## Features
- Leistungsvorgabe als `number`-Entity (0–3500 W, Slider)
- Temperatursensoren (Sensor 1 + 2)
- Tagesenergie in kWh
- Boost-Modus und Fehler-Status als Binary Sensor
- Konfigurierbares Polling-Intervall (10–300 s)
- Kein Cloud-Zwang, vollständig lokal

## Requirements
- Home Assistant >= 2024.1
- myPV ELWA2 im gleichen Netzwerk erreichbar

## Installation

### HACS (empfohlen)
1. HACS → Custom Repositories → `https://github.com/itsh-neumeier/ha-mypv-elwa2` → Typ: Integration
2. Integration suchen: "myPV ELWA2" → Installieren
3. HA neu starten

### Manuell
1. `custom_components/mypv_elwa2` nach `<config>/custom_components/` kopieren
2. Neu starten

## Konfiguration
Einstellungen → Geräte & Dienste → Integration hinzufügen → myPV ELWA2

| Parameter       | Default | Beschreibung                    |
|-----------------|---------|---------------------------------|
| Host            | —       | IP-Adresse des ELWA2            |
| Scan Interval   | 30      | Abfrageintervall in Sekunden    |

## Entities

| Entity            | Typ           | Einheit | Beschreibung                  |
|-------------------|---------------|---------|-------------------------------|
| power_setpoint    | sensor        | W       | Aktuell gesetzte Leistung     |
| temperature_1     | sensor        | °C      | Temperatur Sensor 1           |
| temperature_2     | sensor        | °C      | Temperatur Sensor 2           |
| energy_today      | sensor        | kWh     | Tagesenergie                  |
| boost_active      | binary_sensor | —       | Boost-Modus aktiv             |
| error             | binary_sensor | —       | Gerätefehler                  |
| target_power      | number        | W       | Leistungsvorgabe 0–3500 W     |

=== TRANSLATIONS ===

strings.json (= en.json Inhalt):
{
  "config": {
    "step": {
      "user": {
        "title": "Connect myPV ELWA2",
        "data": {
          "host": "Host (IP address)",
          "scan_interval": "Scan interval (seconds)"
        }
      }
    },
    "error": {
      "cannot_connect": "Cannot connect to device",
      "already_configured": "Device already configured"
    },
    "abort": {
      "already_configured": "Device already configured"
    }
  },
  "options": {
    "step": {
      "init": {
        "title": "myPV ELWA2 Options",
        "data": {
          "scan_interval": "Scan interval (seconds)"
        }
      }
    }
  },
  "entity": {
    "sensor": {
      "power_setpoint": { "name": "Power setpoint" },
      "temperature_1":  { "name": "Temperature 1" },
      "temperature_2":  { "name": "Temperature 2" },
      "energy_today":   { "name": "Energy today" }
    },
    "binary_sensor": {
      "boost_active": { "name": "Boost active" },
      "error":        { "name": "Error" }
    },
    "number": {
      "target_power": { "name": "Target power" }
    }
  }
}

translations/de.json:
{
  "config": {
    "step": {
      "user": {
        "title": "myPV ELWA2 verbinden",
        "data": {
          "host": "Host (IP-Adresse)",
          "scan_interval": "Abfrageintervall (Sekunden)"
        }
      }
    },
    "error": {
      "cannot_connect": "Verbindung zum Gerät nicht möglich",
      "already_configured": "Gerät bereits konfiguriert"
    },
    "abort": {
      "already_configured": "Gerät bereits konfiguriert"
    }
  },
  "options": {
    "step": {
      "init": {
        "title": "myPV ELWA2 Optionen",
        "data": {
          "scan_interval": "Abfrageintervall (Sekunden)"
        }
      }
    }
  },
  "entity": {
    "sensor": {
      "power_setpoint": { "name": "Leistungsvorgabe (aktuell)" },
      "temperature_1":  { "name": "Temperatur 1" },
      "temperature_2":  { "name": "Temperatur 2" },
      "energy_today":   { "name": "Tagesenergie" }
    },
    "binary_sensor": {
      "boost_active": { "name": "Boost aktiv" },
      "error":        { "name": "Fehler" }
    },
    "number": {
      "target_power": { "name": "Leistungsvorgabe" }
    }
  }
}

=== QUALITÄTS-VORGABEN ===

- Python 3.12+ (from __future__ import annotations überall)
- has_entity_name = True auf allen Entities
- EntityCategory.DIAGNOSTIC für: power_setpoint, boost_active, error
- Kein hardcoded UI-Text, alles über translations
- Kein print(), nur _LOGGER = logging.getLogger(__name__)
- Keine Kommentare außer einzeiligen Modul-Docstrings
- aiohttp.ClientSession nur im Coordinator, nicht in Entities
- async_write_ha_state() nicht manuell aufrufen — coordinator.async_request_refresh() nutzen
- DataUpdateCoordinator-Pattern korrekt: Entities erben von CoordinatorEntity

=== HACS-CHECKLISTE ===

- [ ] hacs.json vorhanden
- [ ] manifest.json mit version, documentation, issue_tracker, codeowners
- [ ] README.md im Root (render_readme: true)
- [ ] CHANGELOG.md
- [ ] MIT LICENSE (Copyright 2026 Timo Neumeier)
- [ ] Git Tag v1.0.0 entspricht manifest version
- [ ] Domain mypv_elwa2 kollidiert nicht mit HA Core
- [ ] Keine print()-Aufrufe
- [ ] config_flow: true in manifest
- [ ] integration_type: "device" (kein Hub)
