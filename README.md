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

Then open [notebooks/data_extraction.ipynb](notebooks/data_extraction.ipynb) and select the `.venv` kernel.

To deactivate the virtual environment when you are done:

```bash
deactivate
```
