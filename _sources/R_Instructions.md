
# Pre-workshop instructions for R users

We encourage participants to use `RStudio cloud` during this workshop. The features of `RStudio cloud` map to the features of the desktop application `RStudio local` which you may be familiar with, with a few minor differences.  


To help you get to grips with the main features, a useful `RStudio` cheat-sheet can be found [here](https://rstudio.com/wp-content/uploads/2016/01/rstudio-IDE-cheatsheet.pdf), and to learn about the many `RStudio cloud` features you should go to the online documentation [here](https://rstudio.cloud/learn). 

## Creating an RStudio Cloud account

Please sign up to [RStudio Cloud](https://rstudio.cloud) before the workshop and familiarise yourself with it using the tips below.

If you would prefer to use your local copy of RStudio, you will need to install some software, and make sure that they are connected to one another. This may require you to get permission from your IT services department, so it is particularly important that you do this ahead of time. 

**If you cannot make any part of these instructions work before the session, then please sign up to [RStudio Cloud](https://rstudio.cloud) using your GitHub account instead.**

## Introduction to RStudio Cloud 

### Analysis is organised in Projects

`Projects` keep track of settings tied to a particular data analysis project. When starting a new project you will create a new `RStudio` `Project`. If you want to continue working on something,  just open up the relevant `RStudio` `Project`. 

In order open an existing project:

1. `RStudio cloud`: your projects will be listed under `Your Workspace`

2. `RStudio local`:  click on the `[project_name].Rproj` file in a file browser (your system file browser or the `RStudio` file browser); OR through RStudio menus: `File` > `Open Project...`/`Recent Projects`.

### Setting up RStudio projects

Complete either one of the exercises below, depending on whether you're using `RStudio local/cloud`. 

> ### Create a new `RStudio cloud` project
> 1. Navigate to [rstudio.cloud](https://rstudio.cloud)
> 2. Navigate to the `Your Workspace` tab. 
> 3. Click `New Project`
> 4. Rename `Untitled Project` to `md-workshop`. 

> ### Create a new `RStudio local` project
> 1. Open RStudio
> 2. Select `File` > `New Project` > `New Directory` > `New Project` 
> 3. `Directory name`: "md-workshop"
> 4. `Create project as...`: Select a convenient directory
> 5. Select `Create Project`

You'll also need to install the required packages so please run the following: 

```
list.of.packages <- c("patchwork", "ggpubr", "tidyverse", "data.table", "knitr", "markdown",
 "rmarkdown", "dplyr", "ggplot2", "moments", "car")

new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)
```

### Specialised panes have useful features

Each pane of `RStudio` has a special feature which helps you write code and manage your project. Let's go through them now: 

1. `Console` - Runs `R` commands. 
2. `Files` - File navigator. 
3. `Terminal` - is the same as `Console` but for the `BASH/Git BASH` shell-language (or whatever shell language is available to you, e.g., `Powershell` on Windows).  This is useful for managing files and for doing complicated things with version control (not covered in this course). If you have `BASH/Git-BASH` try typing `pwd` to see what it does. 
4. `Packages` - shows a list of all the packages you have installed. 
5. `Help` -  is the help files for all installed packages. 
6. `Environment` - as already mentioned, contains all your saved  variables/functions/dataframes etc. 
7. `History` - a list of previously run commands. 

#### Environment

`Environment` shows all the environments that are available to you. This includes all the loaded libraries. 

#### History

The history pane shows you all the previous commands that have been run. 

This feature of `History` is particularly useful - you can use it to iteratively build up your `R` script by: first trying things out in the console and second, easily transfer them to your script when you're happy with what they do. The `History` pane is searchable as well so you can add commands from previous sessions. 

### RStudio has tools for easy code writing and running

Other great features are:

1. tab complete, 

2. multiline editing,

3. keyboard shortcuts. 

Note: the keyboard shortcuts won't work in `RStudio Cloud` as they clash with your browser shortcuts.


## Further reading
Mastering all the features of an IDE is time consuming but well worth it. 
Check out RStudio tutorials by clicking [here](https://support.rstudio.com/hc/en-us/sections/200107586-Using-the-RStudio-IDE)

