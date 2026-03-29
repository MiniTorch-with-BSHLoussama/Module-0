---
hide:
  - navigation
---

# MiniTorch Installation

MiniTorch requires **Python 3.11**. To check your version:

```bash
python --version
```

> **Use Anaconda or Miniconda.** Conda is strongly recommended over plain `pip` + `venv`,
> especially on Windows. Conda correctly bundles the native binaries (DLLs) for packages
> like PyTorch and Numba. Plain pip installs of these packages frequently fail on Windows
> with `[WinError 1114]` DLL errors that are very hard to debug.

---

## Quick Start (Recommended)

### With Conda (Recommended)

```bash
# 1. Clone your assignment
git clone <YOUR_FORKED_REPO_URL>
cd <repo-folder>

# 2. Create environment (installs everything)
conda env create -f environment.yml
conda activate minitorch

# 3. Run the app
cd project
streamlit run app.py -- 0
```

**That's it!** The environment file handles Python, PyTorch, Numba, numpy, and all dependencies.

---

### With pip (Advanced Users)

If you prefer pure pip without conda:

```bash
# 1. Create virtual environment
python3.11 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 2. Install package with all dependencies
pip install -e .[extra]

# 3. Run the app
cd project
streamlit run app.py -- 0
```

**Note**: On Windows, this may fail at PyTorch installation. Use conda instead.

---

## Detailed Setup

### 1. Install Conda (If Not Already Installed)

Choose one:
- **Miniconda** (minimal, recommended): https://docs.conda.io/en/latest/miniconda.html
- **Anaconda** (full distribution): https://www.anaconda.com/download

---

### 2. Clone Your Assignment

```bash
mkdir workspace
cd workspace
git clone <YOUR_FORKED_REPO_URL>
cd <repo-folder>
```

---

### 3. Create the Environment

#### Option A — One Command (Recommended)

```bash
conda env create -f environment.yml
conda activate minitorch
```

This installs everything: Python 3.11, PyTorch, Numba, numpy, and all project dependencies.

---

#### Option B — Manual Setup

**Create environment:**
```bash
conda create --name minitorch python=3.11
conda activate minitorch
```

**Install PyTorch via conda:**

Pick CPU or GPU based on your machine:

```bash
# CPU — works everywhere, use if unsure
conda install pytorch cpuonly -c pytorch

# GPU — NVIDIA with CUDA 12.1
conda install pytorch pytorch-cuda=12.1 -c pytorch -c nvidia

# GPU — NVIDIA with CUDA 11.8
conda install pytorch pytorch-cuda=11.8 -c pytorch -c nvidia

# GPU — Apple Silicon (M1/M2/M3)
conda install pytorch -c pytorch
```

> Find your CUDA version with `nvidia-smi` (top-right corner). If unsure, use CPU.

**Install native packages:**
```bash
conda install numpy=1.26.4 numba=0.61.2 llvmlite=0.44.0 pandas jinja2 colorama networkx -c conda-forge
```

> **Why these versions?** `numba==0.61.2` requires `numpy<2.0` for binary compatibility.
> `numpy==1.26.4` is the highest stable 1.x version supported by numba.

**Install MiniTorch package:**
```bash
pip install -e .[extra]
```

---

### 4. Verify Installation

```bash
python -c "import minitorch; print('MiniTorch OK')"
python -c "import torch; print('PyTorch', torch.__version__)"
python -c "import streamlit; print('Streamlit', streamlit.__version__)"
python -c "import numba; print('Numba', numba.__version__)"
```

All four should print version numbers with no errors.

---

### 5. Run the App

Navigate to `project/` and specify the module number:

```bash
cd project
streamlit run app.py -- 0
```

Use `-- 0`, `-- 1`, `-- 2`, etc. for different modules. Your browser opens at `http://localhost:8501`.

---

## Platform Notes

### Windows
- **Always install PyTorch and Numba via conda**, not pip. Pip builds don't bundle required DLLs.
- Run commands in **Anaconda Prompt** or a terminal with conda initialized (prompt shows `(minitorch)`).
- French error *"Une routine d'initialisation d'une bibliothèque de liens dynamiques a échoué"* = DLL error. Fix:
  ```bash
  pip uninstall torch -y
  conda install pytorch cpuonly -c pytorch
  ```

### Linux
- Conda recommended but pip generally works.
- For GPU: ensure system CUDA version matches `pytorch-cuda` version (`nvidia-smi` to check).

### macOS (Apple Silicon — M1/M2/M3)
- Use `conda install pytorch -c pytorch` (MPS/Metal GPU auto-enabled).
- Do **not** install `cpuonly` on Apple Silicon.

---

## Development Setup (For Instructors/Contributors)

If you're modifying MiniTorch code:

```bash
# Install with dev tools
pip install -e .[dev,extra]

# Set up pre-commit hooks (auto-formats on commit)
pre-commit install

# Run linting manually
pre-commit run --all-files
```

This installs flake8, mypy, ruff, and pre-commit hooks for code quality.

---

## Troubleshooting

### `[WinError 1114]` DLL error (Windows)
PyTorch was installed via pip instead of conda:
```bash
pip uninstall torch -y
conda install pytorch cpuonly -c pytorch
```

### `ModuleNotFoundError: No module named 'minitorch'`
Ran `pip install -e .` from wrong directory:
```bash
cd ..  # Go to repo root (folder with pyproject.toml)
pip install -e .[extra]
```

### `app.py: error: the following arguments are required: module_num`
Missing module number. Use `--` separator:
```bash
streamlit run app.py -- 0
```

### `NotImplementedError: Need to implement for Task 0.X`
This is expected — the app works, waiting for your code in `minitorch/operators.py`.

### numpy/numba binary incompatibility
```
ValueError: numpy.dtype size changed, may indicate binary incompatibility
```
numpy 2.x installed but numba requires 1.x:
```bash
conda install numpy=1.26.4 numba=0.61.2 llvmlite=0.44.0 -c conda-forge
```

---

## Package Management

MiniTorch uses **`pyproject.toml`** (modern Python packaging standard) for dependency management.

**Install options:**
```bash
pip install -e .          # Core dependencies only
pip install -e .[dev]     # + development tools (flake8, mypy, pre-commit)
pip install -e .[extra]   # + Streamlit app & visualization tools
pip install -e .[dev,extra]  # Everything
```

**What is `-e`?** Editable mode — changes to code are immediately available without reinstalling.

---

## Quick Reference

**One-command setup:**
```bash
conda env create -f environment.yml
conda activate minitorch
cd project
streamlit run app.py -- 0
```

**Manual conda setup:**
```bash
conda create -n minitorch python=3.11
conda activate minitorch
conda install pytorch cpuonly -c pytorch
conda install numpy=1.26.4 numba=0.61.2 llvmlite=0.44.0 pandas jinja2 colorama networkx -c conda-forge
pip install -e .[extra]
cd project
streamlit run app.py -- 0
```

**Pure pip setup (Linux/Mac):**
```bash
python3.11 -m venv venv
source venv/bin/activate
pip install -e .[extra]
cd project
streamlit run app.py -- 0
```
