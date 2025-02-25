


# Making reproducible examples

*Written by Marija Pejcinovska and last updated on 7 October 2021.*

## Introduction

In this lesson you will:

- learn about using reproducible examples to solve R issues
- learn how to create reproducible examples 
- get introduced to packages that ease the pain of making reproducible examples

Prerequisite skills include:

- Installing packages, troubleshooting, and asking for help

Highlights:

- Making good reproducible examples is essential for getting online help with your R issues (be it on Stack Overflow, R Studio Communities, Slack,..).
- Good reproducible examples are minimal and self-contained.
- R packages like `reprex` are there to help you ask for help the right way!

## Seeking help the helpful way

At this point in the toolkit you have probably picked up a troubleshooting trick or two. 

In fact, you may have already googled how to code up something in R or have come across useful discussions on Stack Exchange or Stack Overflow. 

Most of the time, you are likely to face issues many R users have grappled with before. In a way, this is nice. It means it should be much easier to find a solution to your problem online, usually with only a few google searches. Other times though, you might come across a more challenging problem and the solution might not be as readily available online. 
In such a situation, assuming you've exhausted all other debugging and troubleshooting options, it's possible that you would need to ask for some help online! Fortunately, as you've been making your way through this toolkit, you have probably already learned where to turn for help when needed.

At this stage, knowing how to create a **repr**oducible **ex**ample (or **reprex** for short) would come in handy. The idea of a reprex is exactly what you might expect: it is a minimal illustrative example that will reproduce the issue you are facing, so that others could help you solve it more easily.

Not all reprexes are created equal, however. In order for you to engage productively with the R community and get the help you need,  you should help the reader understand your problem with as little effort on their part as possible. This means that a reprex needs to be minimal and self-contained, so that those helping you can simply copy and paste your R code, reproduce the error and understand your issue... and hopefully help you solve it, of course.

## Elements of a reprex

So, what does a reprex look like?  

All reproducible examples have these few elements in common:  

 - a **minimal data set** on which the code can be run and the error reproduced. 
 - a **minimal, runnable** piece of **code** that is **well formatted** and produces the same error when run on the supplied minimal data set.  
 - call to any necessary **R packages** + any relevant pieces of **information about your R environment** when the issue was encountered. 

## Creating a data set for a reprex

You should always provide some data along with your reproducible example, so that you can easily illustrate your issue. It might not always be possible to share the actual data you've been working on. In fact, it is probably more practical to create a "toy" data set which will produce the same error or issue you've been dealing with.  

There are a few of ways of doing this. 

 - You could **create your own data** by using functions such as those in the `tibble` package. As an example, consider the data frame below. Here we've used the `tibble()` function to create a very small data frame with 4 variables (x, y, z, and w) each with 4 observations (Note: `tibble()` in the tidyverse is similar to the `data.frame()` function in base R). An alternative way of creating a data frame is through the  `tribble()` function, where **tribble** is simply short for **tr**ansposed **tibble**, the syntax for which is visually easier but a little less common (call `?tribble` at the R prompt)
 

```r
first_toy_data <- tibble (
  x = 1:4,
  y = c("a", "b", "c", "d"),
  z = x + x ^ 2,
  w = c("yes", "no", "yes", "yes")
)
first_toy_data
#> # A tibble: 4 × 4
#>       x y         z w    
#>   <int> <chr> <dbl> <chr>
#> 1     1 a         2 yes  
#> 2     2 b         6 no   
#> 3     3 c        12 yes  
#> 4     4 d        20 yes
```
 
- Alternatively, you could consider using some of the **built-in R data sets**. To see the available data sets, try typing  `data()` at the prompt in your R console.  As you keep learning R you'll notice many examples that feature, say, the *mtcars* or *iris* data sets. These data sets do get a little boring over time, but you shouldn't really worry about the dullness of your data when making a reprex. In fact, feel free to bore your helpers with you choices. If you do end up using built in data, you might want to consider using only a subset of the data set (do this by making use of the `head()` or `sample()` functions.)

 - Finally, whatever data you are sharing you might find the `dput()` function helpful. The function will write a text representation of your data which others can then copy-and-paste into their own R scripts to get your data. `dput` essentially generates the R code necessary to recreate your data. The output from `dput` would look something like this:
 

```r
dput(first_toy_data)
#> structure(list(x = 1:4, y = c("a", "b", "c", "d"), z = c(2, 6, 
#> 12, 20), w = c("yes", "no", "yes", "yes")), row.names = c(NA, 
#> -4L), class = c("tbl_df", "tbl", "data.frame"))
```

Running the output from `dput()` will create exactly the data object `first_toy_data` we created above. This is, indeed, the behavior we want. Run the code below to see for yourself.
 

```r
structure(
  list(
    x = 1:4,
    y = c("a", "b", "c", "d"),
    z = c(2, 6,
          12, 20),
    w = c("yes", "no", "yes", "yes")
  ),
  row.names = c(NA,-4L),
  class = c("tbl_df", "tbl", "data.frame")
)
#> # A tibble: 4 × 4
#>       x y         z w    
#>   <int> <chr> <dbl> <chr>
#> 1     1 a         2 yes  
#> 2     2 b         6 no   
#> 3     3 c        12 yes  
#> 4     4 d        20 yes
```

## Adding code 

The code in your reprex should be easy to understand and stripped down to the most bare version that would allow those helping you to replicate your errors.

Here are a few tips you might want to keep in mind as you are readying code for a reprex: 

 - Make sure you only include necessary code!! This simply means including only enough code to reproduce your problem; and not everything that is in your R script.
 - Try to format the code properly. This will make it easier for people to read and understand it. If you are unsure of the recommended formatting styles check out the following tidyverse style guide by Hadley Whickham (https://style.tidyverse.org/).
 - Comment your code if necessary (recall that we use `#` to begin a comment in an R code chunk).  
 - Don't copy and paste code from the console!! Console output contains characters that would make it difficult for folks to re-run your code without doing additional work. This might be an even bigger problem if you post your copied console output to an online forum. The special characters in the console output might be interpreted as special formatting symbols which can render your post unreadable!  
 - If the code that created your data uses any random generation of values (e.g. `sample()`, `rnorm()`, `runif()` etc.) you need to use the `set.seed()` function. This will fix the starting number used in generating a sequence of random values, making your data easy to replicate exactly.  
 - Always test your code in a new, empty R session! This means that before you upload any code and ask for help on Stack Overflow or Slack or RStudio community forums you should make sure that the code runs outside of the R environment where it was created.

A useful tool for making reproducible example is the `reprex` package. We'll see reprex in action shortly. 

## Packages and any other relevant information

Along with the data and code, when asking for help you should always remember to add the relevant packages at the top of your script. If you use certain functions or package-specific data sets you need to specify the required package (by adding `library(package_name)` at the top of your code snippet), otherwise your code will not be exactly reproducible.   

In certain situations, it might be useful to add a bit more information. If you are reporting on an unusual error or believe to have come across a bug in some function or feature of a package, you might need to report the version of R you are using and possibly the operating system. In most cases this would be sufficient, but sometimes you may need to also share the output of `sessionInfo()`.  Note that while packages are absolutely essential, sharing your version of R or a specific package might not be always necessary. 

## A closer look at the reprex package

Making reproducible examples is not always easy. Fortunately, there is an R package which makes some of that work a little bit easier.  

Below is a quick overview of how you can create a reproducible example with `reprex`.

 - Start by installing the `reprex` package 
  (i) We can do this by  installing  from CRAN: 


```r
install.packages("reprex")
```

  (ii) or, by fetching the development version from GitHub


```r
devtools::install_github("tidyverse/reprex")
```
    
 - In an R script write your code, including the data and all necessary calls to packages. For instance, suppose the code in your script is just as the one below.


```r
library(tidyverse)
mpg %>%
  ggplot(aes(x = displ, y = hwy)) %>%
  geom_point(aes(color = class)) %>%
  geom_smooth()
```

 - Highlight the code (including the library statement) and copy it to your clipboard  
 - In your console type `reprex()` and press Enter (you'll need to wait a second or two for R to render your reproducible example).  
 - Once the reprex has been rendered, it is automatically stored on your clipboard and you could simply paste it online and share it with others.  
 - To see what R actually generates once reprex() has been called in the console we'll paste the content of the clipboard below. 
 
``` r
library(tidyverse)
mpg %>% 
  ggplot(aes(x=displ, y=hwy)) %>% 
  geom_point(aes(color=class)) %>% 
  geom_smooth()
#> Error: `mapping` must be created by `aes()`
#> Did you use %>% instead of +?
```

<sup>Created on 2021-01-18 by the [reprex package](https://reprex.tidyverse.org) (v0.3.0)</sup>

## Exercises

### Exercise 1

Suppose you are interested in seeking help from the online RStudio community for an error you get based on the code below.

```r
tibble(
  group = c(
    "trtmnt",
    "control",
    "control",
    "control",
    "trtmnt",
    "control",
    "trtmnt",
    "trtmnt",
    "trtmnt",
    "control"
  ),
  msrmnt = rnorm(10, 5, 0.5),
  ## rnorm(10,5,1.5) generates 10 random normal variables centered at 5 with sd of 1.5
  ## use ?rnorm to check out the function's arguments
  improvement = c("yes", "no", "no", "no", "no", "no", "yes", "no", "no", "yes")
) %>%
  mutate(new_var1 = msrmnt + improvement)
```

**PART A**  

<!-- ```{r ex1reprexp1, echo=FALSE} -->
<!--   question("As it is written, is the above snippet of code a viable reproducible example?", -->
<!--            answer("Yes"),  -->
<!--            answer("No", correct = TRUE) -->
<!--            ) -->


<!-- ``` -->

<!-- **PART B**   -->

<!-- How might you fix it? Add any missing pieces and turn the code chunk below into a usable reprex. -->

<!-- ```{r ex1reprexp2, exercise=TRUE, exercise.eval = FALSE} -->


<!-- ``` -->

**PART C**  

The error you should be getting should involve the `mutate()` function. Specifically the error message should be:


```
#> Error: Problem with `mutate()` input `new_var1`. 
#> x non-numeric argument to binary operator 
#> ℹ Input `new_var1` is `msrmnt + improvement`.
```

Use the `reprex()` function in the code chunk below to check whether you've correctly modified the code in Part B. You can do this by copying and pasting the correct code inside the curly brackets provided in the `reprex` function. See what happens.

<!-- ```{r ex1reprexp3, exercise=TRUE, exercise.lines=15} -->
<!-- reprex::reprex({ -->
<!--   set.seed(789) -->
<!--   library(tidyverse) -->
<!--   tibble( -->
<!--   group = c("trtmnt", "control", "control", "control", "trtmnt", "control", "trtmnt", "trtmnt", "trtmnt", "control") -->
<!-- ) } -->
<!-- ) -->
<!-- ``` -->

<!-- ```{r ex1reprexp3-solution} -->
<!-- reprex::reprex({ -->
<!--   set.seed(789) -->
<!--   library(tidyverse) -->
<!--   tibble( -->
<!--     group = c("trtmnt", "control", "control", "control", "trtmnt", "control", "trtmnt", "trtmnt", "trtmnt", "control"), -->
<!--     msrmnt = rnorm(10,5, 0.5), ## rnorm(10,5,1.5) generates 10 random normal variables centered at 5 with sd of 1.5 -->
<!--     ## use ?rnorm to check out the function's arguments -->
<!--     improvement = c("yes", "no", "no", "no", "no", "no", "yes", "no", "no", "yes") -->
<!--   ) %>%  -->
<!--     mutate(new_var1 = msrmnt + improvement,  -->
<!--            new_var2 = 2 * msrmnt) -->
<!--   } -->
<!-- ) -->
<!-- ``` -->

## Next steps

In this lesson you learned the basics of making a reproducible example. If you are interested in some additional resources, consider the following list of do's and don'ts from the folks that made the `reprex` package: https://reprex.tidyverse.org/articles/reprex-dos-and-donts.html  

For even more information on `reprex`, check out Jenny Bryan webinar on creating reproducible examples with reprex:  https://reprex.tidyverse.org/articles/articles/learn-reprex.html

## Exercises

### Question 1

### Question 2

### Question 3

### Question 4

### Question 5

### Question 6

### Question 7

### Question 8

### Question 9

### Question 10
