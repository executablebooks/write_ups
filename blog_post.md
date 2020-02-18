# The Executable Book Project

Over the last several years, Jupyter Notebooks have become a staple in the
data scientist’s toolkit. As both an interface for interactive computation,
and a document standard for structuring blocks on computational and narrative
content, notebooks are both ubiquitous, flexible, and powerful in a variety of
tasks involving data and code.

Since its inception, notebooks were imagined to be used as part of many
communication processes for scientists and educators - for example, as a part
of journal publishing pipelines. However, there are still several gaps that
exist in the currently-existing Jupyter Notebook landscape when it comes to
writing and publishing.

With this post we announce the Executable Book Project - a Sloan-funded grant
to improve the state of writing and publishing with Jupyter Notebooks (and the
broader open source science ecosystem). It is a collaboration between
Australia National University, UC Berkeley, and Northern Arizona University.
We’ll cover some of the major challenges we hope to tackle below.

## What does it mean to publish executable documents?

We think of this process in three main parts: authoring, execution, and
publishing.

Authoring is when the writer creates their content - they may be doing this in
a notebook-focused interface like the Jupyter Notebook UI or Jupyter Lab, or
they may do this in a traditional text editor. They require the ability to
enrich their document with information such as equations, citations, and
cross-references.

Execution occurs after the author has written their content and wants to
prepare it for publishing. In this step, all executable content needs to be
run against one or more languages. Moreover, they may not want to run
*everything* each time they publish if their documents take a long time to
run.

Publishing occurs once all of the content has been written and run. It is the
final step where the author builds “final” artifacts from their build process.
These artifacts might take many forms - the two most-common ones being an HTML
website and a PDF.

## How can we improve authoring?

### Use the power of Sphinx

Sphinx is a widely-utilized part of the Python ecosystem. It is an open-source
documentation generation engine that has been used across many projects of
varying complexity. Sphinx has native support for many of the features one
needs for publishing, and a strong community that has built on top of the
platform to extend it in new ways.

We aim to build tools that ease the use of the Sphinx ecosystem using
notebooks and executable content. This will allow us to leverage other
community-driven tools as much as possible, and will also help us create
modular tools that can be used in a context outside of publishing.

However, Sphinx natively supports a markup language that is uncommon outside
of Python - reStructuredText (rST). rST is extremely powerful, but it is not
support in notebooks and has less traction than the markdown landscape. There
have been attempts at supporting markdown in Sphinx, but many have fallen
short of a fully-functional equivalent to reStructuredText. This brings us to
our next point...

### Extend Jupyter’s markup syntax

The biggest challenge that Jupyter Notebooks face when it comes to authoring
is that the language in which users write their content doesn’t have many of
the features on needs for publishing. This is a flavor of markdown called
CommonMark, a community-accepted standard in the markdown landscape.
CommonMark doesn’t have native support for things like citations and
equations, and more generally it has no natural place to “extend” its
functionality in new directions.

In order for notebooks to be used as a part of publishing pipelines, we need a
stronger syntax than notebooks natively support. There are many other options
to explore - for example, RMarkdown is a popular flavor of markdown produced
by RStudio. reStructuredText is a different kind of markup language that is
popular in Python and has many features that are needed for publishing.
Moreover, because we wish to utilize Sphinx, we also need to have markdown
equivalents for many of the most-popular rST components - roles and
directives.

We aim to selectively extend the CommonMark format to utilize the
most-powerful parts of the Sphinx ecosystem. Our goal is to keep the
simplicity and readability of CommonMark markdown, while providing the option
of adding the power and extensibility of rST. Importantly, we want any
pre-existing notebook to be a valid document in the new build systems we
create.

If such a new markup syntax were to exist, then it would mean users can
leverage a wide range of tools in the Sphinx ecosystem to build fully-featured
documents from their work. However, this may increase the complexity of the
documents themselves, which may put a strain on collaborative workflows with
co-authors. For this reason, we wish to improve collaborative workflows while
authoring documents.

### Create two-way conversions between text and ipynb files

Authors often wish to collaborate on their work with others, and the Jupyter
Notebook format breaks down when common collaborative tools such as GitHub are
used. These platforms are optimized around *textual representation of
content*, as opposed to JSON bundles that Jupyter Notebooks provide.

We wish to create a text-based representation of notebooks. This should
utilize the extended markdown syntax we descibe above, and provide options for
two-way conversion between ipynb files and their text-based format. This will
hopefully leverage pre-existing tools for text-based synchronization in the
Jupyter landscape, such as the Jupytext package for two-way sync.

### Interactive authoring with a Language Server Protocol implementation

Finally, as authors are now writing and collaborating with a fully-powered
markup language, they will have more power at their fingertips. To help manage
this extra complexity, we hope to build tooling for providing interactive
feedback and support as they write. For example, beginning a “citation” should
provide a list of available citations linked to a bibtex file. Starting a
“directive” should give a drop-down list of available options.

We hope to build Language Server Protocol implementations for the extended
Markdown syntax described above, as well as for its connection to the Sphinx
ecosystem. This will allow for the rapid, enriched feedback described above in
an inter-face agnostic fashion (most modern editors support implementations in
the LSP protocol).

### Wrapping up

At the end of this process, authors will have a folder full of content they
have created. It will be written in either “IPYNB” or “markdown” files, and
will contain an extended markdown syntax that can be used within the Sphinx
ecosystem for publishing. Now, it is time to execute this computational
content, and populate the notebooks with their outputs.

## How can we improve executing and cacheing.

In order to efficiently execute a collection of notebooks, we need the ability
to represent collections of notebooks, to determine whether it is necessary to
execute their code, and to run execution efficiently.

### Storing collections of notebooks

The simplest way that one stores a collection of notebooks is simply by having
a folder on your computer that is full of notebooks. However, there are a
number of drawbacks to this approach. For example, fetching content inside of
a notebook is an inefficient process, storing information across notebooks is
difficult, and storing notebooks anywhere other than your local filesystem is
impossible.

We aim to improve the tooling available to represent collections of Jupyter
Notebooks. These should exist in an abstract fashion that is agnostic to the
way in which the notebooks are stored. For example, it should be possible for
developers to write plugins that connect notebooks with local filesystems,
cloud storage, or a database. This will be a generically-useful tool that we
will utilize as a part of the build, but that we hope to be useful across many
other projects as well.

Once notebooks are stored in a cache, we also with to execute their code and
populate them with outputs.

### Efficient execution of notebooks

After notebooks are cached, it is time to execute them. We don’t want this to
happen *every* time you build your book. If you’ve already executed your book
content before, then usually we just need to re-use the same outputs without
wasting time re-executing code. We don’t even want this to happen every time
you change your book content. For example, what if you only changed a period
or altered a word? That shouldn’t change the outputs of running any code.

We plan to build an efficient execution engine that builds upon the cacheling
layer described above. It will compare the notebook cache against any new
additions that are made to the original content files. It will then execute
the content inside those notebooks in an efficient manner (ideally, this will
also be pluggable in order to leverage different kinds of computational
resources).

### Wrapping up

At the end of this process, we have two different collections of items: the
source files that have been written by the author, and a cache of notebooks
that are fully-populated with content and enriched metadata. It is now time to
build these tools and produce some outputs.

## Improving the publishing process
