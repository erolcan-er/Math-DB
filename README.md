# Math-DB: A Discourse Framework for Mathematical Word Problems

This repository contains **Math-DB**, a discourse-annotated corpus of mathematical word problems built on the [GSM-Symbolic](https://github.com/apple/ml-gsm-symbolic) dataset. Each adjacent clause pair in a problem is labeled with a discourse relation sense drawn from a two-level hierarchy designed for quantitative reasoning (Change, Combine, Compare, Equalize, Other).

The dataset is described in our paper:

> *Math-DB: A Discourse Framework for Mathematical Word Problems to Enhance LLM Reasoning.

## Contents

The annotations are distributed as a single Excel file, `GSM_consolidated_annotations.xlsx`, containing the following sheets:

| Sheet | Description |
|---|---|
| `Annotations` | The full set of 47,815 discourse relations. One row per relation, with problem ID, instance, dataset subset (`main` / `p1` / `p2`), question text, argument spans (Arg-1, Arg-2), discourse connective, and final sense label. |
| `Canonical_Templates` | Canonical relation templates aggregated per original GSM8K problem (1,061 templates). Used to track alignment between symbolic instantiations of the same underlying problem. |
| `Dataset_Counts` | Number of annotated problem instances per subset (`main`, `p1`, `p2`). |
| `Type_Counts` | Counts of Explicit vs. Implicit discourse relations. |
| `DC_Counts` | Frequency of each explicit discourse connective. |
| `Sense_Counts` | Counts of each Level-2 sense label. |
| `Unaligned_Instances` | Problem instances that could not be aligned to a canonical template (e.g., due to discourse-relation count mismatch). |
| `Resegment_Failures` | Problem instances where automatic re-segmentation could not be applied. |

## Corpus Statistics

| Statistic | Value |
|---|---|
| Annotated problems | 11,414 |
| Discourse relations | 47,815 |
| Explicit relations | 15,011 (31.4%) |
| Implicit relations | 32,804 (68.6%) |
| Subsets | Symbolic (4,621), GSM-P1 (4,535), GSM-P2 (2,258) |

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
ann = pd.read_excel("GSM_consolidated_annotations.xlsx", sheet_name="Annotations")

# Filter to the base GSM-Symbolic subset
main = ann[ann["Original_Dataset"] == "main"]

# Inspect one problem
print(main[main["id"] == 0][["DR_Position", "Arg-1-Text", "Arg-2-Text", "DC", "Sense"]])
```

## Citation

If you use Math-DB in your research, please cite:

```bibtex
@inproceedings{mathdb2025,
  title     = {Math-DB: A Discourse Framework for Mathematical Word Problems to Enhance LLM Reasoning},
  author    = {Anonymous},
  booktitle = {Proceedings of ACL},
  year      = {2025}
}
```

(Citation will be updated upon publication.)

## License

The annotations in this repository are released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. See `LICENSE` for details.

The underlying problem text is derived from [GSM-Symbolic](https://github.com/apple/ml-gsm-symbolic) (Mirzadeh et al., 2024), which in turn builds on [GSM8K](https://github.com/openai/grade-school-math) (Cobbe et al., 2021). Please consult the licenses of those upstream datasets for terms governing the source problem text.

## Contact

For questions about the annotations or framework, please open an issue in this repository.
