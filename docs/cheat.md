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

<!-- #region -->
## Code chunks

### Input
~~~
```python

def f(x):
    return x**2

```
~~~
### Output

```python

def f(x):
    return x**2

```
<!-- #endregion -->

## Tables 


### Input

~~~
| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |
~~~

### Output

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |


## Plots (R only)



### Input

~~~
```r ''`{r}
library(ggplot2)
x <- rbeta(1000,5,2)
y <- rbeta(1000,0.3, 0.3)
ggplot(data.frame(x,y), aes(x=x, y=y)) + geom_point()
}
```
~~~

### Output

```r
library(ggplot2)
x <- rbeta(1000,5,2)
y <- rbeta(1000,0.3, 0.3)
ggplot(data.frame(x,y), aes(x=x, y=y)) + geom_point()
```

![beta correlation](./figs/beta_correlation.png)

```python

```
