# PySysML2

PySysML2 is a Python-based parser for the SysML 2.0 textual modeling language. Its main purpose is to parse a SysML 2.0 textual model into a Python object, and then transform that model into various data structures useful for data science and analysis.

## Dependencies

PySysML2 has the following dependencies:

- anytree: provides the tree data structure, the basis for the Python model class
- graphviz: renders images of graphs
- numpy: numerical analysis package
- pandas: data analysis package, provides the DataFrame data structure
- openpyxl: allows Pandas to export to Excel
- antlr4: provides the language parsing workbench

Note that some of these packages (specifically Anytree, Graphviz, and Antlr4) are not available on Anaconda. Also, Pandas does not automatically install the required OpenPyxl module for exporting Excel, so that must be done separately.

## Installation

See the [Development section](#development) for installation instructions if you are a developer.

### Install From Source

```console
git clone git@github.com:TrekkieByDay/PySysML2.git

cd PySysML2/

pip install .
```

## Usage

### CLI

After installation, the `pysysml2` CLI tool should be available. The following demonstrates using the `pysysml2 export` command to export the SysML 2.0 textual model file to various output file formats.
```console
❯ pysysml2 export examples/models/model_test_1.sysml2 --output-dir out/ --format json,txt,csv,xlsx,dot,png
Using output directory: /Users/delannmt/Workspace/Projects/react/sysml/PySysML2/tmp
Exporting to JSON...
Exporting to txt...
Exporting to csv...
Exporting to xlsx...
Exporting to dot...
Exporting to png...
```

For more information about the `pysysml2 export` command, use the `--help` option:
```console
❯ pysysml2 export --help
Usage: pysysml2 export [OPTIONS] MODEL_FILE

  Export a SysML v2 model to various file formats.

Arguments:
  MODEL_FILE  The sysml2 model file.  [required]

Options:
  --format TEXT          One or more comma-separated output file formats.
                         Supported formats: json,txt,csv,xlsx,dot,png
                         [default: json]
  -o, --output-dir PATH  The output directory for the generated file(s).
                         Defaults to current directory.
  --help                 Show this message and exit.
```

### Python API Examples

The `examples/` directory contains an example Python script using `pysysml2` to export sample SysML 2.0 textual models to various output file formats. 

### Jupyter Notebook

PySysML2 can be used through Jupyter notebooks. Check the [PySysML2_notebook.ipynb](PySysML2_notebook.ipynb) notebook to test the parsing functionality using the provided SysML 2.0 models.

## Project Structure

The `pysysml2` directory contains all the code. It is divided into the `grammar` and `modeling` packages.

The `grammar` package contains all the Antlr4 parsing code. The primary artifact of interest is the `SysML.g4` grammar source file, which defines the basic elements of SysML 2.0 that PySysML2 implements. This file is used by the stand-alone Antlr4 command-line application that generates the language parsing Python code. Everything in the `distpy`, `.antlr`, and `distj` directories is auto-generated, and only `distpy` is required for PySysML2. The `sysml2_model_visitor.py` module is an extension of the generated `SysML2Visitor.py` and is the interface between the language parse tree from the textual model and the PySysML2 toolset.

The `modeling` package contains the SysML 2.0 modeling implementation and export tools. The `element` module implements model elements, and the `model` module implements a SysML 2.0 model class built from element objects. All export functions are in `model.py`.

## Development

### Setup Poetry

To use PySysML2 with Poetry, follow these steps:

1. Install Poetry by following the instructions in the [official documentation](https://python-poetry.org/docs/#installation). **TLDR:** run the following command:
```
curl -sSL https://install.python-poetry.org | python3 -
```

2. In the root directory of the project repository, run the following command to install all the required dependencies:
```
poetry install
```

This will install the main dependencies specified in the `pyproject.toml` file.

#### Development Group

PySysML2 has a `dev` group in its `pyproject.toml` file that contains the dependencies required for development and testing. To install these dependencies, run the following command in the root directory of the cloned repository:

```
poetry install --group=dev
```

This will install the development dependencies, including packages such as `pytest`.

### Using Poetry

By default, Poetry creates a virtual environment for each project, so all the
dependencies are installed locally to that environment. This ensures that different
projects can have different dependencies and versions installed without interfering
with each other.

To execute a command inside the virtual environment, use the `poetry run` command.
For instance, to run the tests for this project, run the following command:
```
poetry run pytest
```

To activate the virtual environment, run the following command:
```
poetry shell
```

To exit the virtual environment, use `exit`:
```
exit
```

### Bumping Project Version

The native `poetry version` command only updates the version in `pyproject.toml`. However, the
`__version__` variable in `pysysml2/__init__.py` must also be updated. To ensure the updates
are done in sync, developers should use the [poetry-bumpversion](https://pypi.org/project/poetry-bumpversion/)
plugin for Poetry.
```
poetry self add poetry-bumpversion
```

The `poetry version` command can then be used, and both versions will be updated
together. For example to bump the patch version:
```console
❯ poetry version patch
Bumping version from 0.1.0 to 0.1.1
poetry-bumpversion: processed file: pysysml2/__init__.py
```

### poetry2setup

For the convenience of users installing from source without Poetry, developers can
generate the `setup.py` file from the `pyproject.toml` using the `poetry2setup` tool
(requires [Poetry dev group installation](#development-group)):
```
poetry run poetry2setup > setup.py
```

### poetry2conda

To support Anaconda distribution, developers can generate the conda environment file
from the `pyproject.toml` using the `poetry2conda`
(requires [Poetry dev group installation](#development-group)):
```
poetry run poetry2conda pyproject.toml environment.yaml
```

### Why use Poetry?

Poetry is a Python packaging and dependency management tool that helps simplify the process of building, packaging, and distributing Python projects. It provides a simple and intuitive way to manage project dependencies, handle virtual environments, and create distributable packages.

Using Poetry has several benefits:

- **Dependency management**: Poetry simplifies dependency management by allowing you to easily install, uninstall, and upgrade packages, and automatically resolving dependencies between packages.
- **Virtual environments**: Poetry creates and manages virtual environments for each project, ensuring that different projects can have different dependencies and versions installed without interfering with each other.
- **Package building and publishing**: Poetry provides a simple way to build and publish packages to PyPI, as well as other package indexes such as your company's private package index.
- **PEP standards compliance**: Poetry is designed to comply with the Python Enhancement Proposal (PEP) standards, which helps ensure compatibility with other Python tools and libraries.

### How Poetry helps follow PEP standards

PEP standards are a set of guidelines and recommendations for how to structure, package, and distribute Python code. These standards help ensure that Python packages are well-designed, easy to use, and compatible with other Python tools and libraries.

Poetry is designed to follow the PEP standards, which makes it easier to create Python packages that are compliant with these guidelines. Here are some ways in which Poetry helps follow PEP standards:

- [PEP 517](https://peps.python.org/pep-0517/)/[518](https://peps.python.org/pep-0518/) compliance: Poetry uses the PEP 517/518 standards for building and packaging Python projects, including build isolation to ensure that builds are reproducible and do not rely on the developer's environment. These standards help ensure compatibility with other Python tools such as pip and setuptools.
- [pyproject.toml](https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/): Poetry uses a pyproject.toml configuration file to manage project settings and dependencies. This file conforms to the [PEP 621](https://peps.python.org/pep-0621/) standard, which provides a standard way to define project metadata and dependencies.
- [PEP 440](https://peps.python.org/pep-0440/) versioning: Poetry uses the PEP 440 standard for versioning packages, which provides a standard way to version and compare package versions.
- [PEP 508](https://peps.python.org/pep-0508/) dependencies: Poetry supports PEP 508-style dependencies, which allows you to specify dependencies with more detail and flexibility than standard requirements.txt files.