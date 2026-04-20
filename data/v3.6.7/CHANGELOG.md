# Changelog — Schema v3.6.7

All notable changes to the OptimalCover VSC Pricing Dataset will be recorded in this file.

The dataset is versioned by **pricing methodology schema version** (not release date). A new entry is added when the schema bumps.

Format loosely follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [3.6.7] — 2026-04-17

**Initial public release.**

This is the first public open-data release of the OptimalCover VSC Pricing Dataset. There is no prior published schema to diff against — every row, column, and configuration key here is new to the public record.

### Added

- **`vsc_rates`** — 180 canonical reference-price rows covering:
  - 4 vehicle classes (`A`, `B`, `C`, `D`)
  - 4 mileage bands (`0 - 48,000`, `48,001 - 60,000`, `60,001 - 72,000`, `72,001 - 84,000`)
  - Term lengths from 24 / 24,000 through 72 / 72,000 (months / miles)
  - Canonical $100 deductible only; `$0` and `$250` deductibles are derived at runtime per `pricing_config.deductible_handling`
  - Two bands per scenario: `REFERENCE_LOW` and `REFERENCE_HIGH`
- **`vehicle_eligibility`** — 104 make-level eligibility rows with risk-class assignment (A–D) and included model lists.
- **`mileage_bands`** — 4 mileage-band definitions with inclusive/exclusive bound metadata.
- **`pricing_config`** — 5 configuration keys:
  - `schema_version` = `3.6.7`
  - `deductible_handling` — `$0` = `1.35×` (with absolute `+$500` cap on Class D), `$250` = `0.85×`
  - `constraints` — Class D term caps per mileage band; extrapolation disallowed
  - `eligibility_constraints` — `max_vehicle_age_years = 15`
  - `rounding_policy` — nearest `$50`, collisions allowed
- **Distribution formats:** CSV, JSON, and Parquet for every table.
- **`data_dictionary.md`** — column-level documentation for all four tables.
- **`methodology_summary.md`** — condensed methodology (full version at [optimalcover.com/methodology](https://optimalcover.com/methodology)).
- **`checksums.sha256`** — SHA-256 integrity hashes for every data and metadata file in this release.

### Coverage scope (v3.6.7)

Only **exclusionary** coverage is priced in this release. Stated-component coverage is planned for a future schema version.

The following are **out of scope** and have no pricing rows:

- Vehicles older than 15 years
- Vehicles over 84,000 miles at purchase
- Commercial / business use
- 10-cylinder and 12-cylinder engines
- Fully electric vehicles
- Hybrid powertrains

### Notes

- All prices are rounded to the nearest `$50` per the `rounding_policy`.
- JSONB-typed columns (`vehicle_eligibility.models`, `pricing_config.config_value`) are stored as native arrays/objects in the `.json` files and as JSON-encoded strings in `.csv` and `.parquet` for compatibility with tabular readers.
- Data was exported from the OptimalCover production pricing database (Supabase shadow tables `*_v367`) via `scripts/exportOpenData.ts` in the backend repository.
