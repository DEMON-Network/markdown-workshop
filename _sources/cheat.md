# Markdown cheat sheet

What follows is a non-comprehensive guide to Markdown syntax. The `code` blocks show the Markdown syntax, and underneath is the output. Read through it and then you can proceed to the exercise at the end.  

## Headings

### Input
~~~
    ## Heading 2
    ### Heading 3
    #### Heading 4
~~~


### Output

## Heading 2
### Heading 3
#### Heading 4

## Lists

Note - actual numbers don't matter, just have a number and a period `.` The sub-items have been indented by a tab space. 

### Input

~~~
    1. Item 1
    1. Item 2
      1. sub Item 1
      3. sub Item 2
    2. Item 3
~~~


### Output

1. Item 1
1. Item 2
    1. sub Item 1
    3. sub Item 2
2. Item 3

## Bullets

### Input
~~~
    * Item 1
    * Item 2
        * sub Item 1
        * sub Item 2
    * Item 3
~~~

### Output

* Item 1
* Item 2
    * sub Item 1
    * sub Item 2
* Item 3

## Links
### Input

~~~
[link text](www.google.com)
~~~~

### Output

[link text](https://www.google.com)

## Images 

### Input
~~~~
![tidy data caption](./figs/tidy_data.jpg)
~~~~

### Output

![tidy data caption](./figs/tidy_data.jpg)


### Code chunks

#### Input

You can also include code in code  and the output of code in 'code chunks'.

````markdown
`r ''````{r}
x <- rnorm(2)

for(i in 1:length(x)){
  print(x[[i]])
}
```
````

You run the code by clicking the little triangle icon in the top right.


#### Output
This prints the code and the result:
```{r}
x <- rnorm(2)

for(i in 1:length(x)){
  print(x[[i]])
}
```


### Plots

As you can tell from the example document we `knit`ed at the start you can also show plots:

#### Input
````markdown
```r ''`{r}
library(ggplot2)
x <- rbeta(1000,5,2)
y <- rbeta(1000,0.3, 0.3)
ggplot(data.frame(x,y), aes(x=x, y=y)) + geom_point()
}
```
````

#### Output

gives the following code and plot
~~~
library(ggplot2)
x <- rbeta(1000,5,2)
y <- rbeta(1000,0.3, 0.3)
ggplot(data.frame(x,y), aes(x=x, y=y)) + geom_point()
~~~
{: .language-r}
![beta correlation](../fig/beta_correlation.png)

### Inline code

You can also put code inline with text. 

#### Input
````markdown
```r ''`{r}
x <- rbeta(1000,5,2)
```
````

The mean of the observations is `` `r knitr::inline_expr("mean(x)")` ``

#### Output

```{r}
x <- rbeta(1000,5,2)
```


The mean of the observations is `r mean(x)`. 
