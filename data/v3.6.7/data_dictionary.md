# Data Dictionary — Schema v3.6.7

This document describes every column in every table of the OptimalCover VSC Pricing Dataset (v3.6.7). All values are non-PII, methodology-derived reference data.

---

## `vsc_rates`

Canonical reference prices for Vehicle Service Contracts at the authoritative $100 deductible. One row per `(vehicle_class, mileage_band, term, band)` combination.

| Column | Type | Description | Allowed / example values |
|--------|------|-------------|---------------------------|
| `coverage_level` | string | Coverage tier the rate applies to | `EXCLUSIONARY` (current release covers exclusionary only) |
| `vehicle_class` | string | Risk class assigned to the vehicle | `A`, `B`, `C`, `D` (A = highest reliability / lowest price, D = lowest reliability / highest price) |
| `mileage_band` | string | Label of the mileage band at purchase | e.g. `"0 - 48,000"`, `"48,001 - 60,000"` — see `mileage_bands` table |
| `term` | string | Term length label | e.g. `"24 / 24,000"`, `"48 / 48,000"` — months / miles |
| `deductible` | integer | Per-claim deductible, in USD | Always `100` in this dataset. Other deductibles (`$0`, `$250`) are derived at runtime per `pricing_config.deductible_handling` |
| `band` | string | Which end of the reference range this row represents | `REFERENCE_LOW` or `REFERENCE_HIGH` |
| `price` | integer | Reference price, in USD, rounded to nearest $50 | e.g. `1450` |
| `term_months` | integer | Term length in months | e.g. `24`, `36`, `48`, `60`, `72` |
| `term_miles` | integer | Term length in miles | e.g. `24000`, `48000`, `72000` |

**How to use:** pair `REFERENCE_LOW` and `REFERENCE_HIGH` rows sharing the same `(vehicle_class, mileage_band, term_months, deductible)` to form the reference price range for that scenario.

---

## `vehicle_eligibility`

Make/model → vehicle class (A–D) mapping plus eligibility status.

| Column | Type | Description | Allowed / example values |
|--------|------|-------------|---------------------------|
| `vehicle` | string | Display name of the make | e.g. `"Chevrolet"`, `"BMW"` |
| `normalized_vehicle` | string | Uppercase normalized name used for lookups | e.g. `"CHEVROLET"` |
| `vehicle_class` | string \| null | Risk class assigned to the make | `A`, `B`, `C`, `D`, or `null` if ineligible |
| `eligibility` | string | Whether any model under this make can be priced | `ELIGIBLE`, `INELIGIBLE` |
| `models` | JSON-encoded array of strings | Model names covered by this row | e.g. `["1500","2500","3500"]`. In CSV/Parquet this is a JSON string; in the JSON file it is a native array |
| `note` | string \| null | Optional note (model exceptions, special handling, etc.) | free-text |

**What is NOT eligible (per methodology):** vehicles used commercially, older than 15 years, over 84,000 miles, 10/12-cylinder engines, EVs, and hybrids. These exclusions are enforced by the pricing engine, not encoded row-by-row here.

---

## `mileage_bands`

Definitions of the mileage bands referenced by `vsc_rates.mileage_band`.

| Column | Type | Description |
|--------|------|-------------|
| `label` | string | Human-readable band label (matches `vsc_rates.mileage_band`) |
| `min_odometer` | integer | Inclusive lower bound of the band, in miles |
| `max_odometer` | integer | Upper bound of the band, in miles |
| `max_inclusive` | boolean | `true` if the upper bound is inclusive (i.e. `<= max_odometer`) |

**Current bands (v3.6.7):** `0 - 48,000`, `48,001 - 60,000`, `60,001 - 72,000`, `72,001 - 84,000`.

---

## `pricing_config`

Methodology constants encoded as JSONB key/value pairs. One row per configuration key.

| Column | Type | Description |
|--------|------|-------------|
| `config_key` | string | Name of the configuration block |
| `config_value` | JSON object (in `.json`) / JSON-encoded string (in `.csv` / `.parquet`) | The configuration payload — structure varies by `config_key` |

### Configured keys

| `config_key` | Purpose |
|---|---|
| `schema_version` | The canonical schema version this release corresponds to (e.g. `3.6.7`) |
| `deductible_handling` | How non-canonical deductibles (`$0`, `$250`) are derived from the canonical `$100` prices — multipliers, rounding, caps |
| `constraints` | Class/term caps (e.g. Class D can only be priced up to 60 months under certain mileage bands) and extrapolation policy |
| `eligibility_constraints` | Scope rules — e.g. `max_vehicle_age_years = 15` |
| `rounding_policy` | Rules for rounding derived prices to $50 increments |

---

## Integrity

Every file in this release has a SHA-256 checksum in `checksums.sha256`. Verify with:

```
sha256sum -c checksums.sha256
```

---

## Source of truth

This data is exported from the OptimalCover production pricing database (Supabase, v3.6.7 shadow tables). The export pipeline lives in the backend repo (`scripts/exportOpenData.ts`) and runs against the canonical `*_v367` tables.

Questions or discrepancies? Open an issue at [github.com/Optimal-Cover/vsc-pricing-data](https://github.com/Optimal-Cover/vsc-pricing-data).
