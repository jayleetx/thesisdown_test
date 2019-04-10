Working with `thesisdown`
================

[`thesisdown`](https://github.com/ismayc/thesisdown) is the R Markdown
template for Reed College’s senior thesis. It’s built on
[`bookdown`](https://bookdown.org/yihui/bookdown/), a more general
format for “authoring books and technical documents with R Markdown”.

NB: There’s one or two menu selection paths below, which are written out
as needed to access them using MacOS. Results may vary on Windows.

## Setup

1.  Prerequisites: install [R,
    RStudio](https://www.reed.edu/data-at-reed/resources/R/installr.html),
    and LaTeX ([Windows](https://miktex.org/download) |
    [Mac](http://tug.org/mactex/mactex-download.html)).

2.  Open RStudio and install the `thesisdown` package.
    
    ``` r
    remotes::install_github("ismayc/thesisdown")
    
    # if that throws an error, try installing the following packages
    # install.packages('remotes')
    # install.packages('bookdown')
    ```

3.  Restart RStudio, create a new R Markdown document, and select the
    thesis format (*File / New File / R Markdown / From Template /
    Thesis*). **Be sure to name the directory `index`**, or else it
    won’t knit properly. Click *OK* to create the document.
    
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
your chosen file location, and contains the following files and folders.

  - `00--prelim.Rmd`. Acknowledgements, preface, and dedication. *NOTE*
    why is this (and the abstract) duplicated in the `index.Rmd`, and
    what’s the best way to do it?

  - `00-abstract.Rmd`. Abstract.

  - `01-chap1.Rmd`. Chapter 1.

  - `02-chap2.Rmd`. Chapter 2.

  - `03-chap3.Rmd`. Chapter 3.

  - `04-conclusion.Rmd`. Conclusion.

  - `05-appendix.Rmd`. Appendix.

  - `99-references.Rmd`. References, generated automatically through
    citations.

  - `_bookdown.yml`. A YAML file to define how the multiple Rmd files
    are combined.

  - `bib`. Contains a BibTeX file for citations.

  - `chemarr.sty`. LaTeX macro creating some extra-long arrows for
    chemistry reactions.

  - `csl`. Contains a CSL file to define the APA format of citations.

  - `data`. Folder for data.

  - `figure`. Folder for images.

  - `index.Rmd`. This is the “master” R Markdown file. Defines the front
    of the thesis: name, advisor, major, etc. Also includes the
    introduction.

  - `reedthesis.cls`. Creates the thesis document class.

  - `template.tex`. LaTeX thesis template.

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
foldered as `thesis`, which is a useful structure for holding other
things such as data, Zotero libraries, etc. When running code in the
console, your data can be accessed through `data/file.csv`. When running
it in the thesis Rmd file (inside `index`), however, R (correctly)
recognizes that there is nothing at `index/data/file.csv` and will fail
to read the file. You would then have to change your Rmd script to the
file path `../data.file/csv`, but you then lose the ability to run it in
the console.

Possible fixes:

  - Place everything inside `index`.

  - Use `setwd()` at the top of your Rmd file.

  - Use full absolute file paths all the time.

  - The `here` package, which standardizes relative file locations.
    After loading it once for the project (`library(here)`), using the
    command `here('data', 'file.csv')` (from the Project directory) in
    place of any call to `file.csv` will automatically use the correct
    relative path based on the current working directory.

## Useful links

  - [Reed thesis
    help](https://www.reed.edu/cis/help/thesis/index.html#template)

  - [Reed R
    help](https://www.reed.edu/data-at-reed/resources/R/index.html)

  - [`thesisdown` GitHub
    repository](https://github.com/ismayc/thesisdown)

  - [`bookdown` documentation](https://bookdown.org/yihui/bookdown/)
