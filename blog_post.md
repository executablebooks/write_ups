# The Executable Book Project

Over the last several years, Jupyter Notebooks have become a staple in the
data scientist’s toolkit. As both an interface for interactive computation
and a document standard for structuring blocks on computational and narrative
content, notebooks are ubiquitous, flexible, and powerful for a variety of
tasks involving data and code.

Since their inception, notebooks were aimed at many
communication processes for scientists and educators, including
journal publishing pipelines. However, there are still several gaps that
exist in the current Jupyter Notebook landscape in terms of
writing and publishing.

With this post we announce the Executable Book Project - a Sloan-funded grant
to improve the state of writing and publishing with Jupyter Notebooks (and the
broader open source science ecosystem). The project is a collaboration between
Australia National University, UC Berkeley, and Northern Arizona University.
We’ll cover some of the major challenges we hope to tackle below.

## What does it mean to publish executable documents?

We think of this process in three main parts: authoring, execution, and
publishing.

Authoring is when the writer creates their content. They may do this in
a notebook-focused interface like the Jupyter Notebook UI or Jupyter Lab, or
in a traditional text editor. They require the ability to
enrich their document with information such as equations, citations, and
cross-references.

Execution occurs after the author has written their content and wants to
prepare it for publishing. In this step, all executable content needs to be
run against one or more languages. Authors may or may not want to run
everything each time they publish, depending on how long the components take to 
run.

Publishing occurs once all of the content has been written and run. It is the
final step where the author builds “final” artifacts from their build process.
These artifacts might take many forms - the two most-common ones being an HTML
website and a PDF.

## How can we improve authoring?

### Use the power of Sphinx

Sphinx is a widely-utilized part of the Python ecosystem. It is an open-source
documentation generation engine that has been used across many projects of
varying complexity. Sphinx has native support for many of the features
necessary for publishing, as well as a strong community that has built on top of the
platform to extend it in new ways.

We aim to build tools that ease the use of the Sphinx ecosystem using
notebooks and executable content. This will allow us to leverage other
community-driven tools as much as possible, while helping us create
modular tools that can be used in a context outside of publishing.

However, Sphinx natively supports a markup language that is uncommon outside
of Python - reStructuredText (rST). Although rST is extremely powerful, it is not
supported in notebooks and has less traction than the markdown landscape. While there
have been attempts at supporting markdown in Sphinx, most have fallen
short of a fully-functional equivalent to reStructuredText. This brings us to
our next point...

### Extend Jupyter’s markup syntax

The biggest challenge that Jupyter Notebooks face when it comes to authoring
is that the language in which users write their content 
--- a community-accepted flavor of markdown called CommonMark ---
doesn’t have many of the features one needs for publishing. 
For example, CommonMark doesn’t have native support for citations and
equations. More generally, it has no natural means to extend its
functionality in new directions.

We aim to selectively extend the CommonMark format to utilize the
most powerful parts of the Sphinx ecosystem. Our goal is to keep the
simplicity and readability of CommonMark markdown, while providing the option
of adding the power and extensibility of rST. Importantly, we want any
pre-existing notebook to be a valid document in the new build systems we
create.

If such a new markup syntax were to exist, then it would mean users can
leverage a wide range of tools in the Sphinx ecosystem to build fully-featured
documents from their work. However, this may increase the complexity of the
documents themselves, putting a strain on collaborative workflows with
co-authors. For this reason, we wish to improve collaborative workflows while
authoring documents.

### Create two-way conversions between text and ipynb files

Authors often wish to collaborate with others. The Jupyter
Notebook format inhibits this process when common collaborative tools such as GitHub are
used. These platforms are optimized around *textual representation of
content*, as opposed to the JSON bundles that Jupyter Notebooks provide.

We wish to create a text-based representation of notebooks. This should
utilize the extended markdown syntax we descibe above, and provide options for
two-way conversion between ipynb files and their text-based format. This will
hopefully leverage pre-existing tools for text-based synchronization in the
Jupyter landscape, such as the Jupytext package for two-way sync.

### Interactive authoring with a Language Server Protocol implementation

Authors writing and collaborating with a fully-powered
markup language will have more power at their fingertips. To help manage
this extra complexity, we aim to build tooling for providing interactive
feedback and support as they write. For example, beginning a “citation” should
provide a list of available citations linked to a bibtex file. Starting a
“directive” should give a drop-down list of available options.

We hope to build Language Server Protocol implementations for the extended
Markdown syntax described above, as well as for its connection to the Sphinx
ecosystem. This will allow for the rapid, enriched feedback described above in
an interface agnostic fashion (most modern editors support implementations in
the LSP protocol).

### Wrapping up

At the end of this process, authors will have a folder full of content they
have created. It will be written in either “IPYNB” or “markdown” files, and
will contain an extended markdown syntax that can be used within the Sphinx
ecosystem for publishing. Now it is time to execute this computational
content and populate the notebooks with their outputs.

## How can we improve executing and cacheing.

In order to efficiently execute a collection of notebooks, we need the ability
to represent collections of notebooks, to determine whether it is necessary to
execute their code, and to run execution efficiently.

### Storing collections of notebooks

The simplest way to store a collection of notebooks is in
a folder on your computer. However, there are a
number of drawbacks to this approach. For example, fetching content inside of
a notebook is inefficient, storing information across notebooks is
difficult, and storing notebooks anywhere other than your local filesystem is
not possible.

We aim to improve the tooling available to represent collections of Jupyter
Notebooks. These should exist in an abstract fashion that is agnostic to the
way in which the notebooks are stored. For example, it should be possible for
developers to write plugins that connect notebooks with local filesystems,
cloud storage, or a database. This will be a generically-useful tool that we
will utilize as a part of the build, but also aim to be useful across many
other projects as well.

Once notebooks are stored in a cache, we also wish to execute their code and
populate them with outputs.

### Efficient execution of notebooks

Once notebooks are cached it is time to execute them. We don’t want this to
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
