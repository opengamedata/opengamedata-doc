# Open Game Data Software Platform

Below is a list of standard pieces of software & libraries that make up the **OpenGameData Reference Platform**. This **reference platform** is meant as a *convention* for keeping new development coordinated with respect to the development and deployment environments.

The bar for updating a package is low, just need some simple justification like “we need a feature from a newer version to make X better,” or “the current version is old and nobody uses it anymore,” or “another library needs a newer version.” In general, we do not want our projects to become so oddly specific as to be tied to a specific package/version. The onus is on us to keep OGD projects running on newer libraries to keep a low barrier for entry to new collaborators who want to start fresh.

However, any Field Day/WCER project should avoid using a different version of any of these libraries than what is listed. Instead, ask to bump a version and wait for the OK, so we don’t have incompatibilities pop up randomly.

We’ll use this list for GitHub Actions, requirements.txt, and .devcontainer files.
Note: for some libraries, we use .* to indicate any patch version may be used (we only specify patch version for mission-critical dependencies, such as database libraries).

- Python: 3.10 3.9.2
  - build 0.10.*
  - gitdb 4.0.*
  - GitPython 3.1.*
  - google-cloud-bigquery 3.3.6/3.2.0/3.4.0
  - ipywidgets 8.0.*
  - matplotlib 3.7.*
  - mysql-connector-python 8.0.25
  - numpy 1.23.*
  - flask 2.2.*
  - flask-cors 3.0.10
  - flask-restful 0.3.10
  - pandas 1.5.*
  - pyOpenSSL 23.0.0
  - scikit-learn 1.2.*
  - scipy 1.10.*
  - seaborn 0.12.*
  - sshtunnel 0.4.0
  - statsmodels 0.13.*
  - tensorflow 2.11.*
  - twine 4.0.*
- PHP v.8.1.17
- MySQL 15.1
  - Uses MariaDB 10.5.18
