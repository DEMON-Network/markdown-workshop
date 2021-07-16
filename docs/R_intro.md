
# Literate programming and RMarkdown

Let's start with the definition of literate programming (from [Wikipedia](https://en.wikipedia.org/wiki/Literate_programming)):

*Literate programming is a programming paradigm introduced by Donald Knuth in which a computer program is given an explanation of its logic in a natural language, such as English, interspersed with snippets of macros and traditional source code, from which compilable source code can be generated.*

RMarkdown is a method for performing literate programming and is a powerful way of presenting your analysis in a way that's easy to code and easy to read. The *"snippets of macros and traditional source code"* are your `R` analysis code. The *"explanation of the its logic in a natural language"* is your analysis report.  

This lesson will draw heavily from the following excellent resources:

> ## RMarkdown further reading
> These are all from the RStudio website:
> * [Get Started with RMarkdown](https://rmarkdown.rstudio.com/lesson-1.html) - An extensive tutorial
> * [Reference (pdf)](https://rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf) - A 5 page reference of commands and concepts. 
> * [Cheatsheet (pdf)](https://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf) - A 2 page summary of the most important elements. 


## RMarkdown - what is it?

**Markdown** is a simple plain-text language that can be turned into formatted text, e.g., webpages, forum posts (stackexchange, Github) etc. 

**RMarkdown** is a variant of `Markdown` which allows you to incorporate code  within the
final document. It can be turned into HTML, pdfs, word documents, books, theses etc. as well. 

The material for this practical session is written in `RMarkdown` and `Markdown`. 

## RMarkdown -  what's it good for?

Consider your normal data analysis workflow. It's probably something like this:

**1. Conventional workflow**
1. Do some data analysis in an `R` script. Use comments to help reader understand. 
2. Save plots, tables, etc. in separate files (`fig1.png`, `tab1.csv`, etc.). 
2. Write report in Word and insert figures, tables etc. 

*Updating/changing analysis*: Edit `R` scripts, Word document, insert figures, potentially change figure lables, `fig1.png`-> `fig2.png` etc..  

*Sharing analysis*: Share data, R scripts, plots, tables, Word/Latex files. Also include how all the parts relate to each other in a README. 

With `RMarkdown` your workflow will be more like this: 

**2. Literate programming workflow**
1. Do some data analysis in an `RMarkdown` file. All figures, tables are displayed in line - no need to save separate files.  
2. Write your report around the code in the `RMarkdown` file. The narrative of the analysis helps the reader understand the code.  
3. One-click to create formatted Word/HTML/pdf report, or, one-click to publish to the web (requires setup - not covered in this course).  

*Updating/changing analysis*: Edit the single `RMarkdown` file and click to create report. 

*Sharing analysis*: Share data and the single `RMarkdown` file or the whole project folder. 

Of course for larger projects, you may have more than one `RMarkdown` file and you may have separate analysis scripts as well. An additional *major* benefit over writing documents in Word is that you can track changes with version control. 

<!-- #region -->
## RMarkdown - quick start

Let's create a quick-n-dirty RMarkdown document

> ## Create an RMarkdown file
> 1. Select `File` > `New File` > `R Markdown...`
> 2. `Title`: `Example Report`
> 3. `Author`: [Your name]
> 4. `Default Output Format`: `HTML`
> 3. Click `OK` (not `Create Empty Document`).  


You should see in your code editing pane a `RMarkdown` file called `Untitled1` with the following at 
the top: 
~~~
   ---
   title: "Example Report"
   author: "Your Name"
   date: "25/11/2019"
   output: html_document
   ---
~~~


This is the 'front matter' - a collection of key/value pairs tells R how you want to process the output. The front matter is a powerful way of customising your document and you should consult the RStudio tutorial to find out more about this. The `RMarkdown` file needs to be saved and processed to create the final report, let's do this now. 

> ## Save and process the report
> 1. Select `File` > `Save`
> 2. `File Name:` `example_report`
> 3. Click the `Knit` button (you'll need to enable pop-ups in your Browser if you're using `RStudio cloud`)
> 4. `RStudio cloud` the output will be in a new tabl 
> 5. `RStudio local` the output will be in the `Viewer` pane. 
<!-- #endregion -->


