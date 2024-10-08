---
syncID: de50ed3500de4e8bafeb12547c761116
title: "Build & Work With Functions in R"
description: "This tutorial teaches the basics of creating a function in R."
dateCreated: 2015-10-23
authors: Leah A. Wasser - Adapted from Software Carpentry
contributors: Garrett M. Williams
estimatedTime: 30 minutes
packagesLibraries:
topics: data-analysis
languagesTool: R
dataProduct:
code1: https://raw.githubusercontent.com/NEONScience/NEON-Data-Skills/main/tutorials/R/R-skills/basic-R-skills/R-Basics-Of-Functions/R-Basics-Of-Functions.R
tutorialSeries: R-basics
urlTitle: work-with-functions-r
---

Sometimes we want to perform a calculation, or a set of calculations, multiple 
times in our code.  We could write out the equation over and over in our code -- 
OR -- we could chose to build a function that allows us to repeat several 
operations with a single command. This tutorial will focus on creating functions 
in R.

<div id="ds-objectives" markdown="1">

## Learning Objectives
After completing this tutorial, you will be able to: 

* Explain why we should divide programs into small, single-purpose functions.
* Use a function that takes parameters (input values).
* Return a value from a function.
* Set default values for function parameters.
* Write, or define, a function.
* Test and debug a function. (This section in construction).

## Things You’ll Need To Complete This Tutorial
You will need the most current version of R and, preferably, `RStudio` loaded
on your computer to complete this tutorial.

****

**Set Working Directory:** This lesson assumes that you have set your working 
directory to the location of the downloaded and unzipped data subsets. 

<a href="https://www.neonscience.org/set-working-directory-r" target="_blank"> An overview
of setting the working directory in R can be found here.</a>

**R Script & Challenge Code:** NEON data lessons often contain challenges that 
reinforce learned skills. If available, the code for challenge solutions is found 
in the downloadable R script of the entire lesson, available in the footer of 
each lesson page.


</div>

## Creating Functions

Sometimes we want to perform a calculation, or a set of calculations, multiple 
times in our code. For example, we might need to convert units from Celsius to 
Kelvin, across multiple datasets and save if for future use. 
 
We could write out the equation over and over in our code -- OR -- we could chose 
to build a function that allows us to repeat several operations with a single 
command. This tutorial will focus on creating functions in R.

## Getting Started
Let's start by defining a function `fahr_to_kelvin` that converts temperature 
values from Fahrenheit to Kelvin:


    fahr_to_kelvin <- function(temp) {
    	kelvin <- ((temp - 32) * (5/9)) + 273.15
    	kelvin
    }

Notice the syntax used to define this function:


    FunctionNameHere <- function(Input-variable-here){
    	what-to-do-here
    	what-to-return-here
    }

The definition begins with the name of your new function. Use a good descriptor 
of the function you are doing and make sure it isn't the same as a
a commonly used R function!

This is followed by the call to make it a `function` and a parenthesized list of 
parameter names. The parameters are the input values that the function will use 
to perform any calculations. In the case of `fahr_to_kelvin`, the input will be 
the temperature value that we wish to convert from fahrenheit to kelvin. You can 
have as many input parameters as you would like (but too many are poor style). 

The body, or implementation, is surrounded by curly braces `{ }`. Leaving the 
initial curly bracket at the end of the first line and the final one on its own 
line makes functions easier to read (for the human, the machine doesn't care). 
In many languages, the body of the function - the statements that are executed 
when it runs - must be indented, typically using 4 spaces. 

<div id="ds-dataTip" markdown="1">
<i class="fa fa-star"></i> **Data Tip:** While it is not mandatory in R to indent 
your code 4 spaces within a function, it is  strongly recommended as good 
practice!
</div>

When we call the function, the values we pass to it are assigned to those 
variables so that we can use them inside the function. 

The last line within the function is what R will evaluate as a returning value. 
Remember that the last line has to be a command that will print to the screen, 
and not an object definition, otherwise the function will return nothing - it 
will work, but will provide no output. In our example we print the value of 
the object `Kelvin`. 

Calling our own function is no different from calling any other built in R 
function that you are familiar with.  Let's try running our function.   


    # call function for F=32 degrees
    fahr_to_kelvin(32)

    ## [1] 273.15

    # We could use `paste()` to create a sentence with the answer
    paste('The boiling point of water (212 Fahrenheit) is', 
          fahr_to_kelvin(212),
          'degrees Kelvin.')

    ## [1] "The boiling point of water (212 Fahrenheit) is 373.15 degrees Kelvin."

We've successfully called the function that we defined, and we have access to 
the value that we returned. 

**Question**: What would happen if we instead wrote our function as:


    fahr_to_kelvin_test <- function(temp) {
    	kelvin <- ((temp - 32) * (5 / 9)) + 273.15
    }

Try it: 


    fahr_to_kelvin_test(32)

Nothing is returned!  This is because we didn't specify what the output was in 
the final line of the function.  

However, we can see that the function still worked by assigning the function to 
object "a" and calling "a".


    # assign to a
    a <- fahr_to_kelvin_test(32)
    
    # value of a
    a

    ## [1] 273.15

We can see that even though there was no output from the function, the function 
was still operational.

###Variable Scope

In R, variables assigned a value within a function **do not** retain their values
outside of the function.


    x <- 1:3
    x

    ## [1] 1 2 3

    # define a function to add 1 to the temporary variable 'input'
    plus_one <- function(input) {
      input <- input + 1
    }
    
    # run our function
    plus_one(x)
    
    # x has not actually changed outside of the function
    x

    ## [1] 1 2 3

To change a variable outside of a function you must assign the funciton's output 
to that variable.


    plus_one <- function(input) {
      output <- input + 1     # store results to output variable
      output                  # return output variable
    }
    
    # assign the results of our function to x
    x <- plus_one(x)
    x

    ## [1] 2 3 4

<div id="ds-challenge" markdown="1">
### Challenge: Writing Functions

Now that we've seen how to turn Fahrenheit into Kelvin, try your hand at 
converting Kelvin to Celsius. Remember, for the same temperature Kelvin is 273.15 
degrees less than Celsius. 
</div>



## Compound Functions

What about converting Fahrenheit to Celsius? We could write out the formula as a
new function or we can combine the two functions we have already created. It 
might seem a bit silly to do this just for converting from Fahrenheit to Celcius 
but think about the other applications where you will use functions! 


    # use two functions (F->K & K->C) to create a new one (F->C)
    fahr_to_celsius <- function(temp) {
    	temp_k <- fahr_to_kelvin(temp)
    	temp_c <- kelvin_to_celsius(temp_k)
    	temp_c
    }
    	
    paste('freezing point of water (32 Fahrenheit) in Celsius:', 
          fahr_to_celsius(32.0))

    ## [1] "freezing point of water (32 Fahrenheit) in Celsius: 0"

This is our first taste of how larger programs are built: we define basic 
operations, then combine them in ever-large chunks to get the effect we want. 
Real-life functions will usually be larger than the ones shown here—typically 
half a dozen to a few dozen lines—but they shouldn't ever be much longer than 
that, or the next person who reads it won't be able to understand what's going 
on. 
