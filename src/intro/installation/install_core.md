## Installation

Before you can use OpenGameData, you'll need to install a couple tools.
First, OpenGameData is implemented in Python, which means you'll need a Python interpreter to run the code, which in turn means you need to have Python installed on your system.

Second, you must determine whether you will run OpenGameData from an installed Python package, or from a local source copy.

### Install Python

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

### Install OpenGameData

#### Installation as Package

#### Installation as Local Source Copy

Second, the OpenGameData source code is hosted on the [GitHub website](www.github.com/opengamedata/).
GitHub is built for use with `Git`, a popular version control system we use for source control and distribution.
While it is possible to download and use the **`OpenGameData`** source without `Git`, we strongly recommend using `Git` to obtain the source.
This will allow you to track any changes you make, and to optionally contribute your changes back to the **`OpenGameData`** community.  
We *strongly* recommend [installing and using Git](#install-git) so that you can keep your OpenGameData code up-to-date, and participate in the OpenGameData community by contributing your innovations back the public.

##### Download or Clone OpenGameData

<font style="color:tomato">STUB: The rest of this page is on the to-do list; only section headers have been added so far.</font>

##### Install Python Libraries

### Other Tools

#### Install Git

This step is necessary if you would like to submit changes back to the OpenGameData community for inclusion in the public repository.

We do not specify a minimum (or maximum) recommended version of `Git` to use.
You can obtain a `Git` installer from <https://git-scm.com/downloads>.  
If you are not comfortable working from a command line, consider downloading an installer for a GUI client (included in the linked page above).  

<font style='color:tomato'>TODO: add some recommendations for any customization options in installer.</font>  
