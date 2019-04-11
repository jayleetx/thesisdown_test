Working with `thesisdown`
================

  - [Introduction](#introduction)
  - [Setup](#setup)
  - [File structure](#file-structure)
  - [Dealing with files](#dealing-with-files)
  - [Object referencing](#object-referencing)
  - [Extra fun tidbits](#extra-fun-tidbits)
  - [Useful links](#useful-links)

## Introduction

[`thesisdown`](https://github.com/ismayc/thesisdown) is the R Markdown
template for Reed College’s senior thesis. It’s built on
[`bookdown`](https://bookdown.org/yihui/bookdown/), a more general
format for “authoring books and technical documents with R Markdown”.

## Setup

1.  Prerequisites: install [R,
    RStudio](https://www.reed.edu/data-at-reed/resources/R/installr.html),
    and LaTeX ([Windows](https://miktex.org/download) |
    [Mac](http://tug.org/mactex/mactex-download.html)).

2.  Open RStudio and install the `thesisdown` package.

<!-- end list -->

``` r
if (!require("remotes")) install.packages("remotes")
if (!require("bookdown")) install.packages("bookdown")

remotes::install_github("ismayc/thesisdown")
```

3.  Restart RStudio, create a new R Markdown document, and select the
    thesis format (*File / New File / R Markdown / From Template /
    Thesis*, on Mac). **Be sure to name the directory `index`**, or else
    it won’t knit properly. Click *OK* to create the document.
    
    ![Options for new thesis document](new_thesis_doc.png)

You can also create this from the R Console with the following code,
exchanging the file path for wherever you want the thesis to live.

``` r
rmarkdown::draft(file = "~/Desktop/thesis/index",
                 template = "Thesis",
                 package = "thesisdown",
                 create_dir = TRUE,
                 edit = FALSE)
```

Again, **be sure to name the final folder `index`** to knit properly.
The folder immediately above `index` must already exist in order for
this to run without errors.

## File structure

The blank thesis template now lives inside of the `index` directory in
your chosen file location, and contains the following files and folders:

  - [`00--prelim.Rmd`](index/00--prelim.Rmd). Acknowledgements, preface,
    and dedication. Optional.

  - [`00-abstract.Rmd`](index/00-abstract.Rmd). Abstract.

  - [`01-chap1.Rmd`](index/01-chap1.Rmd). Chapter 1.

  - [`02-chap2.Rmd`](index/02-chap2.Rmd). Chapter 2.

  - [`03-chap3.Rmd`](index/03-chap3.Rmd). Chapter 3.

  - [`04-conclusion.Rmd`](index/04-conclusion.Rmd). Conclusion.

  - [`05-appendix.Rmd`](index/05-appendix.Rmd). Appendix.

  - [`99-references.Rmd`](index/99-references.Rmd). Reference
    formatting, actual bibliography is generated from `bib` upon knit.

  - [`_bookdown.yml`](index/_bookdown.yml). A YAML file to define how
    the multiple Rmd files are combined.

  - [`bib`](index/bib). Contains a BibTeX file for citations. Replace
    with your own.

  - [`chemarr.sty`](index/chemarr.sty). LaTeX macro creating some
    extra-long hooked arrows for chemistry reactions. Optional.

  - [`csl`](index/csl). Contains a CSL file to define the APA format of
    citations. You can download CSLs for other citation formats
    [here](https://www.zotero.org/styles) to replace this one.

  - [`data`](index/data). Folder for data. Optional.

  - [`figure`](index/figure). Folder for images. Optional.

  - [`index.Rmd`](index/index.Rmd). This is the “master” R Markdown
    file. Defines the front of the thesis: name, advisor, major, etc.
    Also includes the introduction.

  - [`reedthesis.cls`](index/reedthesis.cls). Creates the thesis
    document class for LaTeX (do not edit).

  - [`template.tex`](index/template.tex). LaTeX thesis template (do not
    edit).

You can write directly into the `index.Rmd` and numbered files. To
create the actual thesis, pick which output you want in the `index.Rmd`
YAML by commenting (\#) out the lines for `output` that you don’t want,
then knitting (this defaults to GitBook, so you need to set it to PDF).
After knitting for the first time, the following folders appear:

  - `_book`. This contains the generated documents for the thesis: the
    PDF thesis, and the TeX file that generated the PDF (among other
    things).

  - `_bookdown_files`. Contains caches and images created in the thesis
    document (plots, etc.).

The actual generated thesis lives at:

  - [`_book/thesis.pdf`](index/_book/thesis.pdf) (PDF)

  - [`_book/index.html`](index/_book/index.html) (GitBook)

  - [`_book/thesis.docx`](index/_book/thesis.docx) (Word)

  - [`_book/thesis.epub`](index/_book/thesis.epub) (EPUB)

Note that ONLY the PDF file actually complies to the thesis formatting
requirements. The others are just extras.

R also creates some intermediate files in the knitting process, which
are deleted when the knit completes. If this breaks along the way, this
is where you can troubleshoot:

  - `thesis.Rmd`. When the R code breaks and throws an R error, it will
    reference which line the error is on (or rather, which line the
    chunk containing the error starts on). The line number it cites is
    in relation to this file, which is the combination of the multiple
    chapter files that actually knits to directly become the thesis pdf.

  - `thesis.log`. This is the TeX log file, which will detail any errors
    in the LaTeX formatting after the R code is done running. The bottom
    of this file usually has any errors, and the line numbers it cites
    refers to `thesis.tex`.

## Dealing with files

#### `index.Rmd`

  - Be sure to update the YAML header here\! Name, date, division,
    advisor, department, title, output, acknowledgements, dedication,
    preface. These all need to be customized for your thesis.

  - Double check that the files referenced for `bibliography` and `csl`
    are correct. If you change the citation format or have a differently
    named file,

  - `lot` and `lof` are “List of tables” and “List of figures”
    respectively, leave this as true to match the thesis formatting
    requirements.

  - To include extra LaTeX packages, uncomment lines 43-44 and
    change/add packages as needed.

  - Your introduction gets written directly into this file, under the
    “Introduction” header.

#### Adding, subtracting, or renaming chapters

By default, [`_bookdown.yml`](index/_bookdown.yml) handles the chapter
ordering. The third line of this file (`rmd_files`) defines which files
are combined to create the actual thesis document. You can add a chapter
by adding it to the directory and this list, and delete one by removing
it from the list.

#### Appendix

If you have code in one of the Rmd files that breaks the visual flow of
the thesis but should still be included, the appendix is a good place to
display this. For an example of how to do this, look at the
`include_packages` chunk in [`index.Rmd`](index/index.Rmd) (starting on
line 59). To hide this code, the `include = FALSE` option was set. The
output of the chunk, if any is produced, will still be displayed.

The code is displayed using the chunk on line 16 of
[`05-appendix.Rmd`](index/05-appendix.Rmd). It uses the name of the
original chunk to duplicate it here, then sets `results = 'hide'` to
only show the code and hide the output.

#### Abstract and prelim

## Object referencing

#### In-line code

#### Tables and figures

#### Sections

#### Chunks?

#### Citations

## Extra fun tidbits

#### File path issues

R Markdown has some non-intuitive behaviors when it comes to relative
file names. A typical structure in R repositories is the Project (big
P), which is meant to be a self-contained folder containing everything
needed for a project (small p) you’re working on. The working directory
is generally set to the root of the Project, so relative file names are
indexed to this location. When you knit an R Markdown document, however,
it temporarily sets the parent directory of the Rmd file to be the
working directory. This can mess up any relative file paths which ran
fine in lin-by-line code tests (indexed to the Project), but are now
breaking your knit (indexed to the Rmd file).

For example, maybe your `index` directory lives inside of an R Project
foldered as `thesis`, with your data inside the subfolder `data`. When
running code in the console, your data can be accessed through
`data/file.csv`. When running it in the thesis Rmd file (inside
`index`), however, R (correctly) recognizes that there is nothing at
`index/data/file.csv` and will fail to read the file. You would then
have to change your Rmd script to the file path `../data.file/csv`, but
you then lose the ability to run it in the console.

Possible fixes:

  - The `here` package, which standardizes relative file locations.
    After loading it once for the project (`library(here)`), using the
    command `here('data', 'file.csv')` (from the Project directory) in
    place of any call to `file.csv` will automatically use the correct
    relative path based on the current working directory. This is the
    best way to maintain reproducibility and portability in your
    analyses. It also abstracts this file reference problem away so you
    don’t have to think about it\!

  - Place everything inside `index`. This is an option, but it can be
    useful to have thesis-related things outside of the `index`
    structure: data, Zotero libraries, etc.

  - Use `setwd()` at the top of your Rmd file. This is bad because you
    can lose the ability to call things directly from the console, and
    any change in directory setup (new computer, RStudio server, etc.)
    will require a change - which is not a good reproducible practice.

  - Use full absolute file paths all the time. Bad because of the same
    directory change as above (also gets tedious).

#### YAML formatting

YAML (“Yet Another Markup Language”, or “YAML Ain’t Markup Language”) is
a protocol for defining the metadata for a document: title, author,
output format, whether there should be a table of contents, etc.

#### Why does it have to be named `index`?

One of the formats you can knit your thesis to is GitBook, which is
built on top of some HTML. This format is a webpage, so the “home” of
the page needs to be called `index`. If you’re only knitting to pdf, you
can maybe change this, but it’s best to leave it as is just in case…

#### Underscore issues

A problem: R people really like underscores to separate words in object
names (dots represent something else), but LaTeX doesn’t like
underscores (they’re a special character, so displaying them verbatim is
tricky sometimes). It’s hard to anticipate every case where this is an
issue, but the [`xtable`
package](https://cran.r-project.org/web/packages/xtable/vignettes/xtableGallery.pdf)
is a good option for displaying a table that has underscores in column
names or data.

One other issue is with code chunk names containing underscores. To
reference a table in the text, [you can reference
it](https://bookdown.org/yihui/bookdown/tables.html) with the name of
the chunk that created it. If the chunk name has an underscore, however,
the LaTeX reference created will break because of this. So avoid naming
chunks with underscores if you’re referencing tables this way.

## Useful links

  - [Reed thesis
    help](https://www.reed.edu/cis/help/thesis/index.html#template).

  - [Reed R
    help](https://www.reed.edu/data-at-reed/resources/R/index.html).

  - [`thesisdown` GitHub
    repository](https://github.com/ismayc/thesisdown).

  - [`bookdown` documentation](https://bookdown.org/yihui/bookdown/).

  - [‘huskydown’ package](https://github.com/benmarwick/huskydown),
    similar to thesisdown but with more user guidance.

  - [`citr` RStudio add-in](https://github.com/crsh/citr), for
    automating some of the Markdown citation process.

  - [Blog
    posts](https://rosannavanhespenresearch.wordpress.com/2016/02/03/writing-your-thesis-with-r-markdown-1-getting-started/)
    on writing a
    [thesis](https://eddjberry.netlify.com/post/writing-your-thesis-with-bookdown/)
    with R Markdown.

  - [Chester](https://github.com/ismayc)’s old
    [slides](http://www.rpubs.com/cismay/twatchers_2k16_rmd).
