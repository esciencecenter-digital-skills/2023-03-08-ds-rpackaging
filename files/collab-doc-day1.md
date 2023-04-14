![](https://i.imgur.com/iywjz8s.png)


# Week 1 - 2023-03-08 Reproducible Research with R Packages

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

Eva Viviani, Ji Qi, Malte Lüken, Thijs Vroegh


## 🗓️ Agenda
Day 1. Wed 8 March 2023
| Time | Topic |
|--:|:---|
| 13:00 	| Welcome and icebreaker
| 13:15 	| Introduction, Accessing packages
| 14:15 	| Coffee break
| 14:30 	| Getting started
| 15:30 	| Coffee break
| 15:45 	| Licenses
| 16:45 	| Wrap-up
| 17:00 	| END

## 🔧 Exercises

Exercise:
- Got to https://choosealicense.com/
- Choose a license for your package
- Change the license of your package to the chosen one
- Share the function you used to change the license in the chat

Selected answers:
- `usethis::use_mit_license()`
- `usethis::use_agpl3_license()`

Exercise:
- Add another function to your (own, custom) R package

## 🧠 Collaborative Notes

### Introduction, Accessing packages
#### Why \(R\) packaging

- Packaging makes it easier to share and organise code
- Why R packaging?
    - Most modern languages include packaging
    - R is a popular language
    - It is relatively easy to learn
    - Soft introduction to packaging due to R Studio's package development capabilities

- R Studio Tip: When hovering over a button in the interface, an explanation of the button and a **hot key** is displayed

#### Installing R packages from CRAN
- How to install a package:
    - Find name of package (e.g., via search engine, Google)
    - R Studio: Got to "Packages" tab in lower right pane and click "Install"; type name of package in blank form
    - R Command: Type `install.packages("<package_name>")`
- CRAN: Comprehensive R Archice Network - Main online Repository where packages are stored
    - Anyone can submit R packages to CRAN
    - Package submissions are reviewed
- Test for checking if package has been installed: Type `library(<package_name>)` without errors
- Common reasons why packages cannot be installed:
    - Incompatibility with your installed R version
    - A system library is missing (look at the error message to find out which library is missing)

#### Attaching packages
- When running `library()`, the package will be *attached* to the current R enviroment
- When a package is *attached*, its functions can be called directly, e.g.:
```
library(stringr) # Attach package

str_detect(...) # Call function from stringr package

```
- Detach a package with `detach()`
- Call a function from a package without attaching the package (note that https://hackmd.io/jKAJhgm_Q9-33m50lNicEQ?both#he package needs to be installed): `<package_name>::<function_name>`, e.g.:
```
stringr::str_count("Hello")
[1] 5
```
- The `<package_name>::<function_name>` is the preferred way for calling external functions in R packaging
- Why calling functions without attaching a package?
    - The package is large and you only need one function from it
    - Multiple packages contain functions with the same name and you want to call a function from a specific package

#### Install packages from other sources
- Packages can also be installed from [GitHub](https://github.com/)
- Example GitHub package: https://github.com/pabrod/kinematics
- Install package from GitHub via: `devtools::install_github("<GitHub_URL>")` (requires devtools), e.g.:

```
devtools::install_github("https://github.com/pabrod/kinematics")
```
- Why install packages from GitHub:
    - You need the latest "development" version of a package which is not yet on CRAN
    - You want to install a package that is work-in-progress
- Uninstall a package `remove.packages("<package_name>")`, e.g.:
```
remove.packages("kinematics")
```

- It is also possible to install packages from other code sharing platforms, such as GitLab: `devtools::install_gitlab("GitLab_URL")`

- Install a package locally:
    - Open the folder with the package as a project in R Studio
    - Go to "Build" tab in top right pane and click "Install"
    - Syntax (usually not necessary): `R CMD INSTALL --preclean --no-multiarch --with-keep.source kinematics`

### Getting Started
#### Creating a new package
- Create a new project: In R Studio select "File" -> "New project" -> "New directory" -> "R package"
    - Name the package "mysterycoffee" and select a directory to create the package in
    - If comfortable with git, select "Create a git repository"
    - Select "Create project"
- Creates a folder with the R package structure:
    - DESCRIPTION file: Contains general information and meta data for your package (e.g., title, version, author)
    - R folder: Contains R code in the form of functions
        - **Important**: Only define functions (and documentation, comments) here, no code outside functions!
- Test that package mysterycoffee can be installed by selecting "Install" in the "Build" tab (top right pane)
    - This installs and attaches the package after installation
    - Test that the `hello()` function of the package works by typing:
```
hello()
[1] "Hello, world!"
```

### Adding new functions to a package
- Add new functions to the package:
    - Create a new R script (make_groups.R file), and write

```
# Take a names vector and return it as a 2-column shuffled matrix
make_groups <- function(names) {
    shuffled <- matrix(sample(names), ncol=2)
    return(shuffled)
}
```

- **Important**: You need to reinstall the package after adding new code to your package to call the updated functions from the package!
    - There is a difference between functions that are in your local environment (when you run the code in the R script) and those in your package (after you reinstalled the package with the new function)

- Check that the new function works (after reinstalling the package):
```
make_groups(c("Malte", "Pablo", "Eva", "Thijs"))
```
- or:
```
mysterycoffee::make_groups(c("Malte", "Pablo", "Eva", "Thijs"))
```
- The second way is highly preferred in R packaging because we *explicitly* call functions from our dependencies (i.e., other packages or the current package we rely on)
- You can remove the "hello.R" file in the R folder since we don't need it anymore
- **Important**: Save your scripts that call your package functions in a different folder than the package folder!
    - An exception are vignettes (see later part of workshop)
    - This requires a clear separation of code that should go into the package from code that calls the package: Reusable code usually goes into the package
- Multiple functions can be defined in the same file
- One function can call another function from the same or another file in your package, e.g.:
```
make_groups <- function(names) {
    shuffled <- matrix(sample(names), ncol=2)
    return(shuffled)
}

group_my_friends <- function() {
    friends <- c("Malte", "Pablo", "Eva", "Thijs")
    return(make_groups(friends)) # Call the previously defined `make_groups` function
}

```

#### Package versioning
- The version field in the DESCRIPTION file is particularly important: It keeps track of the state of your code which is important for reproducibility
    - Version entries are often structure in a format called [*Semantic Versioning*](https://semver.org/): `MAJOR.MINOR.PATCH`
    - If the behavior of your functions changes (i.e., it breaks previous functionality), increase the major version (1.0.0 -> 2.00)
    - If you add a new functionality (no breaking changes), increase the minor version (1.0.0 -> 1.1.0)
    - If you fix a (small) problem, increase the patch (1.0.0 -> 1.0.1)

#### Licenses
- (Standardized) Statement that defines what others can and cannot do with your package (and its code)
- Open source licenses:
    - GPL licenses: Software can be reused if the new software is also licensed under GPL (*viral* license)
    - Permissive licenses: Software can be reused without any guarantee
    - Public domain (CC0) licenses
- Check out https://choosealicense.com/
- The copyright holder officially chooses the license (i.e., the employer), some employers have an open source policy for choosing licenses
- Choose a license for your package with `usethis` package: `usethis::use_apache_license()`
    - Automatically changes the license to Apache 2.0
    - Creates a file called LICENSE.md the root folder that contains the license text

CC licenses:
- Creative Commons licenses
- CC-BY: Reuse software with the condition of attributing creating
- Developed for creative products, not software
- Software, however, also includes functioning of the code
- CC-BY not recommended for software

Packages should always have a license. Choose the license early in the package development to adapt the development process to the license and avoid license conflicts. For example, example data might be incompatible with your license and cannot be included.

## 🧑🏽‍💻 Homework

### Homework after week 1
Create a package using your own R project, if you have one (see the last point below if you do not have one). Create functions in the R folder of that package.

Remember the following:

* When you are using external functions inside your package, add double colons `::` before calling that function
* Separate out different functions, think about main functions and helper functions
* If you start from a script, you can keep the leftovers (if there are any after separating out functions) in a script that calls your package's functions and runs your workflow.
* It is very good if **at least one of your functions gives an output** (so, it doesn't just "do" something, like print `Hello World`): it actually creates a data frame, or a vector, or an object of some other kind! We will see why next week.
* Don't forget through `Install (and Restart)` whenever you change anything.
* If you are creating your own package, don't forget to update the `DESCRIPTION` file.
* If you are not using your own project, take some time to improve the current `make_groups` function and perhaps add another function.

Don’t worry if you run into issues, this is expected. See if you can fix them, but don't be afraid to bring them next week, so we can solve them together.


## 📚 Resources
[CRAN](https://cran.r-project.org/)
[Licensing R](https://thinkr-open.github.io/licensing-r/)
[Visual representation of licenses](https://thinkr-open.github.io/licensing-r/intro.html#a-visual-representation-of-the-licenses)
[To update your R](https://www.r-project.org/)
[About semantic versioning](https://semver.org/)
[About licenses](https://choosealicense.com/)
[Rtools](https://cran.r-project.org/bin/windows/Rtools/)