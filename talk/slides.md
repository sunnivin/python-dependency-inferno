---
marp: true
theme: theme.css
paginate: true

---




<!-- _class: title -->
<!-- _header: '_21.05.2025_' -->
<!-- paginate: false -->
# The dependency inferno
## A descent into the Python packaging hell

####
####
####
#### :zap: Lightning Talk :zap: 
#### Sunniva Indrehus


---


<!-- paginate: true -->

```
autodesk$ echo $(whoami)
```

---

<!-- _footer: 'Figure credit [xkcd](https://imgs.xkcd.com/comics/python_environment.png)' -->

# The Python dependency hell 

![bg right w:600](https://imgs.xkcd.com/comics/python_environment.png)


---

### Short :snake: :package: history 

- **1991** Python 1.0 released (Guido van Rossum)
- **2004** `setuptools` (Phillip Eby) — Packaging foundation
- **2007** `virtualenv` (Ian Bicking) - Isolating environments
- **2008**  Python 3.0
- **2011** `pip` bundled with Python - Standardizing the installer
- **2012**  `conda` (Anaconda Inc.) and `venv` (Vinay Sajip)
- **2015** [PEP 517](https://peps.python.org/pep-0517/) - Build system interface
- **2016** [PEP 518](https://peps.python.org/pep-0518/) - Build dependencies declaration
- **2017** `pipenv` (PyPA-backed) - Unified env & deps 
- **2018** `poetry` (S. Eustace) - All-in-one manager
- **2019**  [`hatch`](https://github.com/pypa/hatch) (Ofek)
- **2020** [PEP 621](https://peps.python.org/pep-0621/) - Metadata, [`PDM`](https://github.com/pdm-project/pdm) (Frost Ming)
- **2024** [`uv`](https://docs.astral.sh/uv/) (Astral)



--- 

- **1991** Python 1.0 released (Guido van Rossum)
- **2004** `setuptools` (Phillip Eby) — Packaging foundation
- **2007** `virtualenv` (Ian Bicking) - Isolating environments
- **2008**  Python 3.0
- **2011** `pip` bundled with Python - Standardizing the installer
- **2012**  `conda` (Anaconda Inc.) and `venv` (Vinay Sajip)
- **2015** [PEP 517](https://peps.python.org/pep-0517/) - Build system interface
- **2016** [PEP 518](https://peps.python.org/pep-0518/) - Build dependencies declaration
- **2017** `pipenv` (PyPA-backed) - Unified env & deps 
 <div style="border: 2px solid red; padding: 6px; border-radius: 6px; background-color: #fff5f5;">
    <strong>2018</strong> <code>poetry</code> (S. Eustace) - All-in-one manager
    </div>

- **2019**  [`hatch`](https://github.com/pypa/hatch) (Ofek)
- **2020** [PEP 621](https://peps.python.org/pep-0621/) - Metadata, [`PDM`](https://github.com/pdm-project/pdm) (Frost Ming)
 <div style="border: 2px solid red; padding: 6px; border-radius: 6px; background-color: #fff5f5;">
    <strong>2024</strong> <code>uv</code> (Astral)
    </div>


---

# UV

- Ultra-fast Python package manager and resolver
- Drop-in replacement for `pip`, `pip-tools`, and parts of `Poetry`
-  Cross-platform Python installation (:scream:  No `pyenv`! :scream: )
- Lock file is cross-platform


<div style="display: flex; flex-direction: column; align-items: margin-top: 2em;">
  <img src="figures/illustrations/time.svg" alt="Cute kitten" style="max-width: 80%; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.15);" />
  <figcaption style="margin-top: 0.8em; font-size: 0.9em; color: #555; font-style: italic;">
    Time for installing Trio's dependencies with warm cache, Figure credit: <a href="https://github-production-user-asset-6210df.s3.amazonaws.com/1309177/316150505-629e59c0-9c6e-4013-9ad4-adb2bcf5080d.svg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250520%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250520T123025Z&X-Amz-Expires=300&X-Amz-Signature=6f16d4685a6aaca2b0eb123678ffb5870a70f26cf95691da6d75ce50ca366933&X-Amz-SignedHeaders=host#only-light" style="color: blue;">Astral's docs</a>
  </figcaption>
</div>

---

# Basic API 

## Install uv  

```
~$ curl -LsSf https://astral.sh/uv/install.sh | sh
~$ uv 
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>
```

## Install latest python version

```
~$ uv python install
~$ uv python install 3.12
~$ uv python install pypy@3.10
```

--- 


<!-- # Start a project 

Create project
```
~$ uv init test-uv
~test-uv$ ls
├── .git
├── .gitignore
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
```

Run example script 

```
~test-uv$ uv run main.py
  Using CPython 3.13.3
  Creating virtual environment at: .venv
  Hello from test-uv!
```
---  -->

# Start a packaged project 

Create
```
~$ uv init --package test-uv-package
~test-uv-package$ ls 
├── .git
├── .gitignore
├── .python-version
├── README.md
├── pyproject.toml
├── src
│   ├── test_uv_package
│     └── __init__.py
```

Run

```
~test-uv-package$ uv run test-uv-package
  Using CPython 3.13.3
  Creating virtual environment at: .venv
  Hello from test-uv-package!
```

--- 


# Plain dependency migration
<div style="
  font-family: monospace; 
  background-color: white;
  padding: 1em; 
  border-radius: 6px; 
  border: 1px solid #ccc;
  display: inline-block;
  max-width: 1100px;
">
  <div>~my-project$ uvx migrate-to-uv</div>

  <div >Installed 1 package in 16ms</div>

  <div >
    Locking dependencies with "uv lock"...
  </div>

  <div >Using CPython 3.12.4 interpreter at: /Users/sunnivaindrehus/.pyenv/versions/3.12.4/bin/python3.12</div>

  <div style="
    padding-left: 4em;
    border: 2px solid red; 
    border-radius: 4px; 
    padding: 4px; 
    margin: 4px 0;
    background-color: #fff5f5;
  ">Resolved 123 packages in 1.01s</div>

  <div >Successfully migrated project from Poetry to uv!</div>
</div>


---

# Complex dependency migration 

1. Make sure to capture all dependencies (use the right poetry version, and all relevant groups)
  
    ```
    ~/repo$ poetry@1.8 export -f requirements.txt --output requirements.txt --without-hashes --with dev
    ```

2. Remove `pyproject.toml` and `poetry.lock` files.
   
    ```
    ~/repo$ uv init --bare
    ```

3. Install locked dependencies from `.txt` file 
   
    ```
    ~/repo$ uv add -r requirements.txt
    ```

---

# Private packages migration 

### Poetry 1.8 
SCP-like shorthand
```
[tool.poetry.dependencies]
python = ">=3.11,<3.12"
my-private-package = { git = "git@github.com:my-org/my-private-package.git", tag = "1.0.1" }
```


### uv 
Explicit URL schemes
```
[project]
requires-python = ">=3.11,<3.12"
dependencies = [
    "my-private-package"
  ]

[tool.uv.sources]
"my-private-package" = {git= "ssh://git@github.com:my-org/my-private-package.git", rev="34f59994bb920679ba5f0cade8061e0da7ca00b5"}
```

--- 

# Poetry 2.X vs. UV 


![bg right w:500 h:700](figures/illustrations/dependency_hell.png)
