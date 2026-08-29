# HI-REASOH Dataset

**HI-REASOH** is a Hindi-language benchmark dataset for evaluating the reasoning abilities of large language models (LLMs). It covers six reasoning tasks — commonsense arithmetic, domain-specific STEM questions, commonsense comparison, fill-in-the-blank, natural language inference, and arithmetic word problems — with a total of **1,000 questions** written in Hindi (Devanagari script, Devanagari numerals).

The dataset was adapted from a Bangla-language source benchmark. All questions were translated into Hindi, with names, places, currency, and cultural references localized for the Hindi-speaking region (e.g., Bangladeshi names → common Indian names, টাকা → रुपये, Dhaka → Delhi), while English technical terms and labels were kept as-is. Answers were preserved so that every question remains numerically and semantically equivalent to the source.

## Dataset Overview

| File | Task | Questions | Columns |
|------|------|-----------|---------|
| `Task1_CA.csv` | Commonsense Arithmetic | 150 | Q No., Question, Answer |
| `Task2_DS.csv` | Domain-Specific QA (STEM) | 250 | Q No., Question, Answer, Domain, Sub-Domain |
| `Task3_CQ.csv` | Commonsense Comparison Questions | 150 | Q No., Question, Answer, option1, option2 |
| `Task4_FiB.csv` | Fill in the Blanks | 150 | Q No., Question, Answer |
| `Task5_QNLI.csv` | Question Natural Language Inference | 150 | Q No., Premise, Hypothesis, Answer |
| `Task6_AWP.csv` | Arithmetic Word Problems | 150 | Q No., Question, Answer |

**Total: 1,000 questions**

## Task Descriptions

### Task 1 — Commonsense Arithmetic (CA)
Everyday arithmetic questions embedded in short natural-language scenarios (shopping, travel, daily routines). Answers are numeric, written in Devanagari numerals.

### Task 2 — Domain-Specific QA (DS)
STEM questions across four domains: **Math (88)**, **Chemistry (80)**, **Physics (70)**, and **CS (12)**. Each row carries `Domain` and `Sub-Domain` labels in English. Answers are numeric.

### Task 3 — Comparison Questions (CQ)
Two-option commonsense questions: given a short scenario and two options, the model must select the correct one. Answers are the English labels `Option 1` (78) / `Option 2` (72).

### Task 4 — Fill in the Blanks (FiB)
Sentences and math statements with a blank (`_____`) that must be completed. Answers are numeric or short text.

### Task 5 — QNLI
Premise–hypothesis pairs requiring an inference judgment. Labels are kept in English: `Entailment` (53), `neutral` (54), `contradiction` (43).

### Task 6 — Arithmetic Word Problems (AWP)
Multi-step arithmetic word problems drawn from school-level mathematics (fractions, percentages, ratios, interest, mensuration, algebra). Answers are numeric.

## Format

All files are UTF-8 encoded CSVs with a header row. Questions use Devanagari script with Devanagari numerals (०–९); English technical terms, units, and task labels (e.g., `Option 1`, `Entailment`, domain names, GRE, GPU) are kept in Latin script as in the source benchmark.

```python
import csv

with open("Task1_CA.csv", encoding="utf-8") as f:
    rows = list(csv.reader(f))

header, data = rows[0], rows[1:]
print(header)      # ['Q No.', 'Question', 'Answer']
print(data[0])
```

## Construction Notes

- Source questions were manually translated from Bangla to Hindi; numeric answers were converted programmatically (Bangla numerals → Devanagari numerals) to guarantee answer fidelity.
- Person names, place names, and currency were localized to the Hindi-speaking Indian context using a consistent mapping table across all six tasks.
- Every file was validated: no Bangla script remains, all rows parse with the declared column count, and every answer is numerically identical to its source counterpart.
- `Q No.` preserves each question's identifier from the full source benchmark, so rows remain traceable to the original dataset.

## Intended Use

HI-REASOH is intended for benchmarking the Hindi reasoning capabilities of LLMs — zero-shot or few-shot evaluation of arithmetic, commonsense, domain knowledge, and inference in a low-resource language setting.

## Citation

If you use this dataset, please cite this repository:

```
HI-REASOH: A Hindi Reasoning Benchmark Dataset for Large Language Models.
https://github.com/rajandasguptaml/HI-REASOH-Dataset
```
