# Pox


## Features

- Runs a specific `tox` environment in your repository
- Full tox output in action logs
- Caches `tox` cache between runs 

Your first run might take a bit longer

## How to use
For instance, if you setup the following `tox.ini` file:
```
[tox]
envlist = py38-mode-{unit}

[testenv:py38-mode-unit]
deps =
    -rrequirements.txt
    mypy
    black
    flake8
    pytest
    pytest-cov
    pytest-asyncio
    coverage-badge

commands = 
    black src
    black test
    mypy src
    flake8 src
    pytest --cov=src -v --ignore=test/integration
```


You can then setup the following GH Action:

```
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: tox test
    steps:
      - uses: actions/setup-python@v2
        with:
          python-version: '3.8'
      - name: Checkout
        uses: actions/checkout@v2
      - name: Tox Action Step
        id: tox
        uses: paolorechia/pox@v1.0
        with:
          tox_env: 'py38-mode-unit'
      - name: Get the output success flag
        run: |
          echo "Tests have passed: ${{ steps.tox.outputs.success_flag }}"
```

Notice in particular that this block
```
        with:
          tox_env: 'py38-mode-unit'
```

Must match the environment name in `tox.ini` line 2: `[testenv:py38-mode-unit]` 

You must also make sure to set the correct `python-version`: 

```
        with:
          python-version: '3.8'
```
