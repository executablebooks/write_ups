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

## `tl;dr`: what are we hoping to build?

From a user's perspective, this is what we want to enable:

From a single folder with either markdown or Jupyter Notebooks, generate
a high-quality book from this collection of documents. The book should support
features common in publishing, such as citations, cross-references, and
equations. As a part of this process, all executable code in the content files
should be run (and cached) and included along with the book's outputs. These
outputs should include high-quality HTML and PDF. Finally, this process
should be managed by a simple command-line interface that allows users to
focus on the content creation process, not on the backend stack that runs the
actual building.

This stack should be entirely open source

## What are our principles and constraints?

In running this project, we aim to adhere to several principles that we believe
will result in higher-quality technology that aligns with the core principles
of the open source community. Here are a few key components:

* **Support casual users equally to power users**. Complicating the feature
  space to support a "power user feature" should be done with great care.
* **Build modular components that are useful elsewhere**. Rather than building
  a single vertical stack, find parts of the workflow that naturally separate.
  Create modular tools for these parts so that they may benefit the community
  outside of the context of building interactive books.
* **Use pre-existing technology where possible**. Rather than re-inventing
  the wheel, make every effort to utilize pre-existing open source tech.
* **Use pre-existing standards where possible**. In the event that we must
  create new patterns of content creation or tooling, utilize prior art in
  the open source community as much as possible, especially whne it comes to
  markup languages.
* **Contribute improvements upstream**. Where we utilize pre-existing tools,
  contribute improvements to them as we build off of them for this project.
* **Design for the future**. While we have a bit of funding now, it won't last
  forever. This means the technology should be easy for potential developers
  to read, understand, and modify/improve.
* **Users should not need to know anything about the build system**. If they
  want to dig into the guts of our infrastructure, they can, but knowledge
  of Sphinx, Latex, etc should not be a requirement. They should only need to
  use a simple tool to control the process.
* **Don't try to do everything**. Focusing our tools on publishing
  computational documents with reasonable choices made for the user.

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

## How can we improve publishing with notebooks?

Now that we have enriched content in text-based source files, as well as
the cached outputs that are generated from running this content, it is
time to utilize Sphinx to generate a book from our documents.

### A native `ipynb` parser for Sphinx

First, since some content will be written directly in notebooks, it will need
to be natively read into Sphinx. There have been a few attempts at
`ipynb` parsers in the Sphinx ecosystem. However, with the addition of
the extended markdown syntax mentioned above, we can leverage this tool to
make notebooks a first-class citizen in Sphinx.

We plan to build a native `ipynb` reader for the Sphinx ecosystem. This tool
will parse all notebook markdown blocks with our extended markdown parser,
and will insert cached outputs from running each cell into the Sphinx document.
This means that users will also be able to leverage Sphinx directives and
roles from within Notebook markdown.

### Create first class HTML output with Jupyter Book

Once Sphinx generates a document from our collection of source files, we can
use it to generate a number of outputs. One of the most popular is a
**bundle of HTML**. This is a website that has the look-and-feel of a book,
and many interactive features that improve the reading experience.

We plan to leverage the Jupyter Book project to provide a modern, web-based
book outputs for this build system. This involves taking the CSS and Javascript
from the current Jupyter Book implementation and incorporating it into a
Sphinx theme. This theme could work on its own, or as a part of the build
process described in this post.

### Create first-class PDF output

Another common need for authors is to create publication-quality PDFs of
their content. For this purpose, Latex is the right tool for the job. Latex
provides an extreme level of configurability and consistently outputs
professional-looking documents. However, it is known for being difficult
to configure just the way you want it.

We plan to incorporate out-of-the-box support for first-class PDF outputs
when building your book. This will leverage a collection of Latex templates
and configurations that are bundled with the book build system. The output
should be something that is almost ready to send off to a publisher.

## Tying it all together

Finally, as you can see, there is a lot of complexity in all of the above
steps. The final thing that we will build as a part of this project is a
command-line tool that controls the entire process. Given a collection of
content files (and perhaps a configuration file), it will manage the
execution and cacheing, the sphinx build process, and generate outputs for
the latest set of book files. This should be a fairly simple tool that
is user-friendly and has reasonable defaults.
