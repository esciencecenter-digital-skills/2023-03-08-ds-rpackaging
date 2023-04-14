![](https://i.imgur.com/iywjz8s.png)


# Week 3 - 2023-03-22 Reproducible Research with R Packages

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

Barbara Vreede, Pablo Rodriguez-Sanchez

## 🧑‍🙋 Helpers

Eva Viviani, Ji Qi, Malte Lüken





## 🗓️ Agenda
Day 3. Wed 22 March 2023
| Time | Topic |
|--:|:---|
| 13:00 	| Welcome and icebreaker
| 13:15 	| Recap homework & lessons learned
| 14:15 	| Coffee break
| 14:30 	| Documenting your package
| 15:30 	| Coffee break
| 15:45 	| Dependencies, data
| 16:45 	| Wrap-up
| 17:00 	| END

## Troubleshooting: questions raised

### Breakout room 1:


### Breakout room 2:


### Breakout room 3:
- questions about integration v unit tests
- questions about tests vs checks inside functions

<!-- ### Breakout room 4: -->



## 🔧 Exercises

### Exercise: what is in the readme?
- Description of package-
- Installation instructions
- Examples
- How to use functions
-  Authors, (indirect) contact information
- The aim of the package
-  citation
- License
- Very clear and broad instructions+examples +1

### Exercise: documentation
Take 5 minutes to edit the skeleton with real documentation. In particular:
1. Add a title.
1. Describe, below the title, what the function does.
1. Describe the parameter names.
1. Describe the output that is returned.
1. Keep @export.
1. Delete @examples.

Please share your documentation below! You can use tags ```like this``` to show code:



#### Exercise: Add dependencies to the description file, using `usethis::use_package()`.
For practice only:
- Place one package in Imports, and one in Suggests.
- Specify a minimum version.
- Notice that you can delete them again!


## 🧠 Collaborative Notes

### Documentation

Documentation makes your package useful and understandable to the users, and helps retain details that you may forget after a long time. There are several ways to do this.

#### Inline comments

Inline comments help you to add information (logics, metas, etc.) to your code directly. Examples of inline comments can be found in the code below (text started with *#* and colored in light gray).

```r=
make_groups <- function(names) {
    # Shuffle the names...
    shuffled_names <- sample(names)

    #... and reorder them as a matrix
    shuffled_matrix <- matrix(shuffled_names, ncol=2)

    return(shuffled_matrix)
}
```

#### README file

An example of README file: https://github.com/PabRod/kinematics.

Information there can be, e.g., installation manual, the auther contact information, and so on. In general, What you think is important and what you want to emphasize is recommended to be added to the README file.

A quick way to make a README file is to run the following code in the console. This will create the file with a predefined template, which will save you some time.

```r=
usethis::use_readme_md()
```

Some suggestions for writing README files:

- At the very beginning stage, you can leave the installation section empty and fill that out after you have something installable.
- It's not highly suggested to include dependency information. We have a better place to keep that information, which is the DESCRIPTION file.
- It's recommended to write README files in Markdown, which is a syntax language widely used in this situation. Most codebase platforms like GitHub and GitLab can render Markdown files automatically.


#### Roxygen

You may know that the help information of a function, say `approx`, can be shown by running the following command in the console:

```r=
?approx
```

The information then appears in the "Help" tag on the bottom-right of rstudio interface.

To add help information to your own functions, you will need roxygen, an R package, for documenting functions. To install and load roxygen, run the following command in the console:

```r=
install.packages("roxygen2")
library(roxygen2)
```

Before starting using roxygen for documentation, remember to first delete the NAMESPACE file of your package, then go to the Build tag, click "More -> Configure Build Tools...", a dialog like below will pop up:

![](https://codimd.carpentries.org/uploads/upload_a6afd01dab031d17db365c9835134757.png)

Check "Generate documentation with roxygen".

Now, to add the help infomration to your code file, from the menu click "Code -> Insert Roxygen Skeleton", that will create you the help section in your code automatically like this:

![](https://codimd.carpentries.org/uploads/upload_af7c12f0d14f5768cda494583d0d4ab8.png)

From there, you can fill in the information of your functions like title, description, parameter metas, exports and returns (a series of keywords available). To make the help information ready, install your package again, you may find the NAMESPACE file coming back again. Now you can access the help information of your function by using the "?" operator in console.


### Dependencies

Dependencies of a package are packages you need to run this package. To make sure that users will install the required dependencies, e.g. package `kniter`, you can declare that in the DESCRIPTION file by running:

```r=
usethis::use_package("knitr")
```

This will add `knitr` to the "Import" section of the DESCRIPTION file. In case there are some dependencies that you prefer the users to install, but they are not strictly required for using your package, add those dependencies to the "Suggests" section. To do so from the console, run the command below:

```r=
usethis::use_package("desired_package", type="suggests")
```

You can also declare requirements on version of packages to be installed. To check version of a package, say `knitr`, run:

```r=
packageVersion("knitr")
```

And to let the users install it with the minimum version of 1.40, run:

```r=
usethis::use_package("knitr", min_version = "1.40")
```

In addition, to force to install the latest version of all the dependencies, run the command below and it should update the DESCRIPTION file as well:

```r=
usethis::use_latest_dependencies(overwrite = T)
```

Keep in mind that an R package becomes more and more difficult to install with an increase number of dependencies, so try to keep depenencies minimum. To check if there is any unclear dependencies of your package, click the "Check" button in the "Build" tag (top-right of the interface).


### Data

Data can be part of your package. To make data directly availabe to the users, run:

```r=
usethis::use_data(your_data_object)
```

and the data can be accessed in this way:

```r=
your_package::your_data_object
```

To add help information to your data, create a separate R file (named "data.R" in this example) by running the command below:

```r=
usethis::use_r(data)
```

Add the help information there (see the screenshot below as an example):

![](https://codimd.carpentries.org/uploads/upload_a5a848a9d71b8ac4d09cd2737c14571a.png)

Note that the data name in the documentation file should match what you have in NAMESPACE. Then reinstall your package, and the data documentation should be ready.

A flow chart that shows different paths (options) of adding data to your package:

![](https://codimd.carpentries.org/uploads/upload_5d2cf36433f22912fdd4091599cf7e1a.png)

To add data to the `data_raw/` folder, run

```r=
usethis::use_data_raw("your_data")
```

This will create an R file with the same name, in which you write whatever code to prepare your data.


## 🧑🏽‍💻 Homework

As you perhaps already expect, the homework for this week is… write documentation for your package! Some additional tips and points of attention:

- Make sure you are clear on what functions are for the user — label them with `@export` — and what functions are helper functions.
- For the exported functions: insert a Roxygen skeleton (Code > Insert Roxygen Skeleton), and fill it out. Do explore the Roxygen documentation (https://roxygen2.r-lib.org/) and experiment with different tags.
- After documenting the package (e.g. with `devtools::document()` or `roxygen2::roxygenise()`), explore what the help pages for your functions look like, by pulling them up as you normally would with `?function_name`. Adjust as necessary.

Aside from the Roxygen documentation, do write a README.md file with some basic information about your package! This will be the first thing potential users see, so what do you want them to know?

Finally, make sure the dependencies of your package are clearly stated in the DESCRIPTION file. Note also that running a Check will only check your code for functions that are labeled with `packagename::function_name()`, and it will not detect any functions from external packages if they are not labeled correctly. Go through your code to be extra sure that functions are called correctly and you are not missing dependencies!

Remember that next week we will share our respective packages. Your package will not be complete and that is completely expected and perfectly OK. However, it would be nice if your package has at least one function that works and is well documented!


## 📚 Resources
[CRAN](https://cran.r-project.org/)
[Roxygen2 documentation](https://cran.r-project.org/web/packages/roxygen2/vignettes/roxygen2.html)
[Read more about R/sysdata.Rda](https://r-pkgs.org/Data.html#sec-data-sysdata)
[Read more about data-raw/](https://r-pkgs.org/Data.html#sec-data-data-raw)
[Chapter Data in the R Packages book](https://r-pkgs.org/data.html)

-
-
-