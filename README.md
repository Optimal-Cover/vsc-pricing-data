# OptimalCover VSC Pricing Dataset

> Independent, open reference data for Vehicle Service Contract (VSC / extended warranty) pricing — derived from public insurance filings and actuarial reserve disclosures.

**Maintained by:** [OptimalCover](https://optimalcover.com) — independent pricing authority for VSCs.
**Schema version:** `v3.6.7`
**License:** [CC BY 4.0](./LICENSE) — attribution required
**Mirror:** [Hugging Face Hub](https://huggingface.co/datasets/optimalcover/vsc-pricing) *(coming soon)*

---

## What this dataset contains

OptimalCover publishes pricing benchmarks for Vehicle Service Contracts based on public insurance filings, actuarial reserve disclosures, and documented industry cost structures. This repository releases that benchmark data as open, citable reference data.

**Tables included:**

| File | Description |
|------|-------------|
| `data/v3.6.7/vsc_rates.{csv,json,parquet}` | Canonical pricing rates by vehicle class × mileage band × term × deductible |
| `data/v3.6.7/vehicle_eligibility.{csv,json,parquet}` | Make/model → vehicle class (A-D) mapping with eligibility flags |
| `data/v3.6.7/mileage_bands.{csv,json,parquet}` | Mileage band definitions used by the pricing model |
| `data/v3.6.7/pricing_config.{csv,json,parquet}` | Deductible factors, schema version, methodology constants |
| `data/v3.6.7/data_dictionary.md` | Column-level documentation (types, units, allowed values) |
| `data/v3.6.7/methodology_summary.md` | Condensed methodology — how the rates are derived |
| `data/v3.6.7/CHANGELOG.md` | What changed since the previous schema version |
| `data/v3.6.7/checksums.sha256` | SHA-256 checksums for integrity verification |

**What this dataset is NOT:**
- Not a quote, offer, or sale of any product
- Not affiliated with any single VSC provider
- Does not contain customer data, VINs, or any personally identifiable information

---

## Quick start

### Python (pandas)
```python
import pandas as pd
url = "https://raw.githubusercontent.com/Optimal-Cover/vsc-pricing-data/main/data/v3.6.7/vsc_rates.csv"
rates = pd.read_csv(url)
print(rates.head())
```

### Python (Hugging Face datasets)
```python
from datasets import load_dataset
ds = load_dataset("optimalcover/vsc-pricing", "rates")  # coming soon
```

### R
```r
rates <- read.csv("https://raw.githubusercontent.com/Optimal-Cover/vsc-pricing-data/main/data/v3.6.7/vsc_rates.csv")
```

### SQL (DuckDB — no install needed)
```sql
SELECT vehicle_class, AVG(price) AS avg_price
FROM 'https://raw.githubusercontent.com/Optimal-Cover/vsc-pricing-data/main/data/v3.6.7/vsc_rates.csv'
GROUP BY vehicle_class
ORDER BY avg_price;
```

---

## Schema versioning

Releases are tagged by the OptimalCover pricing schema version (e.g. `v3.6.7`). Each schema version is an immutable snapshot — once published, its data files do not change. New methodology refinements ship as new schema versions (`v3.6.8`, `v3.7.0`, etc.) with a CHANGELOG entry.

Older versions are preserved under `data/archive/`.

---

## Citation

Please cite this dataset as:

```
OptimalCover (2026). VSC Pricing Dataset (v3.6.7).
https://github.com/Optimal-Cover/vsc-pricing-data
Licensed under CC BY 4.0.
```

BibTeX:

```bibtex
@dataset{optimalcover_vsc_2026,
  author    = {{OptimalCover}},
  title     = {VSC Pricing Dataset},
  version   = {3.6.7},
  year      = {2026},
  url       = {https://github.com/Optimal-Cover/vsc-pricing-data},
  license   = {CC-BY-4.0}
}
```

A machine-readable `CITATION.cff` is included in this repo for tooling.

---

## Methodology

Full methodology lives at [optimalcover.com/methodology](https://optimalcover.com/methodology). A condensed version ships with each release in `methodology_summary.md`.

Inputs include:
- Public insurance filings (state DOI rate filings, NAIC disclosures)
- Actuarial reserve disclosures from VSC underwriters
- Documented dealer/admin/reserve cost structures
- Vehicle reliability and parts-cost data

Outputs are reference price ranges (`REFERENCE_LOW` / `REFERENCE_HIGH`) — never single point prices, never quotes.

---

## Reporting issues / contributing

Found an inconsistency, want to suggest a methodology refinement, or have a use case to share? Open an issue on this repo. Pull requests are welcome for examples, helper scripts, and documentation. The data files themselves are generated from the OptimalCover backend and should not be edited manually.

---

## Related links

- Website: [optimalcover.com](https://optimalcover.com)
- Methodology: [optimalcover.com/methodology](https://optimalcover.com/methodology)
- Pricing reference (interactive): [optimalcover.com/pricing-bands](https://optimalcover.com/pricing-bands)
- LLM-friendly endpoint: [optimalcover.com/llms-full.txt](https://optimalcover.com/llms-full.txt)
