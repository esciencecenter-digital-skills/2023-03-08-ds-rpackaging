![](https://i.imgur.com/iywjz8s.png)


# Week 2 - 2023-03-08 Reproducible Research with R Packages

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

Barbara Vreede (she/her), Pablo Rodriguez-Sanchez (he/him)

## 🧑‍🙋 Helpers

Eva Viviani (she/her), Ji Qi (he/him), Malte Lüken (he/him), Thijs Vroegh (he/him)


## 🗓️ Agenda
Day 2. Wed 15 March 2023
| Time | Topic |
|--:|:---|
| 13:00 	| Welcome and icebreaker
| 13:15 	| Recap homework & lessons learned
| 14:15 	| Coffee break
| 14:30 	| Testing
| 15:30 	| Coffee break
| 15:45 	| Testing, test coverage
| 16:45 	| Wrap-up
| 17:00 	| END

## 🔧 Exercises


#### Exercise: what does this test do?
Open a newly created testfile (after running `usethis::use_test([function])`, you will find it in `/tests/testthat/`) and take a look at its contents. It should contain something like this:

```r=
test_that("multiplication works", {
  expect_equal(2 * 2, 4)
})
```

This is just an example test, that was created to make your life easier. You can use it as a template for writing your own tests.

Take 10 minutes to:
- Figure out what is happening inside this test. Can you understand it?
- Write another test, just below this one, that checks that addition also works. Tip: you can copy-paste and edit the current test.

#### Exercise: write your own test.
Now it is your turn to write a test. With a vector of 6 names as input, check that the output of `make_groups()` is a matrix with 3 rows and 2 columns.

Tip 1: tests can contain multiple assertions.
Tip 2: the functions `nrow` and `ncol` may come in handy.


#### Exercise: test an uneven number.
Write a new test that passes an uneven number of names to `make_groups`.
Run the test again. What happened?


#### Exercise: Write a test using three different `expect_` functions.
Write one or multiple tests, based on a function of your choice, in which you use three different assertion types. For instance, `expect_error`, or `expect_length`. See [the documentation](https://testthat.r-lib.org/reference/index.html) for a list of functions.


#### Exercise: Check and increase your test coverage
If you have not installed the `covr` package, do so now: `install.packages("covr")`.

1. Make a coverage report with `covr::report()`. What is your coverage percentage?
2. Look at the coverage of an individual function. Does the coverage percentage make sense to you?
3. Write a test for a part of the function that you think is not yet covered. Re-run `covr::report()`. Did you increase your coverage?


## 🧠 Collaborative Notes
### Questions we had in the breakout rooms
- How to make sure that our functions are as general as possible?
    - By refactoring. In general, functions have two main purposes: aiding code reusability and breaking down a task into smaller logical units. When your package grows in size, you'll notice similarities, and routines, and that is the time when you may want to 'refactor': that is, to make those routines as stand-alone functions, i.e., atomic. This will make sure that your functions are as general as possible. An example of this strategy is making functions to plot data. When you create a type of plot that is extremely complex, made of smaller components which are fine-tuned by yourself all the time, you may want to group those components in functions, to give you more opportunity to focus on the flexible components of your plot.

- How do we name our functions? Are there any style conventions?
    - It's easier to think about functions as actions, and as per above, as **atomic** actions. As such, it is very straightforward to name them as verbs, and this is indeed a 'naming convention'. For instance, a function that reads data may be called "read_" + type of file, i.e., read_csv(). This strategy helps reusability and clarity. For an excellent example of this convention, take a look at the `dplyr` package, and the way it names its own functions (e.g., group, group_by, filter, select, etc).

- How to make sure that our end-users understand what our functions are doing?
    - There will be an episode on `Documentation` in the next sessions in which we are going to talk about that in details. In general: Names help the final users. Using vignettes also. You can also leave comments in your function.

- Should we store Data in packages? And if so, how?
    - It depends. If your data is big, you don't want to have it in your package, but packages can contain data, and we are going to cover this topic in the next sessions.

- Should we use pipes (%>%/ |>) and if so, when?
    - Using %>% in packages is not recommended. It reduces clarity, but if that cannot be avoided, the native R's operator: |> is better.

- What's the difference between "helper" and "main" functions? How do you decide which one is which?
    - Helper functions, also known as 'library, non-public, exported or helper functions', their main purpose is code reusability, but they are not necessarily interesting from a user point of view. Main functions instead are meant to be used directly from the user, and may also contain helper functions.

- To create new functions, how would you go about it?
    - There are many ways to accomplish this. You can do it manually (e.g., simply add an ".R" type of file yourself), or you can use the `usethis::use_r()` function, from the `usethis` package. Whichever way you want to go, the important thing to remember is **where** you create/add those files. The nice part of `usethis::use_r()` is that it does that for you which in the long run it reduces the cognitive load, but it is always recommended to keep in mind where those files should be stored (in the `/R/` folder!), in case you need to debug it or double-check it. **TIP:** in case you were wondering, you can store as many functions as you want in an ".R" file, since they are all loaded together at the same time. The choice of doing so it is a matter of mental organisation, and make it easier to navigate your package.

## Today's lesson: All about Tests

Let's say that you have made a function. How do you make sure that it is doing what it is supposed to do? As a first instance, you can run it yourself, and then you inspect its output. Let's make a practical example, and run our `make_groups` function from the package `mysterycoffee` we have build in our previous lesson:

```r=
library(mysterycoffee)
make_groups(c("Anna", "Boris", "Chloe", "Dan"))
```

We can now inspect manually the output, and if our expectation matches the output we can consider the test completed. As a matter of fact this condition is met in this case, and we can consider ourselves satisfied about the fact that the function is doing what it is supposed to do.

This way of testing is definitely *one* strategy. However, in the long run, with many functions, and with more complex outputs it is not sustainable to run tests manually in this way. For this reason we are going to talk about now about how to write tests which allow us to check whether the contents of the functions and our expectations match in an automatic and programmatic way.

We are going to do that by using the `testthat` package. You can take a look at its functionalities in the [Testthat documentation](https://testthat.r-lib.org/reference/index.html) which contains a list of `expect_` functions.

### Write automated tests
We have a folder called `tests/testthat` which already contains `testthat.R` (do not edit this file). This is the result of this command:

```r=
usethis::use_testthat()
```

Once we have run that, we can start working in the `tests/testthat` folder by writing tests for our function `make_groups`. We can create automatically this file by running:


```r=
usethis::use_test("make_groups")
```

This will automatically add a `make_groups.R` file which contains some dummy code. To run the code, click on `Run Tests` (upper right button in the R studio IDE). This will prompt an output in the window in the upper-right part of the R studio IDE which informs us about whether our test has passed or not, and if not, which error has been encountered.

We have run that as an example, of course, as our test at the moment contains this dummy code:

```r=
test_that("multiplication works", {
    expect_equal(2 * 2, 4)
})
```

This of course does not test our `make_groups` function. Let's edit the code:

```r=
test_that("groups are made correctly", {
    group <- make_groups(c("Anna", "Boris", "Chloe", "Dan"))
})
```

Note that if we run the above code as it is, since there is no explicit mention of a test, it will be skipped. We need to add the testing part:

```r=
test_that("groups are made correctly", {
    group <- make_groups(c("Anna", "Boris", "Chloe", "Dan"))
    expect_true(ncol(group) == 2)
})
```

Now, if we run the code, we get an informative output. Note that you can add as many tests as you wish for that function by simply adding more "expectations". For instance, we can add `expect_false()`:

```r=
test_that("groups are made correctly", {
    group <- make_groups(c("Anna", "Boris", "Chloe", "Dan"))
    expect_true(ncol(group) == 2)
    expect_false(nrow(group) == 0)
})
```

This will prompt two 2 tests, as now we have two "expect_" tests. Question: Is this a useful test?

```r=
test_that("groups are made correctly", {
    group <- make_groups(c("Anna", "Boris", "Chloe", "Dan"))
    expect_equal(group[1], "Dan")
})
```

Not really, because we expect the input to change, as the functions should be general purpose.

Q: Suppose I expect that the function fails. How should I go about this?
A: The idea is to always compare your expectation with the output, whatever this is. You want to be careful though in doing that, and being explicit about what is your input and what is your output, and if that makes sense. As a general rule of thumb, more tests = better.

On the top-right window, there is a `Test` button. If we press it, it will run all the code we got in the `tests/testthat` folder, as such it will run our entire test suite. Finally, you can do it by using a short-cut on your keyboard: `Cmd/Ctrl + Shift + T`.

### Testing errors
Try to think not only at situations in which tests should pass, but also at situations in which they should not, and they should prompt an error, or a warning.

I want now to have an error for whenever I have an uneven number of people:

```r=
test_that("make_groups can not work with uneven numbers", {
    uneven <- c("Anna", "Boris", "Chloe", "Dan", "Elise")
    expect_error(make_groups(uneven), "Uneven")
})
```

To make this test make sense, we need to modify this function:

```r=
make_groups <- function(names) {
    if(length(names)%%2 > 0) {
        stop("Uneven number of people")
    }
    shuffled <- matrix(sample(names), ncol = 2)
    return(shuffled)
}
```

If we run now this test, our test will pass, even though there is an error, that's of course because our expectation was exactly that: to throw an error.

TIP: you may want to skip tests sometimes, for example if it doesn't work as expected, or it is not ready yet, you can do that by adding this code `skip()` before our test.

```r=
skip("does not work")
test_that("make_groups can not work with uneven numbers", {
    uneven <- c("Anna", "Boris", "Chloe", "Dan", "Elise")
    expect_error(make_groups(uneven), "Uneven")
})
```

Q: Would adding an `if/else` scenario in a function be considered a test? Should we isolate this component in a stand-alone function?
A: You can add these instances to guide your end-users in the function, but in the test you need to make an explicit test about this.

Q: Is there a way to silence the output of the tests?
A: Yes. There is a way to do that, you can find more info about that in the testthat documentation.

### Manage data for tests
We can only use `.Rda` objects for our tests. For instance, as per our previous example, we would like to have a `people` object:

```r=
people <- c("Anna", "Boris", "Chloe", "Dan", "Elise")
```

We save this object as `.Rda` object by running:

```r=
save(people, file = "tests/testthat/testdata.Rda")
```

Make sure that you save this `.Rda` file in the `tests/testthat/` folder. Once we've done that, we can load this file in the `test_that()` function by using the `load()` function, and add another `expect_true` statement `expect_true("Dan" %in% people)`:

```r=
test_that("groups are made correctly", {
    # load our Rda file
    load(file = "testdata.Rda")
    group <- make_groups(people)
    # additional expect statement
    expect_true("Dan" %in% people)
    expect_true(ncol(group) == 2)
    expect_false(nrow(group) == 0)
})
```

### Code coverage
Code coverage tells you how much of your code is "covered" by your tests.

Let's install the `covr` package by running:

```r=
install.packages("covr")
```

Once the installation is completed, we can call this function `report()`:

```r=
covr::report()
```

Once we run that, it prompts us a table in the `Viewer` window of the R studio IDE which tells us what is the percentage of code that is covered by our tests. At the moment, my code coverage is quite low -- around 40%. Let's try to increase that (you can find an exercise about this in the Exercise section of this document).

We add another test:

```r=
test_that("my friends are all included", {
    friends <- group_my_friends()
    expect_true("Thijs" %in% friends)
})
```

If I run again the code `covr::report()` this will increase my code coverage.

### Unit testing and integration tests

Unit Testing is a type of software testing where individual units or components of a software are tested. Integration testing instead aims at testing the whole modules together, i.e., as a group.


## 🧑🏽‍💻 Homework
In this coming week, we would like you to apply the things you learned today to your own package. Start with the following steps:
1. Setup test infrastructure for your package with `usethis::use_testthat()`
2. Create a test using `usethis::use_test("test-context")`

Then, please write tests for one or more of your functions. You can do this as you see fit. For example:
- Write multiple tests for a single function, to test that the function deals well with multiple kinds of input. Remember you can put multiple assertions in a single test (an assertion is stating an expectation, e.g. `expect_equal`, `expect_true`).
- Check if your function produces specific error messages (using e.g. `expect_error`).
- Write a test for (part of) your workflow, in which multiple functions are used (remember that this is called *integration testing*).
- Use `covr::report()` to identify parts of your package that could benefit from more tests.

As you do this, here are some useful links:
- The collaborative document for today: https://tinyurl.com/Rbackup2
- Testthat documentation (including a description of all expect_ functions): https://testthat.r-lib.org/reference/index.html

Finally, and most importantly, do continue to develop the functions in your package. You can do this together with newly developed tests: your tests can help you confirm that functions still work as expected while you edit (refactor) them.


## 📚 Resources
[CRAN](https://cran.r-project.org/)
[Testthat documentation](https://testthat.r-lib.org/reference/index.html) which contains a list of `expect_` functions.
[Pipes](https://r4ds.hadley.nz/data-transform.html#sec-the-pipe)
[Difference between expect_is and expect_type](https://stackoverflow.com/questions/48658381/what-is-the-difference-between-expect-is-and-expect-type-of-the-testthat-package)

