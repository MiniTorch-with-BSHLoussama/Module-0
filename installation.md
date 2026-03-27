---
hide:
  - navigation
---

# MiniTorch Installation

MiniTorch requires Python 3.8 or higher. To check your version of Python, run:

```bash
>>> python --version
```

We recommend creating a global MiniTorch workspace directory that you will use
for all modules:

```bash
>>> mkdir workspace; cd workspace
```

## Environment Setup

We highly recommend setting up a *virtual environment*. The virtual environment lets you install packages that are only used for your assignments and do not impact the rest of the system.

**Option 1: Anaconda (Recommended)**
```bash
>>> conda create --name minitorch python=3.11    # Run only once
>>> conda activate minitorch
>>> conda install llvmlite                  # For optimization
```

**Option 2: Venv**
```bash
>>> python -m venv venv          # Run only once
>>> source venv/bin/activate
```

The first line should be run only once, whereas the second needs to be run whenever you open a new terminal to get started for the class. You can tell if it works by checking if your terminal starts with `(minitorch)` or `(venv)`.

## Getting the Code

Each assignment is distributed through a Git repo. You should fork the template of the assignment and then edit yours in your forked repo. Once you have forked the template code, you can clone your own version:

```bash
>>> git clone {{ASSIGNMENT}}
>>> cd {{ASSIGNMENT}}
```

## Installation

Install all packages in your virtual environment:

```bash
>>> python -m pip install -e ".[dev]"
>>> python -m pip install -r requirements.extra.txt
>>> python -m pip install -Ue .
```

## Verification

Make sure everything is installed by running:

```bash
>>> python -c "import minitorch; print('Success!')"
```

You're ready to start the assignments!
