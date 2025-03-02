---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

(installation)=

# How to install napari

Welcome to the **napari** installation guide!

This guide will teach you how to do a clean install of **napari** and launch the viewer.

```{note}
If you want to contribute code back into napari, you should follow the [development installation instructions in the contributing guide](https://napari.org/developers/contributing.html) instead.
```

(install-python-package)=

## Install as Python package (recommended)

This installation method allows you to use napari from Python to programmatically
interact with the app. It is the best way to install napari and make full use of
all its features.

It requires:
- [Python {{ python_version_range }}](https://www.python.org/downloads/)
- the ability to install python packages via [pip](https://pypi.org/project/pip/) OR [conda-forge](https://conda-forge.org/docs/user/introduction.html)

You may also want:
- an environment manager like [conda](https://docs.conda.io/en/latest/miniconda.html) or
[venv](https://docs.python.org/3/library/venv.html) **(Highly recommended)**

Python package distributions of napari can be installed via `pip`, `conda-forge`, or from source.

````{important}
While not strictly required, it is highly recommended to install
napari into a clean virtual environment using an environment manager like
[conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) or
[venv](https://docs.python.org/3/library/venv.html).

This should be set up *before* you install napari. For example, setting with
up a Python {{ python_version }} environment with `conda`:

{{ conda_create_env }}
````

Choose one of the options below to install napari as a Python package.

::::::{tab-set}

:::::{tab-item} From conda-forge using conda

If you prefer to manage packages with conda, napari is available on the
conda-forge channel. We also recommend this path for users of arm64 macOS machines
(Apple Silicon, meaning a processor with a name like "M1"). You can install it with:

```sh
conda install -c conda-forge napari pyqt
```

You can then upgrade to a new version of napari using:

```sh
conda update napari
```

If you want to install napari with PySide2 as the backend you need to install it using

```sh
conda install -c conda-forge napari pyside2
```

````{note}
In some cases, `conda`'s default solver can struggle to find out which packages need to be
installed for napari. If it takes too long or you get the wrong version of napari
(see below), consider:
1. Overriding your default channels to use only `conda-forge` by adding
`--override-channels` and specifying the napari and Python versions explicitly.
For example, use {{ python_version_code }} to get Python {{ python_version }} and
{{ napari_conda_version }} to specify the napari version as {{ napari_version }},
the current release.

2. Switching to the new, faster [`libmamba` solver](https://conda.github.io/conda-libmamba-solver/libmamba-vs-classic/),
by updating your `conda` (>22.11), if needed, and then installing and activating
the solver, as follows:
```
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```
3. Alternately, consider installing [`mamba`](https://github.com/mamba-org/mamba)
in your base environment with `conda install -n base -c conda-forge mamba`.
Then you can use `mamba` by replacing `conda` with `mamba` in the installation instructions, for example:
```
mamba install napari
```

````

:::::

:::::{tab-item} From PyPI using pip

napari can be installed from PyPI on most macOS (Intel x86), Linux, and Windows systems with Python
{{ python_version_range }} using pip:

```sh
python -m pip install "napari[all]"
```
You can then upgrade napari to a new version using:

```sh
python -m pip install "napari[all]" --upgrade
```

*(See [Choosing a different Qt backend](#choosing-a-different-qt-backend) below for an explanation of the `[all]`
notation.)*

*(See [Using constraints file](#using-constraints-files) for help installing older versions of napari)*

:::::

:::::{tab-item} From the main branch on Github

To install the latest version with yet to be released features from Github you can use pip:

```sh
python -m pip install "git+https://github.com/napari/napari.git#egg=napari[all]"
```
:::::
::::::


<!-- #region -->
### Checking it worked

After installation you should be able to launch napari from the command line by
simply running

```sh
napari
```

An empty napari viewer should appear as follows:

![macOS desktop with a napari viewer window without any image opened in the foreground, and a terminal in the background with the appropriate conda environment activated (if applicable) and the command to open napari entered.](../assets/tutorials/launch_cli_empty.png)

````{note}
On some platforms, particularly macOS and Windows, there may be a ~30 second
delay before the viewer appears on first launch. This is expected and subsequent
launches should be quick. However, anti-malware and other security software
measures may further delay launches—even after the first launch.
````

You can check the napari version, to ensure it's what you expect, for example
the current release {{ napari_version }}, using the command: `napari --version` .

### Choosing a different Qt backend

napari needs a library called [Qt](https://www.qt.io/) to run its user interface
(UI). In Python, there are two alternative libraries to run this, called
[PyQt5](https://www.riverbankcomputing.com/software/pyqt/download5) and
[PySide2](https://doc.qt.io/qtforpython/). By default, we don't choose for you,
and simply running `python -m pip install napari` will not install either. You *might*
already have one of them installed in your environment, thanks to other
scientific packages such as Spyder or matplotlib. If neither is available,
running napari will result in an error message asking you to install one of
them.

Running `python -m pip install "napari[all]"` will install the default framework, which is currently
PyQt5--but this could change in the future. However, if you have a Mac with the newer arm64
architecture (Apple Silicon), this will not work--see [note on m1](note-m1).

To install napari with a specific framework, you can use:

```sh
python -m pip install "napari[pyqt5]"    # for PyQt5

# OR
python -m pip install "napari[pyside2]"  # for PySide2
```

(note-m1)=
```{note}

For arm64 macOS machines (Apple Silicon), pre-compiled PyQt5 or PySide2 packages
([wheels](https://realpython.com/python-wheels/)) are not available on
[PyPI](https://pypi.org), the repository used by `pip`, so trying to
`pip install napari[all]` or either of the variants above will fail. However,
you can install one of those libraries separately, for example from `conda-forge`,
and then use `pip install napari`.
```

```{note}
If you switch backends, it's a good idea to `pip uninstall` the one
you're not using.
```

### Using constraints files

Since napari 0.4.18, we store constraints files with information about each exact dependency version against which napari was tested.
This could be useful if you need to install napari as a package from PyPI, and prevents creating environments where napari does not start or work properly.

The constraints files are stored in the napari repository under `resources/constraints/constraints_py3.10.txt`. To find
constraints for specific releases, go under the link `https://github.com/napari/napari/tree/{tag}/resources/constraints`
replacing `{tag}` with the desired napari version.

```sh
pip install napari[backend_selection] -c path/to/constraints/file
```

For example, if you would like to install napari on python 3.10:

```sh
pip install napari[all, pyqt] -c constraints_py3.10.txt
```

## Install as a bundled app

napari can also be installed as a bundled app on each of the major platforms,
MacOS, Windows, and Linux with a simple one-click download and installation
process. You might want to install napari as a bundled app if you are unfamiliar
with installing Python packages or if you were unable to get the installation
process described above working. The bundled app version of napari is the same
version that you can get through the above described processes, and can still be
extended with napari plugins installed directly via the app.

```{important}
Note that the bundled app is still
in active development, and may not be very stable. We strongly recommend
[installing as a Python package instead](install-python-package).
```

To access the cross platform bundles you can visit our [release
page](https://github.com/napari/napari/releases) and scroll to the release you
are interested in. For example, the bundles for napari {{ napari_version }} can be
accessed {{ '[here](https://github.com/napari/napari/releases/tag/vNAPARI_VER)'.replace('NAPARI_VER', napari_version) }}.
To get to the download link, just scroll all the way to bottom of the page and
expand the `Assets` section. You can then download the appropriate file for your platform.


<!-- #endregion -->

## Next steps

- to start learning how to use napari, checkout our [getting started](launch) tutorial
- if you are interested in
contributing to napari please check our [contributing
guidelines](napari-contributing)
- if you are running into issues or bugs, please open a [new issue](https://github.com/napari/napari/issues/new/choose) on our [issue
tracker](https://github.com/napari/napari/issues)
- if you want help using napari, we are a community partner on the [imagesc
forum](https://forum.image.sc/tags/napari) and all usage support requests should
be posted on the forum with the tag `napari`. We look forward to interacting
with you there!
