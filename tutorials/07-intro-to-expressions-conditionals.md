Basics of R expressions and conditionals
================
Gaston Sanchez

> ### Learning Objectives:
>
> -   Understand the concept of R Expressions
> -   Learn the difference between simple and compound expressions
> -   Understand the use of braces to group expressions
> -   Conditional structures: if-then-else, and `switch()`

------------------------------------------------------------------------

R Expressions
-------------

Before talking about conditional structures and loops, we must first talk about **expressions**.

R programs are made up of expressions. These can be either *simple* expressions or *compound* expressions. Compound expressions consist of simple expressions separated by semicolons or newlines, and grouped within braces.

``` r
# structure of a compound expression
# with simple expressions separated by semicolons
{expression_1; expression_2; ...; expression_n}

# structure of a compound expression
# with simple expressions separated by newlines
{
  expression_1
  expression_2
  expression_n
}
```

Here's a less abstract example:

``` r
# simple expressions separated by semicolons
{"first"; 1; 2; 3; "last"}
```

    ## [1] "last"

``` r
# simple expressions separated by newlines
{
  "first"
  1
  2
  3
  "last"
}
```

    ## [1] "last"

Writing compound expressions like those in the previous example is not something common among R users. Although the expressions are perfectly valid, these examples are very dummy (just for illustration purposes).

I discourage you from grouping multiple expressions with semicolons because it makes it difficult to inspect things. As for the expressions separated by newlines, they do play an important role but they are typically used together with other programming structures (e.g. functions, conditionals, loops).

### Every expression has a value

A fundamental notion about expressions is that **every expression in R has a value**. If you have a simple expression like:

``` r
a <- 5
```

then `a` has the value 5. In contrast, the value of a compound expression is the value of the last evaluated expression.

Here's one example of a compund expression. Note that the entire expression is assigned to `x`:

``` r
x <- {5; 10}
```

-   What is the value of `x`?
    -   5?
    -   10?
    -   5, 10?

In this case, the compound expression has a value of `10`, which was the last expression inside the braces.

Here's the same compound expression as above, but now with simple expressions in newlines:

``` r
x <- {
  5
  10
}

x
```

    ## [1] 10

To make sure you don't forget it, repeat this mantra:

> -   Every expression in R has a value: the value of the last evaluated statement.
> -   Every expression in R has a value: the value of the last evaluated statement.
> -   Every expression in R has a value: the value of the last evaluated statement.

------------------------------------------------------------------------

### Assignments within Compound Expressions

It is possible to have assignments within compound expressions and the values of the variables which this produces can be used in later expressions.

``` r
# simple expressions (made up of assignments) separated by newlines
{
  one <- 1
  pie <- pi
  zee <- "z"
}

one
```

    ## [1] 1

``` r
pie
```

    ## [1] 3.141593

``` r
zee
```

    ## [1] "z"

Here's another example:

``` r
z <- { x = 10 ; y = x^2; x + y }

x
```

    ## [1] 10

``` r
y
```

    ## [1] 100

``` r
z
```

    ## [1] 110

Now that we've introduced the concept of R expressions, let's introduce conditional structures

------------------------------------------------------------------------

Conditionals
------------

Every programming language comes with a set of structures that allows us to have control over how commands are executed. One of these structures is called **conditionals**, and as its name indicates, they are used to evaluate conditions.

The most common conditional structure, conceptually speaking, is the **if-then-else** statement. This type of statement makes it possible to choose between two (possibly compound) expressions depending on the value of a (logical) condition.

In R (as in many other languages) the if-then-else statement has the following structure:

``` r
if (condition) {
  # do something
} else {
  # do something else
}
```

As you can tell, the `if` clause works like a function: `if(condition)`. Likewise, braces are used to group one or more expressions. If the condition to be evaluated is true, then just the expressions inside the first pair of braces are executed. If the condition is false, then the expressions inside the second pair of braces are executed:

``` r
x <- 1

if (x > 0) {
  print("positive")
} else {
  print("not positive")
}
```

    ## [1] "positive"

For readability reasons, most users prefer to write `if (condition)` instead of `if(condition)`. The *condition* is an expression that when evaluated returns a **logical** value of length one. In other words, whatever you pass as the input of the `if` clause, it has to be something that becomes `TRUE` or `FALSE`

``` r
if (TRUE) {
  print("TRUE")
} else {
  print("FALSE")
}
```

    ## [1] "TRUE"

``` r
if (FALSE) {
  print("TRUE")
} else {
  print("FALSE")
}
```

    ## [1] "FALSE"

#### Minimalist If-then-else

`if` statements can be written in different forms, depending on the types of expressions that are evaluated. If the expressions of both the *True* part and the *False* part are simple expressions, the if-then-else can be simplified as:

``` r
if (condition) expression_1 else expression_2
```

With simple expressions there's actually no need to use braces:

``` r
x <- 10
if (x > 0) y <- sqrt(x) else y <- -sqrt(-x)

y
```

    ## [1] 3.162278

The previous statement can be written more succinctly in R as:

``` r
x <- 10
y <- if (x > 0) sqrt(x) else -sqrt(-x)

y
```

    ## [1] 3.162278

Again, even though the previous commands are perfectly OK, it is preferred to use braces when working with conditional structures. This is a good practice that improves readibility:

``` r
# embrace braces: use them as much as possible!
x <- 10
if (x > 0) {
  y <- sqrt(x) 
} else {
  y <- -sqrt(-x)
}

y
```

    ## [1] 3.162278

### Simple If's

There is a simplified form of if-then-else statement which is available when there is no expression in the False part to evaluate. This statement has the general form:

``` r
if (condition) expression
```

and is equivalent to:

``` r
if (condition) expression else NULL
```

Here's an example:

``` r
x <- 4
y <- 2
if (x > y) print("x is greater than y")
```

    ## [1] "x is greater than y"

### Multiple If's

A common situation involves working with multiple conditions at the same time. You can chain multiple if-else statements:

``` r
y <- 1 # Change this value!

if (y > 0) {
  print("positive")
} else if (y < 0) {
  print("negative")
} else {
  print("zero?")
}
```

    ## [1] "positive"

#### Your turn!

Write R code that will "squish" a number into the interval \[0, 100\], so that a number less than 0 is replaced by 0 and a number greater than 100 is replaced by 100.

``` r
z <- 100 * pi

# Fill in the following if-else statements. You may (or may not) 
# have to add or subtract else if or else statements.
if (TRUE) { # Replace TRUE with a condition.
  
} else if (TRUE) { # Replace TRUE with a condition.
  
} else {
  
}
```

    ## NULL

### Switch

Working with multiple chained if's becomes cumbersome. Consider the following example that uses several if's to convert a day of the week into a number:

``` r
# Convert the day of the week into a number.
day <- "Tuesday" # Change this value!

if (day == 'Sunday') {
  num_day <- 1
} else {
  if (day == "Monday") {
    num_day <- 2
  } else {
    if (day == "Tuesday") {
      num_day <- 3
    } else {
      if (day == "Wednesday") {
        num_day <- 4
      } else {
        if (day == "Thursday") {
          num_day <- 5
        } else {
          if (day == "Friday") {
            num_day <- 6
          } else {
            if (day == "Saturday") {
              num_day <- 7
            }
          }
        }
      }
    }
  }
}


num_day
```

    ## [1] 3

Working with several nested if's like in the example above can be a nightmare.

In R, you can get rid of many of the braces like this:

``` r
# Convert the day of the week into a number.
day <- "Tuesday" # Change this value!

if (day == 'Sunday') {
  num_day <- 1
} else if (day == "Monday") {
  num_day <- 2
} else if (day == "Tuesday") {
  num_day <- 3
} else if (day == "Wednesday") {
  num_day <- 4
} else if (day == "Thursday") {
  num_day <- 5
} else if (day == "Friday") {
  num_day <- 6
} else if (day == "Saturday") {
  num_day <- 7
}

num_day
```

    ## [1] 3

But still we have too many if's, and there's a lot of repetition in the code. If you find yourself using many if-else statements with identical structure for slightly different cases, you may want to consider a **switch** statement instead:

``` r
# Convert the day of the week into a number.
day <- "Tuesday" # Change this value!

switch(day, # The expression to be evaluated.
  Sunday = 1,
  Monday = 2,
  Tuesday = 3,
  Wednesday = 4,
  Thursday = 5,
  Friday = 6,
  Saturday = 7,
  NA) # an (optional) default value if there are no matches
```

    ## [1] 3

Switch statements can also accept integer arguments, which will act as indices to choose a corresponding element:

``` r
# Convert a number into a day of the week.
day_num <- 3 # Change this value!

switch(day_num,
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday")
```

    ## [1] "Tuesday"

### Function `stop()`

Consider the formula for the area *A* of a circle of given radius *r*:

*A* = *π**r*<sup>2</sup>

We can write a function `circle_area()` that calculates the area of a circle. This function takes one argument `radius`, and we can give `radius` a default value of 1:

``` r
circle_area <- function(radius = 1) {
  pi * radius^2
}
```

For example:

``` r
# default (radius 1)
circle_area()
```

    ## [1] 3.141593

``` r
# radius 3
circle_area(radius = 3)
```

    ## [1] 28.27433

What happens if you give radius a negative value?

``` r
# default (radius 1)
circle_area(-1)
```

    ## [1] 3.141593

The function `circle_area()` still returns a value even when the radius is negative, which does not make sense. In many cases, it is desirable to make some checkings on the provided input. If the input is not appropriate, we should stop the execution of the function. For this purpose, we can use the function `stop()`

``` r
circle_area <- function(radius = 1) {
  if (radius < 0) {
    stop("radius must be positive")
  }
  pi * radius^2
}

# this should work
circle_area(1)

# this should not work
circle_area(-1)
```
