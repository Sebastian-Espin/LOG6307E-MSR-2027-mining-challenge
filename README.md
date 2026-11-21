# LOG6307E – MSR 2027 Mining Challenge

Do agent skills cross programming language boundaries? Dependencies stay inside their ecosystem
(npm for JavaScript, PyPI for Python), but an agent skill is a `SKILL.md` file written in natural
language, so in principle any project can reuse it. We use the
[GitSkills](https://huggingface.co/datasets/mvaccargiu/gitskills) dataset (July 2026 snapshot,
[Zenodo DOI 10.5281/zenodo.21875637](https://doi.org/10.5281/zenodo.21875637)) to study:

- **RQ1 (Ecosystem silos vs. universal flow):** To what extent does the verbatim copying of agent
  skills bypass traditional programming language boundaries?
- **RQ2 (Anatomy of a universal skill):** What structural and functional characteristics
  distinguish skills adopted across many diverse primary languages from strictly siloed skills?

## Repository structure

```
notebooks/
  data_extraction.ipynb   download and first exploration of the raw dataset
  data_cleaning.ipynb     cleaning and construction of the CSV files in data/
  data_analysis.ipynb     RQ1 analysis and figures
data/
  repos_clean.csv                  cleaned dataset (committed)
  skills_copied.csv
  adoptions.csv
  linguist_language_types.csv      language types from GitHub Linguist v9.6.0
  gitskills/                       raw download (git-ignored, ~13.4 GB)
figures/                  figures used in the report
requirements.txt
```

## Setup

Requires Python 3.10+.

### 1. Create and activate a virtual environment

macOS / Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows (PowerShell):

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 2. Install the dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Run the notebooks

```bash
jupyter lab
```

Select the `.venv` kernel and run the notebooks in order:

1. [data_extraction.ipynb](notebooks/data_extraction.ipynb): downloads the raw GitSkills tables
   from Hugging Face into `data/gitskills/` (~13.4 GB, only once).
2. [data_cleaning.ipynb](notebooks/data_cleaning.ipynb): builds the three CSV files in `data/`.
3. [data_analysis.ipynb](notebooks/data_analysis.ipynb): runs the analyses and writes the figures
   to `figures/`.

The CSV files are committed, so steps 1 and 2 can be skipped to reproduce the analysis only.

To deactivate the virtual environment when you are done:

```bash
deactivate
```

## Cleaned dataset

The three files are linked by `skill_id` and `repo_id`.

| File | One row per | Main columns |
|---|---|---|
| `repos_clean.csv` | repository with at least one `SKILL.md` (282,200) | `language`, `language_kind`, `stars`, `n_distinct_skills`, `is_aggregator`, `eligible` |
| `skills_copied.csv` | skill copied by at least 2 owners (230,355) | `file_sha`, `body_chars`, `has_scripts`, `has_references`, `sibling_count`, `n_owners`, `n_owners_eligible`, `n_languages` |
| `adoptions.csv` | (copied skill, repository) pair (1,382,321) | `skill_id`, `repo_id`, `n_paths` |

Definitions used in `data_cleaning.ipynb`:

- **Skill:** a distinct `SKILL.md` content (`file_sha`). Files with the same hash are verbatim copies.
- **Copied skill:** found in repositories of at least 2 different owners. Owners are counted
  rather than repositories, so one user copying a skill into several of their own repositories
  counts once.
- **Aggregator:** a repository with more than 100 distinct skills (registries, mirrors,
  marketplaces). About 1% of repositories, holding 54% of all `SKILL.md` files.
- **Language type:** from [GitHub Linguist](https://github.com/github-linguist/linguist) v9.6.0
  (`programming`, `markup`, `data`, `prose`), the release current when GitSkills was mined.
- **Eligible repository:** not an aggregator, not a fork, and with a `programming` primary
  language. Only eligible repositories count in the language analyses.

The full text of a skill is not in the CSV files. It can be read from the raw `artifacts` table
using `file_sha`.
