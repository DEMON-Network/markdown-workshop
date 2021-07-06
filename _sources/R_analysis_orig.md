---
title: "Example analysis for Reproducible Reporting using Markdown"
author: "Meta Miriam Bönniger"
date: "`r format(Sys.time(), '%d %B, %Y')`"
output:
  html_document:
    code_folding: 'hide'
---

# {.tabset}

```{r setup}
knitr::opts_chunk$set(echo = TRUE)
```

## Load/install packages
```{r load packages, message=FALSE}
list.of.packages <- c("patchwork", "ggpubr", "tidyverse", "data.table", "knitr", "markdown", "rmarkdown", "dplyr", "ggplot2", "moments", "car")

# install packages if new
new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)

# load packages
invisible(lapply(list.of.packages, library, character.only = TRUE))
```

## Download dataset for analysis

Download and save the dataset for the analysis of: <http://www.oasis-brains.org/pdf/oasis_longitudinal.csv> 

Dataset description and explanation: 

Open Access Series of Imaging Studies (OASIS): Longitudinal MRI Data in Nondemented and Demented Older Adults.
Marcus, DS, Fotenos, AF, Csernansky, JG, Morris, JC, Buckner, RL, 2010. Journal of Cognitive Neuroscience, 22, 2677-2684. doi: 10.1162/jocn.2009.21407  

```{r load data}
#set working directory
setwd("/Users/boennigerm/Desktop/DEMON_reproducible-science")

#load dataset
dt.raw <- read.csv("oasis_longitudinal.csv") 

#load data dictionary
dt.dict <- read.csv("Data-dictionary.csv") 
```

## Changing variable type

1. We change the values of CDR from numeric values to ordered factors as suggested by the data dictionary.

* 0 = no dementia
* 0.5 = very mild AD
* 1 = mild AD
* 2 = moderate AD

```{r CDR change}
dt <- dt.raw %>%
  mutate(CDR = 
           factor(ifelse(CDR == 0, 
                         "no dementia", 
                         ifelse(CDR == 0.5, 
                                "very mild AD", 
                                ifelse(CDR == 1,
                                       "mild AD",
                                       ifelse(CDR == 2,
                                              "moderate AD",
                                              NA)))), 
                  levels=c("no dementia", "very mild AD", "mild AD", "moderate AD"), 
                  ordered=TRUE))
```

2. We change "Visit" from numeric values to ordered factors (1-5).

```{r Visit change}
dt <- dt %>%
  mutate(Visit = factor(Visit, 
                        levels=c(1,2,3,4,5),
                        ordered=TRUE))
```

3. We change "SES" from numeric values to ordered factors (1 = highest to 5 = lowest status).

```{r SES change}
dt <- dt %>%
  mutate(SES = factor(SES, 
                      levels=c(1,2,3,4,5),
                      ordered=TRUE))
```

## Getting an overview about the dataset

### Non-numerical variables

```{r overview non numerical}
numeric.var <- dt[, sapply(dt, is.numeric)] %>% 
  names()

kable(head(dt %>% 
             dplyr::select(-all_of(numeric.var))) %>% 
        data.frame(),
      caption = "Head of all non-numeric variables in the dataset.")

kable(table(dt$Group, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(Group = Var1), 
      caption = "Factor levels and frequency of variable 'Group'")

kable(table(dt$Visit, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(Visit = Var1), 
      caption = "Factor levels and frequency of variable 'Visit'")

kable(table(dt$M.F, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(M.F = Var1), 
      caption = "Factor levels and frequency of variable 'M.F'")

kable(table(dt$Hand, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(Hand = Var1), 
      caption = "Factor levels and frequency of variable 'Hand'")

kable(table(dt$SES, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(SES = Var1), 
      caption = "Factor levels and frequency of variable 'SES'")

kable(table(dt$CDR, useNA = "ifany") %>% 
        data.frame() %>%
        dplyr::rename(CDR = Var1), 
      caption = "Factor levels and frequency of variable 'CDR'")
```

### Numerical variables

```{r overview numerical}
kable(dt %>% 
        dplyr::select(all_of(numeric.var)) %>%
        summary() %>% 
        data.frame() %>%
        dplyr::select(-Var1) %>%
        dplyr::rename(Variable = Var2) %>%
        dplyr::mutate(Freq = gsub(" ", "", Freq, fixed = TRUE),
                      Freq = gsub("\'", "", Freq, fixed = TRUE)) %>%
        filter(!is.na(Freq)) %>%
        separate(Freq, c("V1", "V2"), sep= ":") %>%
        group_by(Variable) %>%
        spread(V1, V2),
      caption = "Summary of all numeric variables in the dataset")
```

```{r histograms, out.width="50%"}
#fig.show="hold",  suppress = T
par(mar = c(4, 4, 1, .1))
#distribution
distribution_check_plots <- function(variable) {
  a <- dt %>% filter(!is.na(.[[variable]])) 
  
  hist(a[[variable]], 
       breaks = 50, 
       xlab = paste0(variable),
       main = paste0("Histogram of ", variable))
}

invisible(lapply(numeric.var,distribution_check_plots))
```

## Table 1

Create a typical table one. Show it in this Markdown and save it also as csv for integrating it, for example, into a manuscript. 

```{r table 1}
# 1. Select variables for table
myVars <- names(dt %>%
                  dplyr::select(-Subject.ID, -MRI.ID))


# 2. Make a list of all categorical variables from the myVars list for this table
catVars <- dt %>% 
  dplyr::select(all_of(myVars)) %>%
  dplyr::select_if(purrr::negate(is.numeric)) %>% 
  names()

# 3. Check visually for non-normally distributed numerical variables 
#hist() or histograms
nonnormaldist <- c("MR.Delay", "MMSE")

# 4. Create dataset for table 1
table1dt <- dt %>%
  dplyr::select(all_of(myVars))

# 5. Create table
tab1 <- tableone::CreateTableOne(data = table1dt, 
                                 vars = myVars, 
                                 factorVars = catVars)

# 6. Modify list of table to get a data frame
tab2 <- print(tab1, 
              showAllLevels = TRUE, 
              printToggle = FALSE, 
              noSpaces = TRUE, 
              explain = FALSE, 
              test = TRUE, 
              contDigits = 1, 
              catDigits = 1, 
              missing = TRUE,
              nonnormal = nonnormaldist) %>%
  as.data.frame() %>% 
  data.table::setDT(., keep.rownames = TRUE)
#summary(tab1) #gives more information regarding missing values

# 7. Rename column and rownames (of rn) to create a nice table that can be used in a manuscript
tab3 <- tab2 %>%
  dplyr::mutate(
    rn = ifelse(grepl("^X", rn), "", rn)) %>%
  dplyr::mutate(
    rn = dplyr::recode(rn,
                       "Group" = "Group, N (%)",
                       "Visit" = "Number of participants per visit, N (%)",
                       "MR.Delay" = "Time delay for MR imaging, median [IQR]",
                       "M.F" = "Sex, N (%)",
                       "Hand" = "Handedness, N (%)",
                       "Age" = "Age (years), M (SD)",
                       "EDUC" = "Education (years), M (SD)",
                       "SES" = "Socioeconomic status, N (%)",
                       "MMSE" = "Mini mental state examination, median [IQR]",
                       "CDR" = "Clinical Dementia Rating, N (%)",
                       "eTIV" = "Estimated total intracranial volume (cm3), M (SD)",
                       "nWBV" = "Normalized whole-brain volume, M (SD)",
                       "ASF" = "Atlas scaling factor, M (SD)"),
    level = dplyr::recode(level,
                          "F" = "women",
                          "M" = "men",
                          "R" = "right")) %>%
  dplyr::rename("Characteristic" = "rn") 

# 8. Print table in markdown
kable(tab3)

# 9. Save table for manuscript
write.csv(tab3, file = "Table-1.csv", row.names = FALSE) 
```

## Association between and and education and age and MMSE

```{r associations with age, message=FALSE}
# To be able to create a multitude of plots with different y-axis values I created a function for the plots and used a loop (lapply) to create them. 

plotAgeAssociation <- function(yaxis) {
  # create complete cases dataset
  dt.plot <- dt %>%
    dplyr::select(Age, as.name(yaxis), CDR) %>%
    filter(complete.cases(.))
  
  # 1st plot including all CDR groups
  plot1 <- ggplot(data=dt.plot, aes_(x=~Age, y=as.name(yaxis), color=~CDR)) + 
    geom_point() +
    geom_smooth(method = lm) +
    theme(legend.position = "bottom")
  
  # 2nd plot excluding the moderate AD CDR group to visualise the associations of the other groups better
  plot2 <- ggplot(data=dt.plot, aes_(x=~Age, y=as.name(yaxis), color=~CDR)) + 
    geom_point() +
    geom_smooth(method = lm, 
                data=dt.plot%>%filter(CDR!="moderate AD"), 
                aes_(x=~Age, y=as.name(yaxis), color=~CDR)) +
    theme(legend.position="none")
  
  # save the legend separately to have just one joint legend below the plots
  legend <- ggpubr::as_ggplot(ggpubr::get_legend(plot1))
  
  # summarise both plots and the legend to one combined figure
  plot(((plot1 + 
           theme(legend.position="none")) +
          plot2)/
         legend +
         plot_layout(width = c(1,1), heights = c(13, 1)))
  
  # remove the single plots 1 and 2
  rm(plot1); rm(plot2)
}

# run functions for education and MMSE on the y-axis
invisible(lapply(c("EDUC", "MMSE"), plotAgeAssociation))
```

## Show how MMSE performance changes with time?
```{r longitudinal associations}
dt %>% 
  dplyr::select(Subject.ID, MR.Delay, MMSE, CDR) %>%
  filter(complete.cases(.)) %>%
  ggplot(aes(x = MR.Delay, y = MMSE, color = CDR, group = Subject.ID)) +
  geom_line()
```

## How do age, sex, education and CDR associate with MMSE performance for the first visit?

Create a regression model with MMSE as the outcome and age, sex, education and CDR as the independents. Use the no dementia group as reference group for CDR.

```{r regression model}
model <- lm(MMSE ~ Age + M.F + EDUC + relevel(factor(CDR, ordered=FALSE), ref = "no dementia"), 
            data = dt %>% filter(Visit == 1))
```

### Assumption check

```{r skewness of residuals and vif check}
kable(moments::skewness(model$residuals) %>%
        data.frame() %>%
        dplyr::rename("Skewness of residuals" = "."), 
      caption = "Skewness of model residuals")
kable(car::vif(model), 
      caption = "Variance-inflation factors to assess multicolinearity of model independents")
```

Plots to check other regression model assumptions
```{r plots to check the regression assumptions}
par(mfrow = c(2,2)) # Change the panel layout to 2 x 2
plot(model)
```

### Result table
```{r regression model results}
# Creating data frame including all results
result<- broom::tidy(model, conf.int = TRUE, conf.level = 0.95)
result$N <- model$residuals %>% length()
result$r.squared <- summary(model)$r.squared

# Modify outcome for export
result %>%
  dplyr::rename(independent = term) %>%
  dplyr::mutate(
    independent = dplyr::recode(independent,
                         M.FM = 'Sex (ref. women)',
                         EDUC = 'Education (in years)',
                         'relevel(factor(CDR, ordered = FALSE), ref = \"no dementia\")mild AD' = 'CDR (ref. no dementia) mild AD',
                         'relevel(factor(CDR, ordered = FALSE), ref = \"no dementia\")very mild AD' = 'CDR (ref. no dementia) very mild AD')) %>%
  kable(., caption = "Outcome: MMSE")

```

## References R session information
```{r session info}
citation()
sessionInfo() 
lapply(list.of.packages, citation)

#RStudio.Version()[-2] #both functions are not working in 
#rstudioapi::versionInfo()[-2] 
## both functions for the R Studio version are not working in R markdown. Run this code in the console and enter in manually. 
```

To cite RStudio in publications use:
RStudio Team (2019). RStudio: Integrated Development for R. RStudio, Inc., Boston, MA URL <http://www.rstudio.com/.version>
Version: 1.2.5033
Release name: "Orange Blossom"




