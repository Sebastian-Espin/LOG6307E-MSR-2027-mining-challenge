# LOG6307E – MSR 2027 Mining Challenge

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

1. [data_extraction.ipynb](notebooks/data_extraction.ipynb): downloads the raw GitSkills dataset (~13.4 GB) into `data/gitskills/`.
2. [data_cleaning.ipynb](notebooks/data_cleaning.ipynb): builds the cleaned CSV files in `data/`.

The cleaned CSV files are committed, so step 1 can be skipped if you only need them.

To deactivate the virtual environment when you are done:

```bash
deactivate
```
