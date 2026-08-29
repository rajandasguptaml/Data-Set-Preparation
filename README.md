# HI-REASOH Dataset

**HI-REASOH** is a Hindi-language benchmark for evaluating the reasoning abilities of large language models (LLMs). It covers six reasoning tasks — commonsense arithmetic, domain-specific STEM questions, commonsense comparison, fill-in-the-blank, natural language inference, and arithmetic word problems — written in Hindi (Devanagari script, Devanagari numerals) and grounded in an Indian context (Indian names, places, and currency in रुपये).

The repository ships two things:

- **`Dataset/`** — the **full question pool: 3,255 questions** across the six tasks.
- **`Codes/`** — two independent **1,000-question random evaluation splits** (`Run_1`, `Run_2`) sampled from the pool, ready to drop into an LLM evaluation loop.

## Repository Structure

```
.
├── Dataset/                          # Full question pool (3,255 questions)
│   ├── Task1(CA).csv                 # Commonsense Arithmetic          410
│   ├── Task2(DS).csv                 # Domain-Specific QA (STEM)        705
│   ├── Task3(CQ).csv                 # Comparison Questions             400
│   ├── Task4(FiB).csv                # Fill in the Blanks               665
│   ├── Task5(QNLI).csv               # Question NLI                     425
│   └── Task6(AWP).csv                # Arithmetic Word Problems         650
│
└── Codes/                            # Random 1,000-question evaluation splits
    ├── Run_1/HI_REASOH__run_1_random_1000/
    │   ├── Task1_random_150.csv
    │   ├── Task2_random_250.csv
    │   ├── Task3_random_150.csv
    │   ├── Task4_random_150.csv
    │   ├── Task5_random_150.csv
    │   └── Task6_random_150.csv
    └── Run_2/HI_REASOH_run_2_random_1000/
        └── (same six files, independently sampled)
```

## Dataset Overview (`Dataset/`)

| File | Task | Questions | Columns |
|------|------|-----------|---------|
| `Task1(CA).csv` | Commonsense Arithmetic | 410 | Q No., Question, Answer |
| `Task2(DS).csv` | Domain-Specific QA (STEM) | 705 | Q No., Question, Answer, Domain, Sub-Domain |
| `Task3(CQ).csv` | Commonsense Comparison Questions | 400 | Q No., Question, Answer, option1, option2 |
| `Task4(FiB).csv` | Fill in the Blanks | 665 | Q No., Question, Answer |
| `Task5(QNLI).csv` | Question Natural Language Inference | 425 | Q No., Premise, Hypothesis, Answer |
| `Task6(AWP).csv` | Arithmetic Word Problems | 650 | Q No., Question, Answer |

**Total: 3,255 questions.** `Q No.` runs from 1 to *N* within each file and uniquely identifies a question inside its task.

## Task Descriptions

### Task 1 — Commonsense Arithmetic (CA) · 410
Everyday arithmetic questions embedded in short natural-language scenarios (shopping, travel, daily routines). Answers are numeric, written in Devanagari numerals.

### Task 2 — Domain-Specific QA (DS) · 705
STEM questions with English `Domain` / `Sub-Domain` labels. Answers are numeric.

| Domain | Questions | Sub-Domains |
|--------|-----------|-------------|
| Math | 249 | Trigonometry (90), Probability (71), Solid Geometry (69), Set Function (16), Math (3) |
| Physics | 212 | Physics |
| Chemistry | 209 | Chemistry |
| CS | 34 | CS |

One row (`Q No.` 478) currently has an empty `Domain` / `Sub-Domain`.

### Task 3 — Comparison Questions (CQ) · 400
Two-option commonsense questions: given a short scenario and two options (`option1`, `option2`), the model must select the correct one. Answers are the English labels `Option 1` (193) / `Option 2` (207).

### Task 4 — Fill in the Blanks (FiB) · 665
Sentences and math/science statements with a blank (`_____`) that must be completed. Answers are numeric or short text.

### Task 5 — QNLI · 425
Premise–hypothesis pairs requiring an inference judgment. Labels are in English: `Entailment` (141), `neutral` (153), `contradiction` (131).

### Task 6 — Arithmetic Word Problems (AWP) · 650
Multi-step arithmetic word problems drawn from school-level mathematics (fractions, percentages, ratios, interest, mensuration, algebra). Answers are numeric.

## Evaluation Splits (`Codes/`)

Each run is a random sample of **1,000 questions** from `Dataset/`, drawn with the same per-task quota:

| File | Task | Questions |
|------|------|-----------|
| `Task1_random_150.csv` | Commonsense Arithmetic | 150 |
| `Task2_random_250.csv` | Domain-Specific QA | 250 |
| `Task3_random_150.csv` | Comparison Questions | 150 |
| `Task4_random_150.csv` | Fill in the Blanks | 150 |
| `Task5_random_150.csv` | QNLI | 150 |
| `Task6_random_150.csv` | Arithmetic Word Problems | 150 |

**Total per run: 1,000 questions.**

- Rows in a split are **identical** to the corresponding rows in `Dataset/` (same columns, same text, same answer); `Q No.` links each sampled question back to the full pool.
- `Run_1` and `Run_2` are **independent draws** — they share some questions but differ in most (e.g., only 58 of 150 Task 1 questions appear in both). Reporting results on both runs gives a sense of sampling variance.
- Rows are in shuffled order, not sorted by `Q No.`

Split-level label / domain distributions:

| | Run_1 | Run_2 |
|---|---|---|
| Task 2 domains | Math 97 · Physics 76 · Chemistry 69 · CS 8 | Math 88 · Chemistry 80 · Physics 70 · CS 12 |
| Task 3 answers | Option 1 69 · Option 2 81 | Option 1 78 · Option 2 72 |
| Task 5 labels | Entailment 57 · neutral 50 · contradiction 43 | Entailment 53 · neutral 54 · contradiction 43 |

## Format

All files are UTF-8 encoded CSVs with a header row. Questions use Devanagari script with Devanagari numerals (०–९); English technical terms, units, and task labels (e.g., `Option 1`, `Entailment`, domain names, GRE, GPU) are kept in Latin script.

```python
import csv

# Full pool
with open("Dataset/Task1(CA).csv", encoding="utf-8") as f:
    rows = list(csv.reader(f))

header, data = rows[0], rows[1:]
print(header)      # ['Q No.', 'Question', 'Answer']
print(len(data))   # 410
print(data[0])     # ['1', 'करण एक १४ साल का लड़का है। ...', '१०']

# Evaluation split
with open("Codes/Run_1/HI_REASOH__run_1_random_1000/Task1_random_150.csv", encoding="utf-8") as f:
    split = list(csv.reader(f))[1:]
print(len(split))  # 150
```

## Construction Notes

- All questions are written natively in Hindi (Devanagari script, Devanagari numerals) with person names, place names, and currency drawn from the Indian context.
- Every file was validated: all rows parse with the declared column count, `Q No.` values are unique within each task, and answers are consistent with their questions.
- The 1,000-question splits in `Codes/` were produced by random sampling from `Dataset/` with fixed per-task quotas (150 / 250 / 150 / 150 / 150 / 150); each run uses a different random draw.

## Intended Use

HI-REASOH is intended for benchmarking the Hindi reasoning capabilities of LLMs — zero-shot or few-shot evaluation of arithmetic, commonsense, domain knowledge, and inference in a low-resource language setting. Use `Codes/Run_*` for a fixed, comparable 1,000-question evaluation, or `Dataset/` for the full pool.

## Citation

If you use this dataset, please cite this repository:

```
HI-REASOH: A Hindi Reasoning Benchmark Dataset for Large Language Models.
https://github.com/rajandasguptaml/HI-REASOH_Dataset
```
