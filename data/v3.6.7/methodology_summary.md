# Methodology Summary — Schema v3.6.7

> This is a condensed summary. The authoritative, full methodology lives at [optimalcover.com/methodology](https://optimalcover.com/methodology).

---

## What these rates represent

**Reference price ranges** for Vehicle Service Contracts (VSCs / extended warranties), derived from public insurance filings, actuarial reserve disclosures, and documented industry cost structures. Every price is a **range**, never a point. Every price is a **reference**, never a quote.

This dataset is what OptimalCover publishes as the independent pricing authority for VSCs. It is not a sale offer, not affiliated with any single underwriter, and not a dealer price sheet.

---

## Inputs

1. **State Department of Insurance filings.** Publicly filed rate forms from VSC underwriters across US states.
2. **NAIC disclosures.** Statutory reserve and loss-ratio disclosures.
3. **Industry cost structures.** Documented benchmarks for dealer commission, administrator fees, reinsurance, and claim reserves.
4. **Vehicle reliability data.** Aggregated failure-rate and parts-cost signals used to assign vehicles into risk classes A–D.

All inputs are public or available under license. No consumer, VIN, or transaction-level data is used.

---

## How rates are computed

1. **Raw loss-cost curves** are built from filing data per vehicle class × mileage band × term.
2. **Admin/reserve/commission load** is applied using documented industry cost structures.
3. **Upper and lower reference bounds** (`REFERENCE_HIGH`, `REFERENCE_LOW`) are published to reflect the spread between competitively priced offers and typical dealer markup.
4. **All canonical rates are computed at the $100 deductible.** Other deductibles (`$0`, `$250`) are derived at runtime from the canonical price using documented multipliers and caps — see `pricing_config.deductible_handling`.
5. **Prices are rounded to the nearest $50** per `pricing_config.rounding_policy`.

---

## Risk classes

| Class | Meaning | Confidence |
|-------|---------|------------|
| A | Highest reliability, lowest expected claim cost | HIGH |
| B | Above-average reliability | HIGH |
| C | Average reliability | MEDIUM |
| D | Below-average reliability, highest expected claim cost | LOW |

Class D is subject to additional term caps and deductible adjustments (see `pricing_config.constraints`).

---

## Eligibility scope (v3.6.7)

The following are **out of scope** for this dataset and will not have pricing rows:

- Vehicles older than 15 years (`eligibility_constraints.max_vehicle_age_years`)
- Vehicles over 84,000 miles at purchase (see `mileage_bands`)
- Commercial/business use
- 10-cylinder and 12-cylinder engines
- Fully electric vehicles
- Hybrid powertrains

---

## What changed in v3.6.7

See `CHANGELOG.md` for the diff from the previous schema version.

---

## Coverage tier

This release covers **exclusionary** coverage only (`coverage_level = EXCLUSIONARY`). Exclusionary coverage lists what is NOT covered; everything else is in-scope. Stated-component coverage will be added in a future release.

---

## Deductibles

- **Canonical: `$100`.** This is the deductible OptimalCover publishes as authority. All stored rates are at $100.
- **`$0` deductible** prices are derived at runtime using a `1.35×` multiplier, with an absolute `+$500` cap for Class D vehicles.
- **`$250` deductible** prices are derived using a `0.85×` multiplier.
- All derivations are rounded to the nearest $50.

See `pricing_config.deductible_handling` for full rules.

---

## Known limitations

- Rates are derived from publicly filed data. A specific dealer's quote may differ — that is expected, and is precisely why a reference range is useful.
- Regional variation within the US is not modeled at the row level (filings are aggregated nationally for the current release).
- The dataset does not cover vehicles outside the eligibility scope above.
- Reference ranges assume a reasonable, good-faith buyer; they are not warranties about market-lowest or market-highest prices.

---

## Full methodology

Complete methodology — including source citations, statistical approach, and versioning policy — is published at:
- **Web:** [optimalcover.com/methodology](https://optimalcover.com/methodology)
- **Whitepaper:** [optimalcover.com/whitepaper](https://optimalcover.com/whitepaper)
- **AI-readable:** [optimalcover.com/llms-full.txt](https://optimalcover.com/llms-full.txt)
