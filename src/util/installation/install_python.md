## Install Python

As a first step, you should ensure Python is installed on your system.
The quickest way to check is to open up a command prompt (often called a terminal) on your system, and typing:  

```bash
  > python --version
```

This will output something like the following:

```bash
  Python 3.12.4
```

Currently, OpenGameData is meant to run on Python 3.8 and higher.
If you have an older version of Python, but it is at least Python 3, you should think about updating to a newer Python version to avoid incompatibilities.  
If your Python is still on version 2, you should try running:

```bash
  > python3 --version
```

This should tell you if there is a separate installation of Python 3 on your system.
If there is, then you should be good to go.
You just need to be aware that you will need to use `python3` instead of `python` if you want to run data exports with the provided `main.py` (as documented in the "Getting Started" page).

If there is no Python 3 installation, or no Python install at all (i.e. if the command(s) above resulted in an error message), you'll need to perform a brand new installation of Python.
Official Python downloads are available at the Python downloads page: <https://www.python.org/downloads/>
