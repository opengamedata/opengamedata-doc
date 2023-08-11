# Documenting OpenGameData

Inevitably, for any sufficiently large project, a few words of that project's documentation must be spent describing how the project is documented.
This section of the **OpenGameData** (**OGD**) documentation does just that.

The tech stack for the `opengamedata-doc` project can be roughly broken down as follows:

- Storage : **GitHub**
- Build : **Sphinx**
- Deployment : **readthedocs** (**rtd**)
- Implementation : **reStructuredText** (**ReST**) and **Markdown**

## Storage

The documentation source is stored in a project [GitHub](https://github.com/opengamedata/opengamedata-doc).
There's not a lot else to say about this.
If you don't have experience with **Git**, you might start with the [git-scm docs](https://git-scm.com/doc).
From there, you can learn about the services offered by **GitHub** with its [getting started](https://docs.github.com/en/get-started) guide.

## Build

We use the **Sphinx** system to build the `opengamedata-doc` documentation from source into a usable HTML website.
The **Sphinx** build process is configured in `source/conf.py`
There's nothing particularly special about our configuration, but you can read about **Sphinx** configuration generally on the [sphinx-doc](https://www.sphinx-doc.org/en/master/usage/configuration.html) website.

We use several **Sphinx** extensions to support building various implementation languages into our documentation.
We will link to individual extensions' pages as they are discussed below; for general information on **Sphinx** extensions, see the sphinx-doc [extensions page](https://www.sphinx-doc.org/en/master/usage/extensions/)

## Deployment

The documentation is deployed to our [readthedocs](https://opengamedata-doc.readthedocs.io/en/latest/index.html) site after building.
Further, the project is configured via webhook to rebuild and deploy every time changes are pushed to the `opengamedata-doc` GitHub project.
Configuration of the **readthedocs** build process lives in the `.readthedocs.yaml` file (see [here](https://docs.readthedocs.io/en/stable/config-file/v2.html) for **rtd**'s config documentation)

## Implementation

To write the **OGD** documentation, we use a combination of **reStructuredText** (**ReST**), **Markdown**, and a small handful of other special-use formats.
**ReST** is the default format for writing documentation used by the **Sphinx** build system.
Other formats, including **Markdown**, are supported via **Sphinx** extensions (e.g. the `myst_parser` extension).

**ReST** has a steep learning curve but offers significant features for controlling the structure of a document, as its name suggests.
**Markdown** is a markup language that is simple, easy to understand, and fast to write.
Thus, use **Markdown** to write most major sections of documents, and use **ReST** to assemble the individual **Markdown** docs into well-structured pages.

### Markdown

We use **Markdown** as the primary way to write documentation of the OpenGameData system.
As mentioned above, **Sphinx** prefers **reStructuredText**, so we utilize the `myst-parser` extension to render **Markdown** documents.
This extension actually uses an implementation of **Markedly Structured Text**, or **MyST**.
**MyST** is built on the **CommonMark** specification, which is a particular dialect of **Markdown**, but adds some functionality of its own.

In general, we strive to only use the features of **Markdown** that are common across most or all **Markdown** dialects.
Any cases requiring advanced functionality should just be written in **ReST**.
The main exception to this rule is the use of $\LaTeX$-style math parsing, which is covered in the "Special Cases" section below.
That said, it may be helpful to be aware of non-standard things done by the parser we use.
For nitty-gritty details, see the latest version of the
  [CommonMark specification](https://spec.commonmark.org/current/),
as well as **MyST**'s
  [further extensions](https://myst-parser.readthedocs.io/en/latest/syntax/typography.html)
of **CommonMark**.

#### Primer

The [Markdown Guide](https://www.markdownguide.org/) website is a good reference on the (relatively simple) syntax of **Markdown**, including documentation of several popular extensions to the core **Markdown** language.
In addition, the Markdown Guide has a nice [quick reference](https://www.markdownguide.org/cheat-sheet/) on the most common syntax across **Markdown** and **Markdown**-like dialects.

Below, we include our own quick reference, adapted from the quick reference linked above, for the elements we most commonly use. The "Our Use" column indicates our conventions for when/how to use the various bits of syntax to create a visual language within the documentation. Most uses are obvious, but some are scoped to specific uses:

| Markdown         | Syntax                             | Our Use                        |
| ---              | ---                                | ---                            |
| Heading          | \# H1                              | Doc Headers                    |
|                  | \#\# H2                            | Section Headers                |
|                  | \#\#\# H3                          | Subsection Headers             |
|                  | etc...                             | etc...                         |
| **Bold**         | \*\*Bold\*\*                       | System/Tool Names              |
| *Italic*         | \*Italic\*                         | Keywords                       |
| Quote            | \> Quote                           | Keyword Definitions            |
| `code`           | \`code\`                           | Variable/Class/Library Names   |
| ```code block``` | \`\`\`code block\`\`\`             | Example Code                   |
| [Link](.)        | \[Link\]\(Target URL\)             | Links to Other Pages           |
| Unordered List   | \- Item 1                          | Lists of Items                 |
|                  | \- Item 2                          |                                |
|                  | \- ...                             |                                |
| Table            | \| Column 1 \| Column 2 \| ... \|  | Tables of information          |
|                  | \| -------- \| -------- \| -   \|  |                                |
|                  | \| Value 1  \| Value 2  \| ... \|  |                                |

### reStructuredText

#### Primer

### Special Cases

As mentioned, we have a few special cases that use special formats outside of **ReST** and **Markdown**:

- **$\LaTeX$ formatting**: The `myst-parser` extension supports further extensions of its own, described on `myst-parser`'s own [readthedocs site](https://myst-parser.readthedocs.io/en/latest/syntax/optional.html).
  We use `dollarmath` and `amsmath` to support inline use of $\LaTeX$-style implementation of mathematics content
  ([myst-parser page](https://myst-parser.readthedocs.io/en/latest/syntax/optional.html#math-shortcuts)).

- **Code-level documentation**: We use the `autodoc` extension of **Sphinx**, which turns module and function comment blocks into readable API reference
  ([sphinx-doc page](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#module-sphinx.ext.autodoc)).

- **Graphs and Visualizations**: We use **Graphviz**, a graph definition language enabled by the `graphviz` extension
  ([sphinx-doc page](https://www.sphinx-doc.org/en/master/usage/extensions/graphviz.html)).
  Note that documents using **Graphviz** heavily must be implemented in **ReST**, as **Markdown** does not have a means for rendering in-line **Graphviz** graphics.
