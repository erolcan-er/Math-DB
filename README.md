# Math-DB: A Discourse Framework for Mathematical Word Problems

This repository contains **Math-DB**, a discourse-annotated corpus of mathematical word problems built on the [GSM-Symbolic](https://github.com/apple/ml-gsm-symbolic) dataset. Each adjacent clause pair in a problem is labeled with a discourse relation sense drawn from a two-level hierarchy designed for quantitative reasoning (Change, Combine, Compare, Equalize, Other).

The dataset is described in our paper:

> *Math-DB: A Discourse Framework for Mathematical Word Problems to Enhance LLM Reasoning.*

## Contents

The annotations are distributed as a single Excel file, `Math-DB-Annotations.xlsx`, containing the following sheets:

| Sheet | Description |
|---|---|
| `Annotations` | The full set of 47,815 discourse relations. One row per relation, with problem ID, instance, dataset subset (`main` / `p1` / `p2`), question text, argument spans (Arg-1, Arg-2), discourse connective, and final sense label. |
| `Canonical_Templates` | Canonical relation templates aggregated per original GSM8K problem. Used to track alignment between symbolic instantiations of the same underlying problem. |
| `Dataset_Counts` | Number of annotated problem instances per subset (`main`, `p1`, `p2`). |
| `Type_Counts` | Counts of Explicit vs. Implicit discourse relations. |
| `DC_Counts` | Frequency of each explicit discourse connective. |
| `Sense_Counts` | Counts of each Level-2 sense label. |
| `Unaligned_Instances` | Source problem instances that could not be aligned to a canonical template due to a discourse-relation count mismatch. Released as metadata to support future work on robust parsing. |
| `Resegment_Failures` | Problem instances where automatic re-segmentation could not be applied. These instances are still included in `Annotations` (using the original segmentation as fallback); this sheet documents which instances were affected. |

## Corpus Statistics

The repository covers all 12,500 problem instances released in GSM-Symbolic. Of these, 11,414 are fully annotated in the `Annotations` sheet, and 1,086 (≈8.7%) are listed in `Unaligned_Instances` as instances where the automatic parser produced a discourse-relation count that did not match the canonical sense sequence for the template, preventing reliable alignment. The two sheets together account for every source instance.

| Subset | Source | Annotated | Unaligned |
|---|---:|---:|---:|
| Symbolic (base) | 5,000 | 4,621 | 379 |
| GSM-P1 | 5,000 | 4,535 | 465 |
| GSM-P2 | 2,500 | 2,258 | 242 |
| **Total** | **12,500** | **11,414** | **1,086** |

### Relation-level totals

| Statistic | Value |
|---|---|
| Discourse relations | 47,815 |
| Explicit relations | 15,011 (31.4%) |
| Implicit relations | 32,804 (68.6%) |

### Distribution of Level-1 senses

| Level-1 | Count | % |
|---|---:|---:|
| Other | 23,438 | 49.0% |
| Change | 14,891 | 31.1% |
| Combine | 7,484 | 15.7% |
| Compare | 1,498 | 3.1% |
| Equalize | 504 | 1.1% |

## Annotation Schema

Math-DB adapts the PDTB paradigm to mathematical word problems. Each adjacent clause pair `(Arg-1, Arg-2)` receives one label of the form `Level1.Level2`:

- **Change**: `Change.Increase`, `Change.Decrease` — Arg-2 updates a quantity from Arg-1 via an event over time.
- **Combine**: `Combine.PartWhole`, `Combine.Product` — Arg-1 and Arg-2 compose into a whole (additively or multiplicatively).
- **Compare**: `Compare.Difference`, `Compare.Ratio` — Arg-2 relates two quantities by difference or ratio without merging them.
- **Equalize**: `Equalize.MatchDifference`, `Equalize.MatchRatio` — Arg-2 frames a difference or ratio as the adjustment required to reach equality.
- **Other** — Narrative context or the final question clause; does not introduce a new arithmetic operation.

Full definitions, decision criteria, and worked examples are provided in Appendix B of the paper.

## Loading the data

```python
import pandas as pd

# Load the full annotations
ann = pd.read_excel("Math-DB-Annotations.xlsx", sheet_name="Annotations")

# Filter to the base GSM-Symbolic subset
main = ann[ann["Original_Dataset"] == "main"]

# Inspect one problem
print(main[main["id"] == 0][["DR_Position", "Arg-1-Text", "Arg-2-Text", "DC", "Sense"]])
```

## Citation

If you use Math-DB in your research, please cite:



(Citation will be updated upon publication.)

## License

The annotations in this repository are released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. See `LICENSE` for details.

The underlying problem text is derived from [GSM-Symbolic](https://github.com/apple/ml-gsm-symbolic) (Mirzadeh et al., 2024), which in turn builds on [GSM8K](https://github.com/openai/grade-school-math) (Cobbe et al., 2021). Please consult the licenses of those upstream datasets for terms governing the source problem text.

## Contact

For questions about the annotations or framework, please open an issue in this repository.
