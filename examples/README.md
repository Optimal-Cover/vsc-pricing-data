# Examples

Jupyter notebooks demonstrating how to use the OptimalCover VSC Pricing Dataset.

| Notebook | What it shows |
|----------|---------------|
| [`01_getting_started.ipynb`](./01_getting_started.ipynb) | Load every table, inspect structure, pair `REFERENCE_LOW` / `REFERENCE_HIGH` into ranges |
| [`02_pricing_explorer.ipynb`](./02_pricing_explorer.ipynb) | Average prices by class, term-length curves, reference-range spread |
| [`03_deductible_comparison.ipynb`](./03_deductible_comparison.ipynb) | Derive `$0` and `$250` prices from the canonical `$100`, including the Class D cap |

## Running locally

```bash
pip install pandas matplotlib jupyter
jupyter notebook
```

All notebooks read data directly from this repository via `raw.githubusercontent.com`, so no local files are required.

## Try without installing

- **Google Colab** — open any notebook and prefix the GitHub URL with `https://colab.research.google.com/github/`.
- **Kaggle / Deepnote / VS Code** — paste the raw `.ipynb` URL into the notebook importer.

## License

All example code in this folder is licensed under [CC BY 4.0](../LICENSE) along with the rest of the dataset.
