![](https://i.imgur.com/iywjz8s.png)


# Week 4 - 2023-03-08 Reproducible Research with R Packages

Welcome to The Workshop Collaborative Document.


## 🫱🏽‍🫲🏻 Code of Conduct

Participants are expected to follow these guidelines:
* Use welcoming and inclusive language.
* Be respectful of different viewpoints and experiences.
* Gracefully accept constructive criticism.
* Focus on what is best for the community.
* Show courtesy and respect towards other community members.

Report an issue or get in touch:
- b.vreede@esciencecenter.nl
- p.rodriguez-sanchez@esciencecenter.nl
- training@esciencecenter.nl

## ⚖️ License

All content is publicly available under the [Creative Commons Attribution License 4.0](https://creativecommons.org/licenses/by/4.0/).

## 🙋Getting help

To ask a question, raise your hand in the zoom window, or ask a question in the chat.

You can ask questions in the document or chat window and helpers will try to help you.

## 🖥 Workshop website

💻 [Workshop website](https://esciencecenter-digital-skills.github.io/2023-03-08-ds-rpackaging/)

🛠 [Setup instructions](https://esciencecenter-digital-skills.github.io/2023-03-08-ds-rpackaging/#setup)

## 👩‍🏫👩‍💻🎓 Instructors

Barbara Vreede, Pablo Rodríguez-Sánchez

## 🧑‍🙋 Helpers

Eva Viviani, Ji Qi, Malte Lüken, Thijs Vroegh


## 🗓️ Agenda
Day 4. Wed 29 March 2023
| Time | Topic |
|--:|:---|
| 13:00 	| Welcome and icebreaker
| 13:15 	| Recap homework & lessons learned
| 14:15 	| Coffee break
| 14:30 	| Vignettes
| 15:30 	| Coffee break
| 15:45 	| Sharing our packages
| 16:45 	| Wrap-up
| 17:00 	| END

## 🧑🏽‍💻 Recap homework & lessons learned



### Questions from the breakout rooms:

#### Room 1
- Using a dot notation in the function name can have unintended consequences if the first part of the function name is an existing function. E.g.: `hist.distance()`, where `hist()` is an existing function.

#### Room 2

- A function from the package was called but not exported: This led to an error where the function could not be found (like "Error: Object <fun_name> could not be found")

#### Room 3

- `ggplot` and `dplyr` raise a NOTE: no visible binding for global variable ...
- Can I test plots?
    - https://testthat.r-lib.org/articles/snapshotting.html

#### Room 4
- required packages seem not to be attached correctly:
    - checking examples ... ERROR
      Running examples in ‘surv.html-Ex.R’ failed
    - could not find function "%>%"
    - no visible global function definition for ‘%>%’
        - use the native pipe: |> (or, as code:) `|>`
        - use `usethis::use_pipe()` to set up the `magrittr` pipe
- The packages are added to description using use_this
    - Imports: dplyr

## 🔧 Exercises

### Exercise 1: Write a small vignette about your package or mysterycoffe

- Write a short introduction for your package
- Write an example showcasing at least one of you functions

### Exercise 2: Create a (temporarily public) repository on GitHub for your package

- Create a new repository on GitHub.com with the same name as your package. Do **not** select any of the optional files (e.g., README, .gitignore, etc)
- In the GitHub repo, select "Add files" and "Upload files". Drag and drop the content of your package directory into the upload window. Make sure to **not** upload "hidden" files (starting with a .)
- Press "Commit changes" to finalize the upload and commite the files to your repo
- Update the README.md file in the GitHub repo if necessary, so that the installation instructions are correct
- Paste the URL of the repo into this document under your name



## 🧠 Collaborative Notes

### Recap: Documenting Data in R Packages

Example:

```
example_data <- 42

usethis::use_data(example_data)
```

This adds (if necessary) the R version to `Depends` in the DESCRIPTION. It creates a directory in your package called `data/`, and adds the object as an `.rda` file to that directory.

Create a new file in the `R/` directory, for example called `data.R`. You can do this with `usethis::use_r("data")`, or simply by creating an empty file and saving it as `R/data.R`.
Then, you can document the dataset using a roxygen skeleton. See an example of data documentation [here](https://r-pkgs.org/data.html#sec-documenting-data). Note that the @export tag is not necessary.

### Vignettes

Vignettes are extensions of the documentation. They are used for providing examples or tutorials for your package. They can include figures, tables, and *code*. (NB: You can also write your papers in them.)

Reasons for using vignettes:
- Automatically update your documents when your code changes
- It makes your results/document more reproducible
- Automatically paste your tables, figures, results into the document

Vignettes for R packages use R Markdown (an extension of the Markdown language).

#### Creating Vignettes in an R Package

- Create a subdirectory call `vignettes/` in your root (main) package directory. In this subdirectory create a new R Markdown document (via "File" in R Studio). As the output type you can select Html for now. This can also be done with usethis via: `usethis::use_vignette("example_of_usage")`. usethis will also add the vignette to the DESCRIPTION file and adds the knitr package as a suggested dependency
- You can render the R Markdown document as Html by pressing the "Knit" button in R Studio. This will automatically execute all code (which is marked to be executed) in the document and include resulting output (including plots) in the rendered Html document
- R Markdown documents, you can add R code by adding code *chunks*, e.g.:

Text goes here

```{r <cell_name>}

# R code goes here

```

More text

- The `r <cell_name>` indicates that this code chunk contains R code
- You can add different optionional arguments to each chunk, for example, to suppress the code in the rendered document: `r <cell_name>, echo=FALSE`. Another example are the heights and widths for plots: `r <cell_name>, fig.width=3, fig.height=5`

- You can build vignettes with your package via: `devtools::build_vignettes()` or `devtools::install(build_vignettes = TRUE)`. You can check whether the vignettes are included in the installed package via `browseVignettes("<package_name>")`
- The rticles package contains templates with formats for many different journals
- For code chunks that take a long time to run, you can cache the result by adding the argument `r <cell_name>, cache=TRUE`, this will write the result to a cache memory and only rerun the chunk if its code has changed


## 📚 Resources
[CRAN](https://cran.r-project.org/)
[Using snapshot to test plots](https://testthat.r-lib.org/articles/snapshotting.html)
[R Packages chapter on Vignettes](https://r-pkgs.org/vignettes.html)
[Citation File Format](https://citation-file-format.github.io/cff-initializer-javascript/#/)
[Learn to use Git and Github with R and Rstudio](https://happygitwithr.com/)
