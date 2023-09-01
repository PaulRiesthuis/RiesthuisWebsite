---
title: my new post
author: ''
date: '2023-09-01'
slug: my-new-post
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2023-09-01T13:41:41+02:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
output:
    html_document:     
    fig_width: 10
    fig_height: 8
    df_print: paged
    code_folding: hide
    theme: flatly
    toc: yes
    toc_depth: 3
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---

<style type="text/css">
.sidenote, .marginnote { 
  float: right;
  clear: right;
  margin-right: -60%;
  width: 57%;         # best between 50% and 60%
  margin-top: 0;
  margin-bottom: 0;
  font-size: 1.1rem;
  line-height: 1.3;
  vertical-align: baseline;
  position: relative;
  }
</style>
---



```r
library(readxl)
library(tidyverse)
library(ggforce)
library(patchwork)
library(ggridges)
library(lcsm)
library(data.table)
library(gt)
library(psych)
```

In this R Markdown file, we present the data analysis of the study examining the smallest effect size of interest among legal professionals

# Data Preparation
<button id="showButton">Show Explanation</button>

<div id="contentContainer" style="display:none;">
- Load the data 
- Transform in dataframe. 
- First line is skipped due to data format of Qualtrics.
- Rename variable of background job and current job for data analysis.
- Remove participants that did not complete the study (progress is not 100%)
- Change "never" Response to "12" to include in analyses. Hence 12 = no amount of errors would lead to a certain legal decision.
- Add participant column
- Rename long variable names into x1-x18
- Rename/group occupations (beroep)
- Add columns wherein participants are grouped based on their response to the first legal decision for each scenario (low, medium-low, medium-high, high amount of memory errors).
- Put the "group" variable in the correct order
- Put the "beroep"(occupation) in the correct order
</div>
<script>
document.getElementById("showButton").addEventListener("click", function() {
  var contentContainer = document.getElementById("contentContainer");
  if (contentContainer.style.display === "none") {
    contentContainer.style.display = "block";
  } else {
    contentContainer.style.display = "none";
  }
});
</script>


```r
dat_orignal <- read_xlsx("Raw_Data.xlsx",  skip = 1)
dat_orignal <- as.data.frame(dat_orignal)
setnames(dat_orignal, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_orignal, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_orignal <- subset(dat_orignal,dat_orignal$Progress==100)
dat_orignal$participants <- seq(1,295,1)
names(dat_orignal)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_orignal <- dat_orignal %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))
dat_orignal <- dat_orignal %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_orignal <- dat_orignal %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_orignal <- dat_orignal %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_orignal$group1  = factor(dat_orignal$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_orignal$group2  = factor(dat_orignal$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_orignal$group3  = factor(dat_orignal$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_orignal$beroep  = factor(dat_orignal$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))
```

## Scenario + Variables of interest + Legal decisions
<button id="showButton1">Show Explanation</button>

<div id="contentContainer1" style="display:none;">
- Presented scenario = Consider the following case of the People v. S.W.C. (2012). S.W.C. is charged with unarmed robbery. The police obtained incriminating evidence from one of the adult eyewitnesses of the robbery who stated in an initial interview that they saw S.W.C. at the scene of a crime wearing a *blue jacket*. Video footage confirmed that S.W.C. was wearing a blue jacket 15 minutes before the crime occurred. However, during a second interview, the eyewitness *mistakenly remembers and reports* that S.W.C. was actually wearing a *green jacket*. 
- *Three different memory errors (scenarios)*:
  + Scenario 1: Misremember colour jacket
  + Scenario 2: Misremember entire new detail (i.e., yellow hat)
  + Scenario 3: Misremember entire new incriminating detail (i.e., black gun)
- For each scenario they are asked 6 questions
  + Question 1 - In your opinion, how many of these errors in an eyewitness account is sufficient to consider the eyewitness testimony unreliable 
  + Question 2 - In your opinion, how many of these errors in an eyewitness account is sufficient to consider the eyewitness testimony inadmissible 
  + Question 3 - In your opinion, how many of these errors in an eyewitness account would generally lead you to challenge the admissibility of the witness? 
  + Question 4 - In your opinion, how many of these errors in an eyewitness account would make a judge in your jurisdiction rule the eyewitness’ evidence inadmissible? 
  + Question 5 - In your opinion, how many of these errors in an eyewitness account would it take for you to consider retaining an expert witness to inform the factfinder about the functioning of human memory?
  + Question 6 - In your opinion, how many of these errors in an eyewitness account would generally cause the average person to reduce the weight they place on the eyewitness’ evidence? 

  
- *Response option:* Slider from 0 – 10 errors; extra option = No decision no matter how many errors.

- *Dependent variables:*
  + x1    = Scenario 1  -   Question 1
  + x2    = Scenario 1  -   Question 2
  + x3    = Scenario 1  -   Question 3
  + x4    = Scenario 1  -   Question 4
  + x5    = Scenario 1  -   Question 5
  + x6    = Scenario 1  -   Question 6
  + x7    = Scenario 2  -   Question 1
  + x8    = Scenario 2  -   Question 2
  + x9    = Scenario 2  -   Question 3
  + x10   = Scenario 2  -   Question 4
  + x11   = Scenario 2  -   Question 5
  + x12   = Scenario 2  -   Question 6
  + x13   = Scenario 3  -   Question 1
  + x14   = Scenario 3  -   Question 2
  + x15   = Scenario 3  -   Question 3
  + x16   = Scenario 3  -   Question 4
  + x17   = Scenario 3  -   Question 5
  + x18   = Scenario 3  -   Question 6
  
</div>
<script>
document.getElementById("showButton1").addEventListener("click", function() {
  var contentContainer1 = document.getElementById("contentContainer1");
  if (contentContainer1.style.display === "none") {
    contentContainer1.style.display = "block";
  } else {
    contentContainer1.style.display = "none";
  }
});
</script>

# Data Analysis - Summary Statistics
First, analyses are run on all participants together with no strict exclusion criteria.
## Summary Statistics
First, let's create a table to examine the summary statistics.

<button id="showButton2">Show Table</button>

<div id="contentContainer2" style="display:none;">

```r
dat_table <- subset(dat_orignal, select = c(x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))
describe(dat_table, fast = FALSE)
```

```
##     vars   n mean   sd median trimmed  mad min max range skew kurtosis   se
## x1     1 273 4.73 2.38   4.50    4.65 2.67 0.0  10  10.0 0.28    -0.91 0.14
## x2     2 260 4.99 2.45   4.95    4.95 2.89 0.7  10   9.3 0.13    -1.03 0.15
## x3     3 265 4.82 2.29   4.80    4.77 2.52 0.0  10  10.0 0.20    -0.77 0.14
## x4     4 266 4.98 2.42   4.90    4.94 2.82 0.5  10   9.5 0.18    -0.95 0.15
## x5     5 264 4.71 2.34   4.70    4.67 2.82 0.0  10  10.0 0.10    -1.02 0.14
## x6     6 269 5.14 2.46   5.10    5.13 2.97 0.0  10  10.0 0.02    -0.97 0.15
## x7     7 280 5.13 2.47   4.95    5.07 2.89 0.0  10  10.0 0.21    -0.88 0.15
## x8     8 268 5.08 2.41   5.00    5.05 2.82 0.0  10  10.0 0.10    -0.92 0.15
## x9     9 268 5.07 2.37   4.90    5.04 2.67 0.0  10  10.0 0.12    -0.84 0.14
## x10   10 265 4.99 2.42   5.00    4.96 2.97 0.0  10  10.0 0.12    -0.89 0.15
## x11   11 263 4.86 2.37   4.80    4.84 2.67 0.0  10  10.0 0.08    -0.87 0.15
## x12   12 275 5.14 2.30   5.00    5.09 2.67 0.0  10  10.0 0.18    -0.71 0.14
## x13   13 278 4.73 2.53   4.80    4.70 2.82 0.0  10  10.0 0.10    -0.99 0.15
## x14   14 265 4.92 2.50   5.00    4.92 2.97 0.2  10   9.8 0.02    -0.98 0.15
## x15   15 257 4.82 2.54   4.80    4.80 2.82 0.0  10  10.0 0.10    -1.07 0.16
## x16   16 265 5.06 2.55   5.00    5.05 3.26 0.0  10  10.0 0.01    -1.10 0.16
## x17   17 272 4.77 2.39   4.70    4.73 2.67 0.1  10   9.9 0.13    -0.87 0.14
## x18   18 281 4.94 2.59   5.00    4.90 3.26 0.0  10  10.0 0.12    -1.06 0.15
```
</div>
<script>
document.getElementById("showButton2").addEventListener("click", function() {
  var contentContainer2 = document.getElementById("contentContainer2");
  if (contentContainer2.style.display === "none") {
    contentContainer2.style.display = "block";
  } else {
    contentContainer2.style.display = "none";
  }
});
</script>

## Summary Statistics grouped by occupation

<button id="showButton3">Show Table</button>

<div id="contentContainer3" style="display:none;">

```r
dat_table <- subset(dat_orignal, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))
describeBy(dat_table, group = "beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars  n mean   sd min  max range   se
## beroep    1 67  NaN   NA Inf -Inf  -Inf   NA
## x1        2 61 4.95 2.30 0.0  9.8   9.8 0.29
## x2        3 59 4.96 2.18 1.1  9.8   8.7 0.28
## x3        4 61 5.07 2.14 1.4 10.0   8.6 0.27
## x4        5 61 5.22 2.26 0.5  8.9   8.4 0.29
## x5        6 66 4.98 2.20 1.0  9.5   8.5 0.27
## x6        7 61 5.52 2.37 0.6  9.9   9.3 0.30
## x7        8 64 5.48 2.35 1.0 10.0   9.0 0.29
## x8        9 63 5.33 2.25 1.0  9.8   8.8 0.28
## x9       10 63 4.82 2.03 0.7  9.1   8.4 0.26
## x10      11 63 5.13 2.27 0.2  9.1   8.9 0.29
## x11      12 64 4.88 2.14 0.9  9.2   8.3 0.27
## x12      13 64 5.18 2.15 1.1 10.0   8.9 0.27
## x13      14 62 5.27 2.40 0.6 10.0   9.4 0.31
## x14      15 63 5.09 2.14 0.9  9.8   8.9 0.27
## x15      16 63 5.10 2.13 1.0  9.1   8.1 0.27
## x16      17 61 5.24 2.34 1.5  9.9   8.4 0.30
## x17      18 63 5.21 2.02 1.0 10.0   9.0 0.25
## x18      19 65 5.19 2.42 1.0 10.0   9.0 0.30
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars  n mean   sd min  max range   se
## beroep    1 32  NaN   NA Inf -Inf  -Inf   NA
## x1        2 30 5.08 2.27 0.8  9.1   8.3 0.42
## x2        3 29 5.59 2.69 0.8 10.0   9.2 0.50
## x3        4 29 5.19 2.30 0.8  9.1   8.3 0.43
## x4        5 29 5.21 2.72 0.7 10.0   9.3 0.51
## x5        6 30 4.87 2.29 0.5  8.6   8.1 0.42
## x6        7 30 5.44 2.28 1.0  9.4   8.4 0.42
## x7        8 31 5.37 2.58 1.0  9.7   8.7 0.46
## x8        9 29 5.75 2.62 0.6 10.0   9.4 0.49
## x9       10 29 5.51 2.45 1.3 10.0   8.7 0.46
## x10      11 29 5.52 2.50 1.4 10.0   8.6 0.46
## x11      12 30 5.04 2.44 1.6  8.9   7.3 0.45
## x12      13 31 5.47 2.55 1.2 10.0   8.8 0.46
## x13      14 31 4.94 2.53 0.8  9.5   8.7 0.45
## x14      15 28 5.56 2.55 1.1  9.7   8.6 0.48
## x15      16 28 5.24 2.64 1.3  9.2   7.9 0.50
## x16      17 30 5.34 2.65 1.2 10.0   8.8 0.48
## x17      18 31 5.21 2.66 0.8 10.0   9.2 0.48
## x18      19 31 5.75 2.84 0.7 10.0   9.3 0.51
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars   n mean   sd min  max range   se
## beroep    1 139  NaN   NA Inf -Inf  -Inf   NA
## x1        2 127 4.81 2.52 0.6 10.0   9.4 0.22
## x2        3 122 5.08 2.59 0.7 10.0   9.3 0.23
## x3        4 124 4.87 2.48 0.0 10.0  10.0 0.22
## x4        5 121 5.06 2.53 0.7 10.0   9.3 0.23
## x5        6 116 4.70 2.51 0.5 10.0   9.5 0.23
## x6        7 125 5.12 2.58 0.7 10.0   9.3 0.23
## x7        8 130 5.08 2.50 0.0 10.0  10.0 0.22
## x8        9 123 5.05 2.52 0.0 10.0  10.0 0.23
## x9       10 126 5.27 2.51 0.0 10.0  10.0 0.22
## x10      11 120 5.11 2.42 0.0 10.0  10.0 0.22
## x11      12 120 5.06 2.52 0.0 10.0  10.0 0.23
## x12      13 127 5.11 2.40 0.0 10.0  10.0 0.21
## x13      14 130 4.63 2.69 0.5 10.0   9.5 0.24
## x14      15 125 4.93 2.63 0.6 10.0   9.4 0.24
## x15      16 120 4.90 2.74 0.0 10.0  10.0 0.25
## x16      17 124 5.25 2.67 0.5 10.0   9.5 0.24
## x17      18 126 4.60 2.54 0.1  9.6   9.5 0.23
## x18      19 131 4.83 2.67 0.7 10.0   9.3 0.23
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars  n mean   sd min  max range   se
## beroep    1 47  NaN   NA Inf -Inf  -Inf   NA
## x1        2 46 4.34 2.19 1.0  9.1   8.1 0.32
## x2        3 43 4.73 2.21 1.5 10.0   8.5 0.34
## x3        4 46 4.29 1.90 1.2  9.1   7.9 0.28
## x4        5 46 4.54 2.17 0.6  8.6   8.0 0.32
## x5        6 45 4.39 2.07 1.1  9.1   8.0 0.31
## x6        7 43 4.97 2.29 0.0  9.4   9.4 0.35
## x7        8 46 4.82 2.58 0.5 10.0   9.5 0.38
## x8        9 47 4.45 2.07 0.5  9.4   8.9 0.30
## x9       10 45 4.74 2.32 0.5 10.0   9.5 0.35
## x10      11 45 4.05 2.30 0.4  9.4   9.0 0.34
## x11      12 42 4.44 2.09 1.4  9.1   7.7 0.32
## x12      13 44 5.22 2.07 1.0  9.8   8.8 0.31
## x13      14 45 4.43 2.16 0.0  8.9   8.9 0.32
## x14      15 43 4.57 2.48 0.2  9.5   9.3 0.38
## x15      16 42 4.11 2.30 0.1  9.1   9.0 0.35
## x16      17 43 4.34 2.23 0.0  8.5   8.5 0.34
## x17      18 43 4.50 2.16 0.2  8.8   8.6 0.33
## x18      19 44 4.72 2.34 0.0  9.7   9.7 0.35
## ------------------------------------------------------------ 
## beroep: Other
##        vars  n mean   sd min  max range   se
## beroep    1 10  NaN   NA Inf -Inf  -Inf   NA
## x1        2  9 2.88 1.39 1.0  5.0   4.0 0.46
## x2        3  7 2.64 1.49 1.0  5.0   4.0 0.56
## x3        4  5 3.18 1.93 1.0  6.0   5.0 0.86
## x4        5  9 3.82 2.16 1.0  7.1   6.1 0.72
## x5        6  7 3.57 2.64 0.0  8.0   8.0 1.00
## x6        7 10 2.98 1.69 1.0  5.0   4.0 0.53
## x7        8  9 4.09 1.90 0.1  6.0   5.9 0.63
## x8        9  6 4.73 2.76 0.1  8.0   7.9 1.13
## x9       10  5 3.68 2.19 0.1  6.1   6.0 0.98
## x10      11  8 5.41 3.06 0.1 10.0   9.9 1.08
## x11      12  7 3.01 2.53 0.0  7.0   7.0 0.96
## x12      13  9 3.64 1.65 0.1  5.0   4.9 0.55
## x13      14 10 3.43 2.28 1.0  6.8   5.8 0.72
## x14      15  6 2.50 2.34 1.0  7.0   6.0 0.95
## x15      16  4 2.25 1.89 1.0  5.0   4.0 0.95
## x16      17  7 3.31 2.69 1.0  8.2   7.2 1.02
## x17      18  9 3.76 2.42 1.0  7.0   6.0 0.81
## x18      19 10 3.30 2.29 0.8  7.6   6.8 0.72
```
</div>
<script>
document.getElementById("showButton3").addEventListener("click", function() {
  var contentContainer3 = document.getElementById("contentContainer3");
  if (contentContainer3.style.display === "none") {
    contentContainer3.style.display = "block";
  } else {
    contentContainer3.style.display = "none";
  }
});
</script>

Results seem to indicate that there are no differences between scenarions or the different occupations and that within the groups there is no general consensus.

# Data -Analysis - Individual Trace Plots (all participants by occupation)
Individual trace graphs will be plotted for amount of memory errors for each legal decision by occupation and their response to the first legal decision. Participants who made no decision are assigned to the number 12 for plotting purposes. The plots need variable lists.

## *Scenario 1 - Misremember colour jacket*
<button id="showButton4">Show Graph Scenario 1</button>

<div id="graphContainer4" style="display:none;">

```r
dat_scen <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_scen <- as.data.frame(dat_scen)
setnames(dat_scen, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_scen, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_scen <- subset(dat_scen,dat_scen$Progress==100)
dat_scen[is.na(dat_scen)] <- 12
dat_scen$participants <- seq(1,295,1)
names(dat_scen)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_scen <- dat_scen %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))
dat_scen <- dat_scen %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_scen <- dat_scen %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_scen <- dat_scen %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))

dat_scen$group1  = factor(dat_scen$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen$group2  = factor(dat_scen$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen$group3  = factor(dat_scen$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))

dat_scen$beroep  = factor(dat_scen$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

first_var_list <- c("x1", "x2", "x3", "x4", "x5", "x6")
second_var_list <- c("x7", "x8", "x9", "x10", "x11", "x12")
third_var_list <- c("x13", "x14", "x15", "x16", "x17", "x18")

scen1 <- plot_trajectories(data = dat_scen,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
  
scen1
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" height="100%" />
</div>
<script>
document.getElementById("showButton4").addEventListener("click", function() {
  var graphContainer4 = document.getElementById("graphContainer4");
  if (graphContainer4.style.display === "none") {
    graphContainer4.style.display = "block";
  } else {
    graphContainer4.style.display = "none";
  }
});
</script>


## *Scenario 2 - Misremember entire new detail (i.e., yellow hat)*
<button id="showButton5">Show Graph Scenario 2</button>

<div id="graphContainer5" style="display:none;">

```r
scen2 <- plot_trajectories(data = dat_scen,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" height="100%" />
</div>
<script>
document.getElementById("showButton5").addEventListener("click", function() {
  var graphContainer5 = document.getElementById("graphContainer5");
  if (graphContainer5.style.display === "none") {
    graphContainer5.style.display = "block";
  } else {
    graphContainer5.style.display = "none";
  }
});
</script>

## *Scenario 3 - Misremember entire new incriminating detail (i.e., black gun)*
<button id="showButton6">Show Graph Scenario 3</button>

<div id="graphContainer6" style="display:none;">

```r
scen3 <- plot_trajectories(data = dat_scen,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" height="100%" />
</div>
<script>
document.getElementById("showButton6").addEventListener("click", function() {
  var graphContainer6 = document.getElementById("graphContainer6");
  if (graphContainer6.style.display === "none") {
    graphContainer6.style.display = "block";
  } else {
    graphContainer6.style.display = "none";
  }
});
</script>

# Data Analysis - Specific Countries

## Summary Statistics by Country of Practice
Dutch, Belgian, and USA participants are put in separate groups. Remaining countries are put together in other.



<button id="showButtonc">Show Summary Stats per Country</button>

<div id="contentContainerc" style="display:none;">


```r
dat_tab_count <- subset(dat_country, select = c(country,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))
describeBy(dat_tab_count, group = "country", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## country: NL
##         vars  n mean   sd min  max range   se
## country    1 40  NaN   NA Inf -Inf  -Inf   NA
## x1         2 35 2.89 1.87 0.6  8.6   8.0 0.32
## x2         3 28 3.68 2.26 0.7  8.8   8.1 0.43
## x3         4 30 3.28 2.24 0.0 10.0  10.0 0.41
## x4         5 29 4.37 2.67 0.7 10.0   9.3 0.50
## x5         6 24 2.61 1.64 0.5  6.6   6.1 0.33
## x6         7 32 3.54 2.27 0.7  8.2   7.5 0.40
## x7         8 37 3.48 2.32 0.0 10.0  10.0 0.38
## x8         9 29 3.33 2.16 0.0  8.2   8.2 0.40
## x9        10 30 3.84 2.29 0.0  9.2   9.2 0.42
## x10       11 28 4.82 2.60 0.0 10.0  10.0 0.49
## x11       12 25 3.26 1.92 0.0  7.2   7.2 0.38
## x12       13 34 3.84 2.29 0.0  9.2   9.2 0.39
## x13       14 37 2.35 1.92 0.5  6.8   6.3 0.32
## x14       15 30 2.37 1.93 0.6  8.2   7.6 0.35
## x15       16 28 2.46 2.15 0.7  8.5   7.8 0.41
## x16       17 30 3.62 2.73 0.5 10.0   9.5 0.50
## x17       18 32 2.75 2.05 0.7  6.9   6.2 0.36
## x18       19 38 3.06 2.29 0.7 10.0   9.3 0.37
## ------------------------------------------------------------ 
## country: BE
##         vars  n mean   sd min  max range   se
## country    1 29  NaN   NA Inf -Inf  -Inf   NA
## x1         2 26 2.95 1.98 0.7  8.0   7.3 0.39
## x2         3 23 2.63 1.54 0.8  6.0   5.2 0.32
## x3         4 22 3.45 2.25 0.8 10.0   9.2 0.48
## x4         5 24 3.28 2.24 0.6  8.2   7.6 0.46
## x5         6 24 3.02 1.88 0.0  8.0   8.0 0.38
## x6         7 26 2.80 1.87 0.0  7.8   7.8 0.37
## x7         8 25 3.81 1.92 0.1  8.0   7.9 0.38
## x8         9 21 3.81 2.44 0.1 10.0   9.9 0.53
## x9        10 20 3.94 2.31 0.1 10.0   9.9 0.52
## x10       11 21 3.99 2.79 0.1 10.0   9.9 0.61
## x11       12 22 3.65 2.54 0.0 10.0  10.0 0.54
## x12       13 25 3.87 2.18 0.1 10.0   9.9 0.44
## x13       14 26 2.97 2.31 0.1  8.0   7.9 0.45
## x14       15 21 2.78 2.74 0.2 10.0   9.8 0.60
## x15       16 17 2.28 1.80 0.1  6.1   6.0 0.44
## x16       17 22 3.37 2.73 0.0 10.0  10.0 0.58
## x17       18 24 3.41 2.62 0.2 10.0   9.8 0.53
## x18       19 26 2.52 1.65 0.0  6.0   6.0 0.32
## ------------------------------------------------------------ 
## country: USA
##         vars  n mean   sd min  max range   se
## country    1 99  NaN   NA Inf -Inf  -Inf   NA
## x1         2 92 4.77 2.10 0.0  9.4   9.4 0.22
## x2         3 87 5.21 2.31 0.8 10.0   9.2 0.25
## x3         4 92 4.46 1.78 0.8 10.0   9.2 0.19
## x4         5 89 4.75 2.17 1.3 10.0   8.7 0.23
## x5         6 93 4.66 2.00 0.8 10.0   9.2 0.21
## x6         7 87 5.48 2.16 1.2 10.0   8.8 0.23
## x7         8 93 5.40 2.22 1.0 10.0   9.0 0.23
## x8         9 93 5.07 2.11 0.3 10.0   9.7 0.22
## x9        10 93 4.98 1.98 0.3 10.0   9.7 0.20
## x10       11 91 4.41 1.94 0.3  9.4   9.1 0.20
## x11       12 91 4.55 1.85 0.3  9.0   8.7 0.19
## x12       13 91 5.11 1.89 0.7 10.0   9.3 0.20
## x13       14 90 5.25 2.14 0.0 10.0  10.0 0.23
## x14       15 89 5.16 1.97 0.8  9.8   9.0 0.21
## x15       16 88 4.91 2.21 0.2 10.0   9.8 0.24
## x16       17 88 4.90 2.23 1.0 10.0   9.0 0.24
## x17       18 92 4.72 1.86 0.1 10.0   9.9 0.19
## x18       19 92 5.24 2.22 0.2 10.0   9.8 0.23
## ------------------------------------------------------------ 
## country: Other
##         vars   n mean   sd min  max range   se
## country    1 127  NaN   NA Inf -Inf  -Inf   NA
## x1         2 120 5.62 2.31 0.9 10.0   9.1 0.21
## x2         3 122 5.58 2.39 0.9 10.0   9.1 0.22
## x3         4 121 5.72 2.29 0.8 10.0   9.2 0.21
## x4         5 124 5.62 2.37 0.5 10.0   9.5 0.21
## x5         6 123 5.48 2.36 0.5  9.5   9.0 0.21
## x6         7 124 5.81 2.35 0.6 10.0   9.4 0.21
## x7         8 125 5.68 2.51 0.5 10.0   9.5 0.22
## x8         9 125 5.70 2.42 0.5 10.0   9.5 0.22
## x9        10 125 5.62 2.50 0.5 10.0   9.5 0.22
## x10       11 125 5.62 2.49 0.2 10.0   9.8 0.22
## x11       12 125 5.62 2.48 0.0  9.4   9.4 0.22
## x12       13 125 5.77 2.35 0.7 10.0   9.3 0.21
## x13       14 125 5.43 2.41 0.6 10.0   9.4 0.22
## x14       15 125 5.72 2.34 0.5 10.0   9.5 0.21
## x15       16 124 5.63 2.38 0.0  9.4   9.4 0.21
## x16       17 125 5.81 2.39 1.1  9.9   8.8 0.21
## x17       18 124 5.58 2.37 0.4  9.8   9.4 0.21
## x18       19 125 5.80 2.52 0.7 10.0   9.3 0.23
```

</div>
<script>
document.getElementById("showButtonc").addEventListener("click", function() {
  var contentContainerc = document.getElementById("contentContainerc");
  if (contentContainerc.style.display === "none") {
    contentContainerc.style.display = "block";
  } else {
    contentContainerc.style.display = "none";
  }
});
</script>

The data seem to be strange as there are many USA legal professionals and other countries. See below for more analyses with stricter exclusion criteria


## The Netherlands/ N = 40
<button id="showButtonnl">Netherlands Summary Statistics</button>

<div id="contentContainernl" style="display:none;">


```r
dat_scen1_NL <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_scen1_NL <- as.data.frame(dat_scen1_NL)
setnames(dat_scen1_NL, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_scen1_NL, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_scen1_NL <- subset(dat_scen1_NL,dat_scen1_NL$Progress==100)
dat_scen1_NL$participants <- seq(1,295,1)
names(dat_scen1_NL)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_scen1_NL <- dat_scen1_NL %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))
dat_scen1_NL <- dat_scen1_NL %>% 
  mutate(country = case_when(`List of Countries...28` == "Netherlands" ~ "NL",
                      `List of Countries...28`  == "Belgium" ~ "BE",
                      `List of Countries...28`  == "United States of America" ~ "USA",
                      TRUE ~ "Other"))

dat_scen1_NL$beroep = factor(dat_scen1_NL$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

dat_scen1_NL <- subset(dat_scen1_NL, dat_scen1_NL$country=="NL")


dat_table_NL <- subset(dat_scen1_NL, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_NL, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
## NULL
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars n mean sd min  max range se
## beroep    1 1  NaN NA Inf -Inf  -Inf NA
## x1        2 1  6.3 NA 6.3  6.3     0 NA
## x2        3 0  NaN NA Inf -Inf  -Inf NA
## x3        4 0  NaN NA Inf -Inf  -Inf NA
## x4        5 0  NaN NA Inf -Inf  -Inf NA
## x5        6 1  5.2 NA 5.2  5.2     0 NA
## x6        7 1  7.2 NA 7.2  7.2     0 NA
## x7        8 1  7.3 NA 7.3  7.3     0 NA
## x8        9 0  NaN NA Inf -Inf  -Inf NA
## x9       10 0  NaN NA Inf -Inf  -Inf NA
## x10      11 0  NaN NA Inf -Inf  -Inf NA
## x11      12 1  4.2 NA 4.2  4.2     0 NA
## x12      13 1  7.0 NA 7.0  7.0     0 NA
## x13      14 1  6.2 NA 6.2  6.2     0 NA
## x14      15 0  NaN NA Inf -Inf  -Inf NA
## x15      16 0  NaN NA Inf -Inf  -Inf NA
## x16      17 0  NaN NA Inf -Inf  -Inf NA
## x17      18 1  4.2 NA 4.2  4.2     0 NA
## x18      19 1  6.4 NA 6.4  6.4     0 NA
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 37  NaN   NA Inf -Inf  -Inf   NA
## x1        2 32 2.79 1.84 0.6  8.6   8.0 0.33
## x2        3 27 3.74 2.28 0.7  8.8   8.1 0.44
## x3        4 29 3.32 2.26 0.0 10.0  10.0 0.42
## x4        5 27 4.38 2.70 0.7 10.0   9.3 0.52
## x5        6 23 2.50 1.57 0.5  6.6   6.1 0.33
## x6        7 29 3.45 2.22 0.7  8.2   7.5 0.41
## x7        8 35 3.30 2.25 0.0 10.0  10.0 0.38
## x8        9 29 3.33 2.16 0.0  8.2   8.2 0.40
## x9       10 29 3.83 2.33 0.0  9.2   9.2 0.43
## x10      11 27 4.73 2.60 0.0 10.0  10.0 0.50
## x11      12 24 3.22 1.95 0.0  7.2   7.2 0.40
## x12      13 32 3.70 2.28 0.0  9.2   9.2 0.40
## x13      14 34 2.14 1.69 0.5  6.2   5.7 0.29
## x14      15 29 2.41 1.95 0.6  8.2   7.6 0.36
## x15      16 27 2.51 2.17 0.7  8.5   7.8 0.42
## x16      17 29 3.71 2.73 0.5 10.0   9.5 0.51
## x17      18 29 2.62 1.95 0.7  6.8   6.1 0.36
## x18      19 35 2.90 2.14 0.7 10.0   9.3 0.36
## ------------------------------------------------------------ 
## beroep: Parelegal
## NULL
## ------------------------------------------------------------ 
## beroep: Other
##        vars n mean   sd min  max range   se
## beroep    1 2  NaN   NA Inf -Inf  -Inf   NA
## x1        2 2 2.80 1.13 2.0  3.6   1.6 0.80
## x2        3 1 2.00   NA 2.0  2.0   0.0   NA
## x3        4 1 2.00   NA 2.0  2.0   0.0   NA
## x4        5 2 4.25 3.18 2.0  6.5   4.5 2.25
## x5        6 0  NaN   NA Inf -Inf  -Inf   NA
## x6        7 2 3.00 2.83 1.0  5.0   4.0 2.00
## x7        8 1 6.00   NA 6.0  6.0   0.0   NA
## x8        9 0  NaN   NA Inf -Inf  -Inf   NA
## x9       10 1 4.20   NA 4.2  4.2   0.0   NA
## x10      11 1 7.20   NA 7.2  7.2   0.0   NA
## x11      12 0  NaN   NA Inf -Inf  -Inf   NA
## x12      13 1 5.00   NA 5.0  5.0   0.0   NA
## x13      14 2 3.90 4.10 1.0  6.8   5.8 2.90
## x14      15 1 1.10   NA 1.1  1.1   0.0   NA
## x15      16 1 1.00   NA 1.0  1.0   0.0   NA
## x16      17 1 1.00   NA 1.0  1.0   0.0   NA
## x17      18 2 3.95 4.17 1.0  6.9   5.9 2.95
## x18      19 2 4.20 4.81 0.8  7.6   6.8 3.40
```

</div>
<script>
document.getElementById("showButtonnl").addEventListener("click", function() {
  var contentContainernl = document.getElementById("contentContainernl");
  if (contentContainernl.style.display === "none") {
    contentContainernl.style.display = "block";
  } else {
    contentContainernl.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 1

<button id="showButtonnl1">Show Graph Scen 1/NL</button>

<div id="graphContainernl1" style="display:none;">

```r
dat_scen1_NL[is.na(dat_scen1_NL)] <- 12
dat_scen1_NL <- dat_scen1_NL %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_scen1_NL <- dat_scen1_NL %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_scen1_NL <- dat_scen1_NL %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_scen1_NL$group1  = factor(dat_scen1_NL$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_NL$group2  = factor(dat_scen1_NL$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_NL$group3  = factor(dat_scen1_NL$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))

scen1_nl <- plot_trajectories(data = dat_scen1_NL,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_nl
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnl1").addEventListener("click", function() {
  var graphContainernl1 = document.getElementById("graphContainernl1");
  if (graphContainernl1.style.display === "none") {
    graphContainernl1.style.display = "block";
  } else {
    graphContainernl1.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 2

<button id="showButtonnl2">Show Graph Scen 2/NL</button>

<div id="graphContainernl2" style="display:none;">

```r
scen2_nl <- plot_trajectories(data = dat_scen1_NL,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_nl
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnl2").addEventListener("click", function() {
  var graphContainernl2 = document.getElementById("graphContainernl2");
  if (graphContainernl2.style.display === "none") {
    graphContainernl2.style.display = "block";
  } else {
    graphContainernl2.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 3

<button id="showButtonnl3">Show Graph Scen 3/NL</button>

<div id="graphContainernl3" style="display:none;">

```r
scen_nl3 <- plot_trajectories(data = dat_scen1_NL,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen_nl3
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnl3").addEventListener("click", function() {
  var graphContainernl3 = document.getElementById("graphContainernl3");
  if (graphContainernl3.style.display === "none") {
    graphContainernl3.style.display = "block";
  } else {
    graphContainernl3.style.display = "none";
  }
});
</script>


## Belgium/ N = 29
<button id="showButtonbe">Belgium Summary Statistics</button>

<div id="contentContainerbe" style="display:none;">


```r
dat_scen1_BE <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_scen1_BE <- as.data.frame(dat_scen1_BE)
setnames(dat_scen1_BE, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_scen1_BE, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_scen1_BE <- subset(dat_scen1_BE,dat_scen1_BE$Progress==100)
dat_scen1_BE$participants <- seq(1,295,1)
names(dat_scen1_BE)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_scen1_BE <- dat_scen1_BE %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))

dat_scen1_BE <- dat_scen1_BE %>% 
  mutate(country = case_when(`List of Countries...28` == "Netherlands" ~ "NL",
                      `List of Countries...28`  == "Belgium" ~ "BE",
                      `List of Countries...28`  == "United States of America" ~ "USA",
                      TRUE ~ "Other"))

dat_scen1_BE$beroep = factor(dat_scen1_BE$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

dat_scen1_BE <- subset(dat_scen1_BE, dat_scen1_BE$country=="BE")


dat_table_BE <- subset(dat_scen1_BE, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_BE, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars n mean   sd min  max range   se
## beroep    1 2  NaN   NA Inf -Inf  -Inf   NA
## x1        2 2 1.70 0.42 1.4  2.0   0.6 0.30
## x2        3 2 2.20 0.28 2.0  2.4   0.4 0.20
## x3        4 2 2.00 0.00 2.0  2.0   0.0 0.00
## x4        5 2 1.50 0.71 1.0  2.0   1.0 0.50
## x5        6 2 1.80 0.28 1.6  2.0   0.4 0.20
## x6        7 2 1.70 0.42 1.4  2.0   0.6 0.30
## x7        8 2 2.10 0.14 2.0  2.2   0.2 0.10
## x8        9 2 1.65 0.49 1.3  2.0   0.7 0.35
## x9       10 2 2.00 0.00 2.0  2.0   0.0 0.00
## x10      11 2 1.50 0.71 1.0  2.0   1.0 0.50
## x11      12 2 1.70 0.42 1.4  2.0   0.6 0.30
## x12      13 2 1.70 0.42 1.4  2.0   0.6 0.30
## x13      14 2 1.55 0.78 1.0  2.1   1.1 0.55
## x14      15 2 1.20 0.28 1.0  1.4   0.4 0.20
## x15      16 2 1.75 1.06 1.0  2.5   1.5 0.75
## x16      17 2 2.25 0.35 2.0  2.5   0.5 0.25
## x17      18 2 1.45 0.64 1.0  1.9   0.9 0.45
## x18      19 2 1.60 0.85 1.0  2.2   1.2 0.60
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars n mean   sd min  max range   se
## beroep    1 5  NaN   NA Inf -Inf  -Inf   NA
## x1        2 3 3.60 2.62 0.8  6.0   5.2 1.51
## x2        3 3 4.27 3.00 0.8  6.0   5.2 1.73
## x3        4 3 3.53 2.61 0.8  6.0   5.2 1.51
## x4        5 3 5.03 3.74 0.9  8.2   7.3 2.16
## x5        6 3 3.27 2.61 0.8  6.0   5.2 1.51
## x6        7 3 2.93 2.69 1.0  6.0   5.0 1.55
## x7        8 4 4.78 2.40 1.8  7.3   5.5 1.20
## x8        9 3 6.40 3.42 3.2 10.0   6.8 1.97
## x9       10 3 6.33 3.51 3.0 10.0   7.0 2.03
## x10      11 3 6.20 3.70 2.6 10.0   7.4 2.14
## x11      12 3 5.37 3.10 2.0  8.1   6.1 1.79
## x12      13 4 6.12 3.26 3.0 10.0   7.0 1.63
## x13      14 4 4.58 2.54 0.8  6.1   5.3 1.27
## x14      15 2 7.10 1.56 6.0  8.2   2.2 1.10
## x15      16 2 6.05 0.07 6.0  6.1   0.1 0.05
## x16      17 4 5.70 3.38 1.8 10.0   8.2 1.69
## x17      18 4 4.95 3.98 0.8 10.0   9.2 1.99
## x18      19 4 3.17 2.17 0.7  6.0   5.3 1.09
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars n mean   sd min  max range   se
## beroep    1 8  NaN   NA Inf -Inf  -Inf   NA
## x1        2 8 3.33 2.79 0.7  8.0   7.3 0.99
## x2        3 6 1.90 0.92 0.8  3.2   2.4 0.38
## x3        4 7 3.84 3.12 0.8 10.0   9.2 1.18
## x4        5 6 2.33 1.50 0.7  5.0   4.3 0.61
## x5        6 7 2.23 0.75 1.1  3.0   1.9 0.28
## x6        7 7 2.87 2.32 0.9  7.8   6.9 0.88
## x7        8 5 3.84 2.55 1.1  8.0   6.9 1.14
## x8        9 4 2.83 1.59 1.1  4.9   3.8 0.80
## x9       10 5 3.66 1.98 0.9  6.1   5.2 0.88
## x10      11 3 2.97 1.00 2.0  4.0   2.0 0.58
## x11      12 6 3.93 3.25 1.0 10.0   9.0 1.33
## x12      13 5 3.62 1.67 1.1  5.1   4.0 0.75
## x13      14 6 2.35 2.81 0.7  8.0   7.3 1.15
## x14      15 6 2.90 3.57 0.9 10.0   9.1 1.46
## x15      16 5 1.46 0.75 0.8  2.5   1.7 0.33
## x16      17 4 3.17 2.95 0.8  7.0   6.2 1.48
## x17      18 6 3.20 2.08 1.0  7.0   6.0 0.85
## x18      19 6 2.22 1.42 0.9  5.0   4.1 0.58
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars n mean   sd min  max range   se
## beroep    1 6  NaN   NA Inf -Inf  -Inf   NA
## x1        2 6 2.60 1.33 1.5  5.1   3.6 0.54
## x2        3 6 2.57 1.04 1.5  4.0   2.5 0.43
## x3        4 6 3.43 1.75 1.2  5.8   4.6 0.72
## x4        5 6 3.47 2.33 0.6  7.1   6.5 0.95
## x5        6 5 3.72 1.63 1.5  5.5   4.0 0.73
## x6        7 6 2.80 2.01 0.0  5.0   5.0 0.82
## x7        8 6 3.65 1.42 1.8  5.0   3.2 0.58
## x8        9 6 2.97 1.18 1.3  4.0   2.7 0.48
## x9       10 6 3.87 1.80 1.4  6.5   5.1 0.73
## x10      11 6 2.85 1.83 0.8  6.1   5.3 0.75
## x11      12 4 4.05 1.25 2.3  5.0   2.7 0.63
## x12      13 6 3.82 2.01 1.0  7.0   6.0 0.82
## x13      14 6 2.55 2.26 0.1  6.3   6.2 0.92
## x14      15 6 1.75 1.41 0.2  4.3   4.1 0.58
## x15      16 5 1.58 1.09 0.1  3.0   2.9 0.49
## x16      17 6 2.00 2.12 0.0  5.6   5.6 0.86
## x17      18 5 2.80 3.11 0.2  8.2   8.0 1.39
## x18      19 6 1.95 1.60 0.0  3.9   3.9 0.65
## ------------------------------------------------------------ 
## beroep: Other
##        vars n mean   sd min  max range   se
## beroep    1 8  NaN   NA Inf -Inf  -Inf   NA
## x1        2 7 2.90 1.54 1.0  5.0   4.0 0.58
## x2        3 6 2.75 1.60 1.0  5.0   4.0 0.66
## x3        4 4 3.48 2.09 1.0  6.0   5.0 1.05
## x4        5 7 3.70 2.11 1.0  7.1   6.1 0.80
## x5        6 7 3.57 2.64 0.0  8.0   8.0 1.00
## x6        7 8 2.98 1.59 1.0  5.0   4.0 0.56
## x7        8 8 3.85 1.88 0.1  6.0   5.9 0.66
## x8        9 6 4.73 2.76 0.1  8.0   7.9 1.13
## x9       10 4 3.55 2.50 0.1  6.1   6.0 1.25
## x10      11 7 5.16 3.21 0.1 10.0   9.9 1.21
## x11      12 7 3.01 2.53 0.0  7.0   7.0 0.96
## x12      13 8 3.48 1.68 0.1  5.0   4.9 0.59
## x13      14 8 3.31 2.05 1.0  6.0   5.0 0.73
## x14      15 5 2.78 2.50 1.0  7.0   6.0 1.12
## x15      16 3 2.67 2.08 1.0  5.0   4.0 1.20
## x16      17 6 3.70 2.72 1.0  8.2   7.2 1.11
## x17      18 7 3.70 2.21 1.0  7.0   6.0 0.84
## x18      19 8 3.08 1.77 1.0  5.0   4.0 0.63
```

</div>
<script>
document.getElementById("showButtonbe").addEventListener("click", function() {
  var contentContainerbe = document.getElementById("contentContainerbe");
  if (contentContainerbe.style.display === "none") {
    contentContainerbe.style.display = "block";
  } else {
    contentContainerbe.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 1

<button id="showButtonbe1">Show Graph Scen 1/BE</button>

<div id="graphContainerbe1" style="display:none;">

```r
dat_scen1_BE[is.na(dat_scen1_BE)] <- 12
dat_scen1_BE <- dat_scen1_BE %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_scen1_BE <- dat_scen1_BE %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_scen1_BE <- dat_scen1_BE %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_scen1_BE$group1  = factor(dat_scen1_BE$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_BE$group2  = factor(dat_scen1_BE$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_BE$group3  = factor(dat_scen1_BE$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))

scen1_be <- plot_trajectories(data = dat_scen1_BE,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_be
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbe1").addEventListener("click", function() {
  var graphContainerbe1 = document.getElementById("graphContainerbe1");
  if (graphContainerbe1.style.display === "none") {
    graphContainerbe1.style.display = "block";
  } else {
    graphContainerbe1.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 2

<button id="showButtonbe2">Show Graph Scen 2/BE</button>

<div id="graphContainerbe2" style="display:none;">

```r
scen2_be <- plot_trajectories(data = dat_scen1_BE,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_be
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbe2").addEventListener("click", function() {
  var graphContainerbe2 = document.getElementById("graphContainerbe2");
  if (graphContainerbe2.style.display === "none") {
    graphContainerbe2.style.display = "block";
  } else {
    graphContainerbe2.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 3

<button id="showButtonbe3">Show Graph Scen 3/BE</button>

<div id="graphContainerbe3" style="display:none;">

```r
scen3_be <- plot_trajectories(data = dat_scen1_BE,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_be
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbe3").addEventListener("click", function() {
  var graphContainerbe3 = document.getElementById("graphContainerbe3");
  if (graphContainerbe3.style.display === "none") {
    graphContainerbe3.style.display = "block";
  } else {
    graphContainerbe3.style.display = "none";
  }
});
</script>


## USA/ N = 99
<button id="showButtonUSA">USA Summary Statistics</button>

<div id="contentContainerUSA" style="display:none;">


```r
dat_scen1_USA <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_scen1_USA <- as.data.frame(dat_scen1_USA)
setnames(dat_scen1_USA, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_scen1_USA, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_scen1_USA <- subset(dat_scen1_USA,dat_scen1_USA$Progress==100)
dat_scen1_USA$participants <- seq(1,295,1)
names(dat_scen1_USA)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_scen1_USA <- dat_scen1_USA %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))

dat_scen1_USA <- dat_scen1_USA %>% 
  mutate(country = case_when(`List of Countries...28` == "Netherlands" ~ "NL",
                      `List of Countries...28`  == "Belgium" ~ "BE",
                      `List of Countries...28`  == "United States of America" ~ "USA",
                      TRUE ~ "Other"))

dat_scen1_USA$beroep = factor(dat_scen1_USA$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

dat_scen1_USA <- subset(dat_scen1_USA, dat_scen1_USA$country=="USA")


dat_table_USA <- subset(dat_scen1_USA, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_USA, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars  n mean   sd min  max range   se
## beroep    1 24  NaN   NA Inf -Inf  -Inf   NA
## x1        2 22 4.92 2.34 0.0  9.4   9.4 0.50
## x2        3 19 5.06 2.08 2.2  8.5   6.3 0.48
## x3        4 21 4.84 2.09 1.4 10.0   8.6 0.46
## x4        5 19 5.33 2.27 2.1  8.7   6.6 0.52
## x5        6 23 4.78 1.82 2.0  8.3   6.3 0.38
## x6        7 18 5.57 1.80 3.0  8.9   5.9 0.42
## x7        8 22 5.95 2.19 2.8 10.0   7.2 0.47
## x8        9 21 5.63 2.22 2.0  9.8   7.8 0.49
## x9       10 21 4.78 1.87 2.0  9.0   7.0 0.41
## x10      11 21 4.78 2.11 1.6  8.3   6.7 0.46
## x11      12 22 4.55 1.44 2.2  7.3   5.1 0.31
## x12      13 22 4.96 1.94 2.7 10.0   7.3 0.41
## x13      14 20 5.80 2.30 2.2 10.0   7.8 0.51
## x14      15 21 5.25 1.74 3.1  8.8   5.7 0.38
## x15      16 21 5.22 2.12 2.2  8.7   6.5 0.46
## x16      17 19 5.22 2.23 2.2  9.0   6.8 0.51
## x17      18 21 5.16 1.83 2.2 10.0   7.8 0.40
## x18      19 23 4.97 2.24 1.8  9.8   8.0 0.47
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars  n mean   sd min  max range   se
## beroep    1 12  NaN   NA Inf -Inf  -Inf   NA
## x1        2 12 4.97 1.88 2.4  8.9   6.5 0.54
## x2        3 12 6.53 2.70 1.8 10.0   8.2 0.78
## x3        4 12 5.24 1.81 1.5  8.3   6.8 0.52
## x4        5 12 5.70 2.68 2.0 10.0   8.0 0.77
## x5        6 12 4.76 1.42 1.9  6.6   4.7 0.41
## x6        7 12 5.77 1.47 3.1  8.3   5.2 0.42
## x7        8 12 5.68 2.37 2.5  9.7   7.2 0.69
## x8        9 12 6.12 1.65 3.5  9.5   6.0 0.48
## x9       10 12 5.51 1.99 1.7  7.7   6.0 0.57
## x10      11 12 5.38 2.24 1.4  8.9   7.5 0.65
## x11      12 12 4.93 2.30 2.0  8.8   6.8 0.66
## x12      13 12 5.57 2.29 2.3  9.7   7.4 0.66
## x13      14 12 4.88 2.37 1.5  9.5   8.0 0.68
## x14      15 12 5.66 2.42 2.0  9.7   7.7 0.70
## x15      16 12 4.73 2.66 1.3  8.5   7.2 0.77
## x16      17 12 5.37 1.99 2.0  8.1   6.1 0.57
## x17      18 12 5.02 1.78 2.9  8.2   5.3 0.51
## x18      19 12 6.92 2.18 4.0 10.0   6.0 0.63
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 37  NaN   NA Inf -Inf  -Inf   NA
## x1        2 33 5.19 2.07 2.4  9.1   6.7 0.36
## x2        3 34 5.15 2.39 0.8  9.5   8.7 0.41
## x3        4 34 4.46 1.75 0.8  8.3   7.5 0.30
## x4        5 33 4.62 2.13 1.3  8.5   7.2 0.37
## x5        6 33 5.32 2.34 0.8 10.0   9.2 0.41
## x6        7 34 5.71 2.51 1.2 10.0   8.8 0.43
## x7        8 34 5.46 1.83 1.0 10.0   9.0 0.31
## x8        9 34 5.05 2.36 0.3 10.0   9.7 0.40
## x9       10 36 5.25 2.16 0.3 10.0   9.7 0.36
## x10      11 34 4.41 1.69 0.3  8.3   8.0 0.29
## x11      12 34 4.86 2.11 0.3  9.0   8.7 0.36
## x12      13 34 5.01 1.91 0.7 10.0   9.3 0.33
## x13      14 34 5.76 2.14 1.1  8.9   7.8 0.37
## x14      15 34 5.44 2.08 0.8  9.8   9.0 0.36
## x15      16 32 5.40 2.36 0.2 10.0   9.8 0.42
## x16      17 35 5.27 2.53 1.0 10.0   9.0 0.43
## x17      18 35 4.64 2.17 0.1  8.3   8.2 0.37
## x18      19 34 5.28 2.23 1.1 10.0   8.9 0.38
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars  n mean   sd min  max range   se
## beroep    1 26  NaN   NA Inf -Inf  -Inf   NA
## x1        2 25 3.98 1.91 1.0  9.1   8.1 0.38
## x2        3 22 4.70 1.98 2.6 10.0   7.4 0.42
## x3        4 25 3.78 1.34 1.6  7.2   5.6 0.27
## x4        5 25 4.04 1.70 1.5  8.2   6.7 0.34
## x5        6 25 3.62 1.50 1.1  6.5   5.4 0.30
## x6        7 23 4.93 2.20 2.4  9.4   7.0 0.46
## x7        8 25 4.71 2.57 1.7 10.0   8.3 0.51
## x8        9 26 4.16 1.48 1.5  7.1   5.6 0.29
## x9       10 24 4.50 1.75 1.7  8.8   7.1 0.36
## x10      11 24 3.62 1.75 1.0  9.4   8.4 0.36
## x11      12 23 3.88 1.43 1.4  6.7   5.3 0.30
## x12      13 23 5.15 1.67 2.4  8.1   5.7 0.35
## x13      14 24 4.26 1.55 0.0  6.9   6.9 0.32
## x14      15 22 4.40 1.62 1.8  9.0   7.2 0.35
## x15      16 23 4.05 1.63 1.3  7.0   5.7 0.34
## x16      17 22 3.79 1.45 1.0  6.8   5.8 0.31
## x17      18 24 4.32 1.39 2.3  8.5   6.2 0.28
## x18      19 23 4.57 1.84 0.2  9.3   9.1 0.38
## ------------------------------------------------------------ 
## beroep: Other
## NULL
```

</div>
<script>
document.getElementById("showButtonUSA").addEventListener("click", function() {
  var contentContainerUSA = document.getElementById("contentContainerUSA");
  if (contentContainerUSA.style.display === "none") {
    contentContainerUSA.style.display = "block";
  } else {
    contentContainerUSA.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 1

<button id="showButtonUSA1">Show Graph Scen 1/USA</button>

<div id="graphContainerUSA1" style="display:none;">

```r
dat_scen1_USA[is.na(dat_scen1_USA)] <- 12
dat_scen1_USA <- dat_scen1_USA %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_scen1_USA <- dat_scen1_USA %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_scen1_USA <- dat_scen1_USA %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_scen1_USA$group1  = factor(dat_scen1_USA$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_USA$group2  = factor(dat_scen1_USA$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_USA$group3  = factor(dat_scen1_USA$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
scen1_USA <- plot_trajectories(data = dat_scen1_USA,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_USA
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSA1").addEventListener("click", function() {
  var graphContainerUSA1 = document.getElementById("graphContainerUSA1");
  if (graphContainerUSA1.style.display === "none") {
    graphContainerUSA1.style.display = "block";
  } else {
    graphContainerUSA1.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 2

<button id="showButtonUSA2">Show Graph Scen 2/USA</button>

<div id="graphContainerUSA2" style="display:none;">

```r
scen2_USA <- plot_trajectories(data = dat_scen1_USA,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_USA
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSA2").addEventListener("click", function() {
  var graphContainerUSA2 = document.getElementById("graphContainerUSA2");
  if (graphContainerUSA2.style.display === "none") {
    graphContainerUSA2.style.display = "block";
  } else {
    graphContainerUSA2.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 3

<button id="showButtonUSA3">Show Graph Scen 3/USA</button>

<div id="graphContainerUSA3" style="display:none;">

```r
scen3_USA <- plot_trajectories(data = dat_scen1_USA,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_USA
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSA3").addEventListener("click", function() {
  var graphContainerUSA3 = document.getElementById("graphContainerUSA3");
  if (graphContainerUSA3.style.display === "none") {
    graphContainerUSA3.style.display = "block";
  } else {
    graphContainerUSA3.style.display = "none";
  }
});
</script>

## Other/ N = 127
<button id="showButtonOT">Other  Summary Statistics</button>

<div id="contentContainerOT" style="display:none;">


```r
dat_scen1_OT <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_scen1_OT <- as.data.frame(dat_scen1_OT)
setnames(dat_scen1_OT, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_scen1_OT, "Wat is uw huidige beroep? - Selected Choice", "beroep")
dat_scen1_OT <- subset(dat_scen1_OT,dat_scen1_OT$Progress==100)
dat_scen1_OT$participants <- seq(1,295,1)
names(dat_scen1_OT)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_scen1_OT <- dat_scen1_OT %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))

dat_scen1_OT <- dat_scen1_OT %>% 
  mutate(country = case_when(`List of Countries...28` == "Netherlands" ~ "NL",
                      `List of Countries...28`  == "Belgium" ~ "BE",
                      `List of Countries...28`  == "United States of America" ~ "USA",
                      TRUE ~ "Other"))

dat_scen1_OT$beroep = factor(dat_scen1_OT$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

dat_scen1_OT <- subset(dat_scen1_OT, dat_scen1_OT$country=="Other")


dat_table_OT <- subset(dat_scen1_OT, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_OT, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars  n mean   sd min  max range   se
## beroep    1 41  NaN   NA Inf -Inf  -Inf   NA
## x1        2 37 5.15 2.24 1.1  9.8   8.7 0.37
## x2        3 38 5.05 2.21 1.1  9.8   8.7 0.36
## x3        4 38 5.36 2.11 1.6  9.3   7.7 0.34
## x4        5 40 5.35 2.18 0.5  8.9   8.4 0.34
## x5        6 41 5.24 2.33 1.0  9.5   8.5 0.36
## x6        7 41 5.69 2.51 0.6  9.9   9.3 0.39
## x7        8 40 5.39 2.37 1.0  9.8   8.8 0.37
## x8        9 40 5.35 2.18 1.0  9.5   8.5 0.34
## x9       10 40 4.98 2.08 0.7  9.1   8.4 0.33
## x10      11 40 5.50 2.24 0.2  9.1   8.9 0.35
## x11      12 40 5.22 2.36 0.9  9.2   8.3 0.37
## x12      13 40 5.47 2.17 1.1 10.0   8.9 0.34
## x13      14 40 5.19 2.36 0.6  9.5   8.9 0.37
## x14      15 40 5.20 2.22 0.9  9.8   8.9 0.35
## x15      16 40 5.21 2.06 2.0  9.1   7.1 0.33
## x16      17 40 5.40 2.38 1.5  9.9   8.4 0.38
## x17      18 40 5.43 2.00 1.4  9.4   8.0 0.32
## x18      19 40 5.49 2.45 1.3 10.0   8.7 0.39
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars  n mean   sd min  max range   se
## beroep    1 14  NaN   NA Inf -Inf  -Inf   NA
## x1        2 14 5.40 2.60 0.9  9.1   8.2 0.69
## x2        3 14 5.06 2.55 1.2  8.3   7.1 0.68
## x3        4 14 5.49 2.62 0.8  9.1   8.3 0.70
## x4        5 14 4.83 2.71 0.7  8.5   7.8 0.73
## x5        6 14 5.29 2.85 0.5  8.6   8.1 0.76
## x6        7 14 5.58 2.62 1.0  9.4   8.4 0.70
## x7        8 14 5.14 2.96 1.0  9.2   8.2 0.79
## x8        9 14 5.29 3.20 0.6  9.5   8.9 0.86
## x9       10 14 5.34 2.74 1.3  8.8   7.5 0.73
## x10      11 14 5.49 2.64 1.6  8.8   7.2 0.71
## x11      12 14 5.11 2.70 1.6  8.9   7.3 0.72
## x12      13 14 5.10 2.77 1.2  8.7   7.5 0.74
## x13      14 14 5.01 2.89 0.9  8.9   8.0 0.77
## x14      15 14 5.26 2.81 1.1  8.8   7.7 0.75
## x15      16 14 5.55 2.83 1.3  9.2   7.9 0.76
## x16      17 14 5.22 3.11 1.2  9.7   8.5 0.83
## x17      18 14 5.53 3.12 1.7  9.8   8.1 0.83
## x18      19 14 5.43 3.17 0.7  9.8   9.1 0.85
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 57  NaN   NA Inf -Inf  -Inf   NA
## x1        2 54 5.99 2.28 1.6 10.0   8.4 0.31
## x2        3 55 6.05 2.44 0.9 10.0   9.1 0.33
## x3        4 54 6.10 2.33 1.4 10.0   8.6 0.32
## x4        5 55 5.97 2.40 1.0 10.0   9.0 0.32
## x5        6 53 5.59 2.32 0.5  9.1   8.6 0.32
## x6        7 55 5.92 2.29 1.0 10.0   9.0 0.31
## x7        8 56 6.08 2.40 0.8 10.0   9.2 0.32
## x8        9 56 6.10 2.29 1.0 10.0   9.0 0.31
## x9       10 56 6.18 2.49 1.3 10.0   8.7 0.33
## x10      11 56 5.84 2.57 0.8 10.0   9.2 0.34
## x11      12 56 6.09 2.40 0.0  9.4   9.4 0.32
## x12      13 56 6.12 2.35 0.7 10.0   9.3 0.31
## x13      14 56 5.69 2.33 1.9 10.0   8.1 0.31
## x14      15 56 6.15 2.10 2.6 10.0   7.4 0.28
## x15      16 56 6.08 2.33 0.0  9.4   9.4 0.31
## x16      17 56 6.18 2.28 1.1  9.2   8.1 0.31
## x17      18 56 5.75 2.41 1.1  9.6   8.5 0.32
## x18      19 56 6.04 2.48 0.9  9.4   8.5 0.33
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars  n mean   sd min  max range   se
## beroep    1 15  NaN   NA Inf -Inf  -Inf   NA
## x1        2 15 5.64 2.27 2.0  8.6   6.6 0.59
## x2        3 15 5.63 2.35 1.8  8.8   7.0 0.61
## x3        4 15 5.48 2.26 2.0  9.1   7.1 0.58
## x4        5 15 5.79 2.36 0.6  8.6   8.0 0.61
## x5        6 15 5.91 2.28 2.4  9.1   6.7 0.59
## x6        7 14 5.96 2.00 3.1  9.1   6.0 0.53
## x7        8 15 5.46 2.89 0.5  9.8   9.3 0.75
## x8        9 15 5.55 2.69 0.5  9.4   8.9 0.69
## x9       10 15 5.49 3.11 0.5 10.0   9.5 0.80
## x10      11 15 5.21 2.82 0.4  9.0   8.6 0.73
## x11      12 15 5.39 2.78 1.5  9.1   7.6 0.72
## x12      13 15 5.89 2.44 2.6  9.8   7.2 0.63
## x13      14 15 5.46 2.50 1.4  8.9   7.5 0.64
## x14      15 15 5.96 2.86 0.5  9.5   9.0 0.74
## x15      16 14 5.11 2.87 0.4  9.1   8.7 0.77
## x16      17 15 6.09 2.02 1.5  8.5   7.0 0.52
## x17      18 14 5.42 2.57 0.4  8.8   8.4 0.69
## x18      19 15 6.05 2.33 2.4  9.7   7.3 0.60
## ------------------------------------------------------------ 
## beroep: Other
## NULL
```

</div>
<script>
document.getElementById("showButtonOT").addEventListener("click", function() {
  var contentContainerOT = document.getElementById("contentContainerOT");
  if (contentContainerOT.style.display === "none") {
    contentContainerOT.style.display = "block";
  } else {
    contentContainerOT.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other Countries- Scen 1

<button id="showButtonOT1">Show Graph Scen 1/Other</button>

<div id="graphContainerOT1" style="display:none;">

```r
dat_scen1_OT[is.na(dat_scen1_OT)] <- 12
dat_scen1_OT <- dat_scen1_OT %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_scen1_OT <- dat_scen1_OT %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_scen1_OT <- dat_scen1_OT %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_scen1_OT$group1  = factor(dat_scen1_OT$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_OT$group2  = factor(dat_scen1_OT$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_scen1_OT$group3  = factor(dat_scen1_OT$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
scen1_OT <- plot_trajectories(data = dat_scen1_OT,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_OT
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonOT1").addEventListener("click", function() {
  var graphContainerOT1 = document.getElementById("graphContainerOT1");
  if (graphContainerOT1.style.display === "none") {
    graphContainerOT1.style.display = "block";
  } else {
    graphContainerOT1.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other Countries - Scen 2

<button id="showButtonOT2">Show Graph Scen 2/Other</button>

<div id="graphContainerOT2" style="display:none;">

```r
scen2_OT <- plot_trajectories(data = dat_scen1_OT,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_OT
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-23-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonOT2").addEventListener("click", function() {
  var graphContainerOT2 = document.getElementById("graphContainerOT2");
  if (graphContainerOT2.style.display === "none") {
    graphContainerOT2.style.display = "block";
  } else {
    graphContainerOT2.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other Countries - Scen 3

<button id="showButtonOT3">Show Graph Scen 3/Other</button>

<div id="graphContainerOT3" style="display:none;">

```r
scen3_OT <- plot_trajectories(data = dat_scen1_OT,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_OT
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonOT3").addEventListener("click", function() {
  var graphContainerOT3 = document.getElementById("graphContainerOT3");
  if (graphContainerOT3.style.display === "none") {
    graphContainerOT3.style.display = "block";
  } else {
    graphContainerOT3.style.display = "none";
  }
});
</script>


# Data Analyses with strict exclusion criteria
  - Upon visual inspection of the data, some data points seem off (e.g., could be bots) 
    + Many participants from the United States (survey was in dutch)
    + Some participants with more years of practice than age/years of practice improbable
    + Some participants completed the study very fast.
    
Hence, analyses are run with more strict exclusion criteria to see whether similar pattern of resuts remain.
  
## Exclusion criteria
  - Participants who's years of experience indicated that they started their legal career before the age of 18.
  - Participants who indicated to currently be a "judge" but have less than 5 years experience in the legal system.
  - Participants who took less than 300 seconds (5min) to complete survey (decision arbitrary but judged to be too fast).
  
<button id="showButtonEX">Summary Statistics Exluded by Country</button>

<div id="contentContainerEX" style="display:none;">

```r
dat_exclusion <- read_xlsx("Raw_data.xlsx",  skip = 1)
dat_exclusion <- as.data.frame(dat_exclusion)
setnames(dat_exclusion, "Wat is de achtergrond van uw huidige beroep? - Selected Choice", "background")
setnames(dat_exclusion, "Wat is uw huidige beroep? - Selected Choice", "beroep")
names(dat_exclusion)[29:46] <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9","x10","x11","x12","x13","x14","x15","x16","x17","x18")
dat_exclusion <- dat_exclusion %>% 
  mutate(beroep = case_when(beroep == "Rechter"  ~ "Judge",
                            beroep == "(substituut) Procureur des konings/officier van justitie"  ~ "Prosecutor",
                            beroep == "Advocaat"  ~ "Lawyer",
                            beroep == "Jurist"  ~ "Parelegal",
                            beroep == "Anders (graag specificeren):\n"  ~ "Other"))
dat_exclusion <- subset(dat_exclusion,dat_exclusion$Progress==100)
dat_exclusion$participants <- seq(1,295,1)
dat_exclusion[is.na(dat_exclusion)] <- 12
#Exclusions strict
dat_exclusion <- subset(dat_exclusion, dat_exclusion$`Leeftijd (in jaren)`- dat_exclusion$`Hoeveel jaren werkt u ongeveer al in het rechtssysteem?` > 18)
dat_exclusion <- subset(dat_exclusion,!(dat_exclusion$`Hoeveel jaren werkt u ongeveer al in het rechtssysteem?` < 5 & dat_exclusion$beroep=="Judge"))
dat_exclusion <- subset(dat_exclusion, dat_exclusion$`Duration (in seconds)` > 300)


dat_exclusion <- dat_exclusion %>% 
  mutate(group1 = case_when(x1 < 2.5 ~ "Low",
                         x1 >=2.5 & x1 <5.0 ~ "Medium-low",
                         x1 >=5.0 & x1 <7.5 ~ "Medium-high",
                         x1 >= 7.5 & x1 <=10~ "High",
                         x1 > 10 ~ "No-decision"))
dat_exclusion <- dat_exclusion %>% 
  mutate(group2 = case_when(x7 < 2.5 ~ "Low",
                         x7 >=2.5 & x7 <5.0 ~ "Medium-low",
                         x7 >=5.0 & x7 <7.5 ~ "Medium-high",
                         x7 >= 7.5 & x7 <=10~ "High",
                         x7 > 10 ~ "No-decision"))
dat_exclusion <- dat_exclusion %>% 
  mutate(group3 = case_when(x13 < 2.5 ~ "Low",
                         x13 >=2.5 & x13 <5.0 ~ "Medium-low",
                         x13 >=5.0 & x13 <7.5 ~ "Medium-high",
                         x13 >= 7.5 & x13 <=10~ "High",
                         x13 > 10 ~ "No-decision"))
dat_exclusion <- dat_exclusion %>% 
  mutate(country = case_when(`List of Countries...28` == "Netherlands" ~ "NL",
                      `List of Countries...28`  == "Belgium" ~ "BE",
                      `List of Countries...28`  == "United States of America" ~ "USA",
                      TRUE ~ "Other"))


dat_exclusion$group1  = factor(dat_exclusion$group1, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_exclusion$group2  = factor(dat_exclusion$group2, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))
dat_exclusion$group3  = factor(dat_exclusion$group3, levels=c("Low", "Medium-low", "Medium-high", "High", "No-decision"))

dat_exclusion$beroep  = factor(dat_exclusion$beroep, levels=c("Judge", "Prosecutor", "Lawyer", "Parelegal", "Other"))

dat_table_EX <- subset(dat_exclusion, select = c(beroep, country, x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_EX, group="country", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## country: BE
##         vars  n mean   sd min  max range   se
## beroep     1 23  NaN   NA Inf -Inf  -Inf   NA
## country    2 23  NaN   NA Inf -Inf  -Inf   NA
## x1         3 23 3.57 2.73 0.7   12  11.3 0.57
## x2         4 23 4.37 3.88 0.8   12  11.2 0.81
## x3         5 23 5.48 4.07 0.8   12  11.2 0.85
## x4         6 23 4.80 4.02 0.6   12  11.4 0.84
## x5         7 23 4.88 3.75 0.8   12  11.2 0.78
## x6         8 23 3.67 3.23 0.0   12  12.0 0.67
## x7         9 23 5.05 3.21 0.1   12  11.9 0.67
## x8        10 23 6.26 4.07 0.1   12  11.9 0.85
## x9        11 23 6.37 3.91 0.1   12  11.9 0.81
## x10       12 23 5.95 4.09 0.1   12  11.9 0.85
## x11       13 23 5.99 4.05 0.0   12  12.0 0.84
## x12       14 23 5.37 3.62 0.1   12  11.9 0.75
## x13       15 23 3.88 3.42 0.7   12  11.3 0.71
## x14       16 23 5.57 4.78 0.8   12  11.2 1.00
## x15       17 23 6.23 4.94 0.8   12  11.2 1.03
## x16       18 23 5.37 4.38 0.0   12  12.0 0.91
## x17       19 23 5.43 4.27 0.8   12  11.2 0.89
## x18       20 23 3.92 3.59 0.0   12  12.0 0.75
## ------------------------------------------------------------ 
## country: NL
##         vars  n mean   sd min  max range   se
## beroep     1 39  NaN   NA Inf -Inf  -Inf   NA
## country    2 39  NaN   NA Inf -Inf  -Inf   NA
## x1         3 39 4.06 3.56 0.6   12  11.4 0.57
## x2         4 39 6.20 4.35 0.7   12  11.3 0.70
## x3         5 39 5.34 4.27 0.0   12  12.0 0.68
## x4         6 39 6.47 4.18 0.7   12  11.3 0.67
## x5         7 39 6.48 4.84 0.5   12  11.5 0.77
## x6         8 39 5.27 4.02 0.7   12  11.3 0.64
## x7         9 39 4.10 3.22 0.0   12  12.0 0.52
## x8        10 39 5.73 4.38 0.0   12  12.0 0.70
## x9        11 39 5.87 4.14 0.0   12  12.0 0.66
## x10       12 39 7.07 3.98 0.0   12  12.0 0.64
## x11       13 39 6.60 4.58 0.0   12  12.0 0.73
## x12       14 39 4.97 3.63 0.0   12  12.0 0.58
## x13       15 39 3.03 3.20 0.5   12  11.5 0.51
## x14       16 39 4.79 4.60 0.6   12  11.4 0.74
## x15       17 39 5.33 4.84 0.7   12  11.3 0.77
## x16       18 39 5.71 4.42 0.5   12  11.5 0.71
## x17       19 39 4.59 4.22 0.7   12  11.3 0.68
## x18       20 39 3.47 3.01 0.7   12  11.3 0.48
## ------------------------------------------------------------ 
## country: Other
##         vars  n mean   sd min  max range   se
## beroep     1 73  NaN   NA Inf -Inf  -Inf   NA
## country    2 73  NaN   NA Inf -Inf  -Inf   NA
## x1         3 73 6.14 2.38 0.9 12.0  11.1 0.28
## x2         4 73 6.10 2.50 1.0 12.0  11.0 0.29
## x3         5 73 6.30 2.46 0.8 12.0  11.2 0.29
## x4         6 73 6.07 2.63 0.5 12.0  11.5 0.31
## x5         7 73 6.03 2.62 0.5 12.0  11.5 0.31
## x6         8 73 6.20 2.50 0.6 12.0  11.4 0.29
## x7         9 73 6.10 2.61 0.5 12.0  11.5 0.31
## x8        10 73 5.95 2.56 0.5 10.0   9.5 0.30
## x9        11 73 6.08 2.63 0.5 10.0   9.5 0.31
## x10       12 73 5.90 2.57 0.4 10.0   9.6 0.30
## x11       13 73 5.89 2.67 0.0  9.4   9.4 0.31
## x12       14 73 6.05 2.40 0.7 10.0   9.3 0.28
## x13       15 73 5.77 2.44 1.4 10.0   8.6 0.29
## x14       16 73 5.91 2.45 0.5 10.0   9.5 0.29
## x15       17 73 5.98 2.49 0.0  9.4   9.4 0.29
## x16       18 73 6.14 2.34 1.1  9.7   8.6 0.27
## x17       19 73 5.82 2.45 0.4  9.6   9.2 0.29
## x18       20 73 6.07 2.46 0.7  9.7   9.0 0.29
## ------------------------------------------------------------ 
## country: USA
##         vars  n mean   sd min  max range   se
## beroep     1 76  NaN   NA Inf -Inf  -Inf   NA
## country    2 76  NaN   NA Inf -Inf  -Inf   NA
## x1         3 76 5.13 2.89 0.0   12  12.0 0.33
## x2         4 76 5.99 3.26 1.1   12  10.9 0.37
## x3         5 76 4.84 2.69 0.8   12  11.2 0.31
## x4         6 76 5.16 2.98 1.3   12  10.7 0.34
## x5         7 76 4.71 2.50 0.8   12  11.2 0.29
## x6         8 76 6.17 2.94 1.8   12  10.2 0.34
## x7         9 76 5.81 2.77 1.0   12  11.0 0.32
## x8        10 76 5.26 2.57 0.3   12  11.7 0.29
## x9        11 76 5.24 2.55 0.3   12  11.7 0.29
## x10       12 76 4.74 2.69 0.3   12  11.7 0.31
## x11       13 76 4.88 2.72 0.3   12  11.7 0.31
## x12       14 76 5.45 2.48 0.7   12  11.3 0.28
## x13       15 76 5.56 2.85 0.0   12  12.0 0.33
## x14       16 76 5.82 3.04 0.8   12  11.2 0.35
## x15       17 76 5.63 3.24 0.2   12  11.8 0.37
## x16       18 76 5.35 3.19 1.0   12  11.0 0.37
## x17       19 76 5.15 2.77 0.1   12  11.9 0.32
## x18       20 76 5.70 2.98 0.2   12  11.8 0.34
```
</div>
<script>
document.getElementById("showButtonEX").addEventListener("click", function() {
  var contentContainerEX = document.getElementById("contentContainerEX");
  if (contentContainerEX.style.display === "none") {
    contentContainerEX.style.display = "block";
  } else {
    contentContainerEX.style.display = "none";
  }
});
</script>


<button id="showButtonEXoc">Summary Statistics Excluded by Occupation</button>

<div id="contentContainerEXoc" style="display:none;">

```r
dat_table_EX <- subset(dat_exclusion, select = c(beroep, country, x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_EX, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##         vars  n mean   sd min  max range   se
## beroep     1 25  NaN   NA Inf -Inf  -Inf   NA
## country    2 25  NaN   NA Inf -Inf  -Inf   NA
## x1         3 25 4.80 2.77 0.0 12.0  12.0 0.55
## x2         4 25 5.52 3.43 1.5 12.0  10.5 0.69
## x3         5 25 5.20 2.85 1.4 12.0  10.6 0.57
## x4         6 25 5.61 3.30 0.5 12.0  11.5 0.66
## x5         7 25 4.22 1.85 1.6  8.3   6.7 0.37
## x6         8 25 5.78 3.29 0.6 12.0  11.4 0.66
## x7         9 25 5.44 2.77 1.8 12.0  10.2 0.55
## x8        10 25 5.32 2.91 1.0 12.0  11.0 0.58
## x9        11 25 5.00 2.96 0.7 12.0  11.3 0.59
## x10       12 25 4.58 2.71 1.2 12.0  10.8 0.54
## x11       13 25 4.52 2.55 1.1 12.0  10.9 0.51
## x12       14 25 4.80 2.24 2.0 12.0  10.0 0.45
## x13       15 25 5.32 3.04 1.0 12.0  11.0 0.61
## x14       16 25 5.33 3.19 1.0 12.0  11.0 0.64
## x15       17 25 5.71 3.22 1.0 12.0  11.0 0.64
## x16       18 25 5.60 3.51 1.6 12.0  10.4 0.70
## x17       19 25 5.46 3.03 1.0 12.0  11.0 0.61
## x18       20 25 4.53 2.64 1.0 12.0  11.0 0.53
## ------------------------------------------------------------ 
## beroep: Prosecutor
##         vars  n mean   sd min  max range   se
## beroep     1 23  NaN   NA Inf -Inf  -Inf   NA
## country    2 23  NaN   NA Inf -Inf  -Inf   NA
## x1         3 23 5.27 2.72 0.8   12  11.2 0.57
## x2         4 23 6.27 3.36 0.8   12  11.2 0.70
## x3         5 23 5.78 3.00 0.8   12  11.2 0.63
## x4         6 23 6.01 3.45 0.7   12  11.3 0.72
## x5         7 23 5.05 2.65 0.5   12  11.5 0.55
## x6         8 23 5.61 2.69 1.0   12  11.0 0.56
## x7         9 23 5.69 2.82 1.0   12  11.0 0.59
## x8        10 23 6.45 3.08 0.6   12  11.4 0.64
## x9        11 23 6.11 3.09 1.3   12  10.7 0.64
## x10       12 23 6.00 3.16 1.4   12  10.6 0.66
## x11       13 23 5.20 2.82 1.6   12  10.4 0.59
## x12       14 23 5.80 2.97 1.3   12  10.7 0.62
## x13       15 23 5.05 2.82 0.8   12  11.2 0.59
## x14       16 23 6.31 3.37 1.1   12  10.9 0.70
## x15       17 23 6.11 3.42 1.3   12  10.7 0.71
## x16       18 23 5.97 3.20 1.5   12  10.5 0.67
## x17       19 23 5.54 2.86 0.8   12  11.2 0.60
## x18       20 23 6.09 3.11 0.7   12  11.3 0.65
## ------------------------------------------------------------ 
## beroep: Lawyer
##         vars   n mean   sd min  max range   se
## beroep     1 115  NaN   NA Inf -Inf  -Inf   NA
## country    2 115  NaN   NA Inf -Inf  -Inf   NA
## x1         3 115 5.54 3.22 0.6   12  11.4 0.30
## x2         4 115 6.09 3.38 0.7   12  11.3 0.32
## x3         5 115 5.75 3.34 0.0   12  12.0 0.31
## x4         6 115 6.08 3.42 0.7   12  11.3 0.32
## x5         7 115 6.06 3.62 0.5   12  11.5 0.34
## x6         8 115 5.98 3.23 0.7   12  11.3 0.30
## x7         9 115 5.67 3.05 0.0   12  12.0 0.28
## x8        10 115 5.86 3.30 0.0   12  12.0 0.31
## x9        11 115 5.93 3.06 0.0   12  12.0 0.29
## x10       12 115 6.23 3.20 0.0   12  12.0 0.30
## x11       13 115 6.05 3.46 0.0   12  12.0 0.32
## x12       14 115 5.66 2.98 0.0   12  12.0 0.28
## x13       15 115 5.05 3.22 0.5   12  11.5 0.30
## x14       16 115 5.58 3.41 0.6   12  11.4 0.32
## x15       17 115 5.82 3.62 0.0   12  12.0 0.34
## x16       18 115 5.89 3.29 0.5   12  11.5 0.31
## x17       19 115 5.34 3.38 0.1   12  11.9 0.32
## x18       20 115 5.32 3.17 0.7   12  11.3 0.30
## ------------------------------------------------------------ 
## beroep: Parelegal
##         vars  n mean   sd min  max range   se
## beroep     1 39  NaN   NA Inf -Inf  -Inf   NA
## country    2 39  NaN   NA Inf -Inf  -Inf   NA
## x1         3 39 4.47 2.43 1.0 12.0  11.0 0.39
## x2         4 39 5.57 3.02 1.6 12.0  10.4 0.48
## x3         5 39 4.47 2.22 1.2 12.0  10.8 0.36
## x4         6 39 4.61 2.45 0.6 12.0  11.4 0.39
## x5         7 39 4.69 2.69 1.1 12.0  10.9 0.43
## x6         8 39 5.78 3.08 0.0 12.0  12.0 0.49
## x7         9 39 5.07 2.77 0.5 12.0  11.5 0.44
## x8        10 39 4.46 2.00 0.5  9.4   8.9 0.32
## x9        11 39 5.22 2.78 0.5 12.0  11.5 0.44
## x10       12 39 4.42 2.80 0.4 12.0  11.6 0.45
## x11       13 39 5.41 3.18 1.4 12.0  10.6 0.51
## x12       14 39 5.88 2.59 2.4 12.0   9.6 0.41
## x13       15 39 4.89 2.69 0.0 12.0  12.0 0.43
## x14       16 39 5.34 3.22 0.5 12.0  11.5 0.52
## x15       17 39 4.97 3.23 0.4 12.0  11.6 0.52
## x16       18 39 4.90 3.11 0.0 12.0  12.0 0.50
## x17       19 39 5.12 2.84 0.4 12.0  11.6 0.45
## x18       20 39 5.32 2.92 0.0 12.0  12.0 0.47
## ------------------------------------------------------------ 
## beroep: Other
##         vars n mean   sd min  max range   se
## beroep     1 9  NaN   NA Inf -Inf  -Inf   NA
## country    2 9  NaN   NA Inf -Inf  -Inf   NA
## x1         3 9 2.88 1.39 1.0  5.0   4.0 0.46
## x2         4 9 4.72 4.32 1.0 12.0  11.0 1.44
## x3         5 9 7.10 4.84 1.0 12.0  11.0 1.61
## x4         6 9 4.60 3.49 1.0 12.0  11.0 1.16
## x5         7 9 6.78 4.32 2.0 12.0  10.0 1.44
## x6         8 9 2.77 1.64 1.0  5.0   4.0 0.55
## x7         9 9 5.12 3.16 0.1 12.0  11.9 1.05
## x8        10 9 8.00 4.38 0.1 12.0  11.9 1.46
## x9        11 9 7.38 4.65 0.1 12.0  11.9 1.55
## x10       12 9 6.92 4.06 0.1 12.0  11.9 1.35
## x11       13 9 6.34 4.63 0.0 12.0  12.0 1.54
## x12       14 9 4.42 3.25 0.1 12.0  11.9 1.08
## x13       15 9 3.42 2.42 1.0  6.8   5.8 0.81
## x14       16 9 6.79 5.27 1.0 12.0  11.0 1.76
## x15       17 9 7.67 5.27 1.0 12.0  11.0 1.76
## x16       18 9 6.24 4.90 1.0 12.0  11.0 1.63
## x17       19 9 4.66 3.67 1.0 12.0  11.0 1.22
## x18       20 9 3.33 2.42 0.8  7.6   6.8 0.81
```
</div>
<script>
document.getElementById("showButtonEXoc").addEventListener("click", function() {
  var contentContainerEXoc = document.getElementById("contentContainerEXoc");
  if (contentContainerEXoc.style.display === "none") {
    contentContainerEXoc.style.display = "block";
  } else {
    contentContainerEXoc.style.display = "none";
  }
});
</script>


### Individual trace plots - Scen 1 - Strict Exclusion data

<button id="showButtonEX1">Show Graph Scen 1/Other</button>

<div id="graphContainerEX1" style="display:none;">

```r
scen1_EX <- plot_trajectories(data = dat_exclusion,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_EX
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-27-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonEX1").addEventListener("click", function() {
  var graphContainerEX1 = document.getElementById("graphContainerEX1");
  if (graphContainerEX1.style.display === "none") {
    graphContainerEX1.style.display = "block";
  } else {
    graphContainerEX1.style.display = "none";
  }
});
</script>


### Individual trace plots - Scen 2 - Strict Exclusion data

<button id="showButtonEX2">Show Graph Scen 2/Other</button>

<div id="graphContainerEX2" style="display:none;">

```r
scen2_EX <- plot_trajectories(data = dat_exclusion,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_EX
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-28-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonEX2").addEventListener("click", function() {
  var graphContainerEX2 = document.getElementById("graphContainerEX2");
  if (graphContainerEX2.style.display === "none") {
    graphContainerEX2.style.display = "block";
  } else {
    graphContainerEX2.style.display = "none";
  }
});
</script>


### Individual trace plots - Scen 3 - Strict Exclusion data

<button id="showButtonEX3">Show Graph Scen 3/Other</button>

<div id="graphContainerEX3" style="display:none;">

```r
scen3_EX <- plot_trajectories(data = dat_exclusion,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_EX
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-29-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonEX3").addEventListener("click", function() {
  var graphContainerEX3 = document.getElementById("graphContainerEX3");
  if (graphContainerEX3.style.display === "none") {
    graphContainerEX3.style.display = "block";
  } else {
    graphContainerEX3.style.display = "none";
  }
});
</script>


# Data Analysis Data with strict exclusion - Specific Countries

## The Netherlands/ N = 40
<button id="showButtonnlex">Netherlands Summary Statistics</button>

<div id="contentContainernlex" style="display:none;">


```r
dat_exclusion_nl <- dat_exclusion
dat_exclusion_nl <- subset(dat_exclusion_nl, dat_exclusion_nl$country=="NL")

dat_table_ex_nl <- subset(dat_exclusion_nl, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_table_ex_nl, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
## NULL
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars n mean sd  min  max range se
## beroep    1 1  NaN NA  Inf -Inf  -Inf NA
## x1        2 1  6.3 NA  6.3  6.3     0 NA
## x2        3 1 12.0 NA 12.0 12.0     0 NA
## x3        4 1 12.0 NA 12.0 12.0     0 NA
## x4        5 1 12.0 NA 12.0 12.0     0 NA
## x5        6 1  5.2 NA  5.2  5.2     0 NA
## x6        7 1  7.2 NA  7.2  7.2     0 NA
## x7        8 1  7.3 NA  7.3  7.3     0 NA
## x8        9 1 12.0 NA 12.0 12.0     0 NA
## x9       10 1 12.0 NA 12.0 12.0     0 NA
## x10      11 1 12.0 NA 12.0 12.0     0 NA
## x11      12 1  4.2 NA  4.2  4.2     0 NA
## x12      13 1  7.0 NA  7.0  7.0     0 NA
## x13      14 1  6.2 NA  6.2  6.2     0 NA
## x14      15 1 12.0 NA 12.0 12.0     0 NA
## x15      16 1 12.0 NA 12.0 12.0     0 NA
## x16      17 1 12.0 NA 12.0 12.0     0 NA
## x17      18 1  4.2 NA  4.2  4.2     0 NA
## x18      19 1  6.4 NA  6.4  6.4     0 NA
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 36  NaN   NA Inf -Inf  -Inf   NA
## x1        2 36 4.06 3.67 0.6   12  11.4 0.61
## x2        3 36 6.00 4.25 0.7   12  11.3 0.71
## x3        4 36 5.06 4.11 0.0   12  12.0 0.69
## x4        5 36 6.44 4.19 0.7   12  11.3 0.70
## x5        6 36 6.21 4.85 0.5   12  11.5 0.81
## x6        7 36 5.35 4.12 0.7   12  11.3 0.69
## x7        8 36 3.74 3.00 0.0   12  12.0 0.50
## x8        9 36 5.21 4.15 0.0   12  12.0 0.69
## x9       10 36 5.58 4.04 0.0   12  12.0 0.67
## x10      11 36 6.79 3.96 0.0   12  12.0 0.66
## x11      12 36 6.37 4.57 0.0   12  12.0 0.76
## x12      13 36 4.72 3.57 0.0   12  12.0 0.59
## x13      14 36 2.89 3.20 0.5   12  11.5 0.53
## x14      15 36 4.50 4.42 0.6   12  11.4 0.74
## x15      16 36 5.08 4.72 0.7   12  11.3 0.79
## x16      17 36 5.49 4.28 0.5   12  11.5 0.71
## x17      18 36 4.64 4.34 0.7   12  11.3 0.72
## x18      19 36 3.34 2.98 0.7   12  11.3 0.50
## ------------------------------------------------------------ 
## beroep: Parelegal
## NULL
## ------------------------------------------------------------ 
## beroep: Other
##        vars n  mean   sd  min  max range   se
## beroep    1 2   NaN   NA  Inf -Inf  -Inf   NA
## x1        2 2  2.80 1.13  2.0  3.6   1.6 0.80
## x2        3 2  7.00 7.07  2.0 12.0  10.0 5.00
## x3        4 2  7.00 7.07  2.0 12.0  10.0 5.00
## x4        5 2  4.25 3.18  2.0  6.5   4.5 2.25
## x5        6 2 12.00 0.00 12.0 12.0   0.0 0.00
## x6        7 2  3.00 2.83  1.0  5.0   4.0 2.00
## x7        8 2  9.00 4.24  6.0 12.0   6.0 3.00
## x8        9 2 12.00 0.00 12.0 12.0   0.0 0.00
## x9       10 2  8.10 5.52  4.2 12.0   7.8 3.90
## x10      11 2  9.60 3.39  7.2 12.0   4.8 2.40
## x11      12 2 12.00 0.00 12.0 12.0   0.0 0.00
## x12      13 2  8.50 4.95  5.0 12.0   7.0 3.50
## x13      14 2  3.90 4.10  1.0  6.8   5.8 2.90
## x14      15 2  6.55 7.71  1.1 12.0  10.9 5.45
## x15      16 2  6.50 7.78  1.0 12.0  11.0 5.50
## x16      17 2  6.50 7.78  1.0 12.0  11.0 5.50
## x17      18 2  3.95 4.17  1.0  6.9   5.9 2.95
## x18      19 2  4.20 4.81  0.8  7.6   6.8 3.40
```

</div>
<script>
document.getElementById("showButtonnlex").addEventListener("click", function() {
  var contentContainernlex = document.getElementById("contentContainernlex");
  if (contentContainernlex.style.display === "none") {
    contentContainernlex.style.display = "block";
  } else {
    contentContainernlex.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 1

<button id="showButtonnlex1">Show Graph Scen 1/NL</button>

<div id="graphContainernlex1" style="display:none;">

```r
scen1_nl_ex <- plot_trajectories(data = dat_exclusion_nl,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_nl_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-31-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnlex1").addEventListener("click", function() {
  var graphContainernlex1 = document.getElementById("graphContainernlex1");
  if (graphContainernlex1.style.display === "none") {
    graphContainernlex1.style.display = "block";
  } else {
    graphContainernlex1.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 2

<button id="showButtonnlex2">Show Graph Scen 2/NL</button>

<div id="graphContainernlex2" style="display:none;">

```r
scen2_nl_ex <- plot_trajectories(data = dat_exclusion_nl,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_nl_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-32-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnlex2").addEventListener("click", function() {
  var graphContainernlex2 = document.getElementById("graphContainernlex2");
  if (graphContainernlex2.style.display === "none") {
    graphContainernlex2.style.display = "block";
  } else {
    graphContainernlex2.style.display = "none";
  }
});
</script>

### Individual Trace Plot NL - Scen 3

<button id="showButtonnlex3">Show Graph Scen 3/NL</button>

<div id="graphContainernlex3" style="display:none;">

```r
scen3_nl_ex <- plot_trajectories(data = dat_exclusion_nl,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_nl_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-33-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonnlex3").addEventListener("click", function() {
  var graphContainernlex3 = document.getElementById("graphContainernlex3");
  if (graphContainernlex3.style.display === "none") {
    graphContainernlex3.style.display = "block";
  } else {
    graphContainernlex3.style.display = "none";
  }
});
</script>


## Belgium/ N = 23
<button id="showButtonbeex">Belgium Summary Statistics</button>

<div id="contentContainerbeex" style="display:none;">


```r
dat_exclusion_be <- dat_exclusion
dat_exclusion_be <- subset(dat_exclusion_be, dat_exclusion_be$country=="BE")

dat_tab_ex_be <- subset(dat_exclusion_be, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_tab_ex_be, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars n mean sd min  max range se
## beroep    1 1  NaN NA Inf -Inf  -Inf NA
## x1        2 1    2 NA   2    2     0 NA
## x2        3 1    2 NA   2    2     0 NA
## x3        4 1    2 NA   2    2     0 NA
## x4        5 1    2 NA   2    2     0 NA
## x5        6 1    2 NA   2    2     0 NA
## x6        7 1    2 NA   2    2     0 NA
## x7        8 1    2 NA   2    2     0 NA
## x8        9 1    2 NA   2    2     0 NA
## x9       10 1    2 NA   2    2     0 NA
## x10      11 1    2 NA   2    2     0 NA
## x11      12 1    2 NA   2    2     0 NA
## x12      13 1    2 NA   2    2     0 NA
## x13      14 1    1 NA   1    1     0 NA
## x14      15 1    1 NA   1    1     0 NA
## x15      16 1    1 NA   1    1     0 NA
## x16      17 1    2 NA   2    2     0 NA
## x17      18 1    1 NA   1    1     0 NA
## x18      19 1    1 NA   1    1     0 NA
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars n mean   sd min  max range   se
## beroep    1 4  NaN   NA Inf -Inf  -Inf   NA
## x1        2 4 5.70 4.71 0.8   12  11.2 2.36
## x2        3 4 6.20 4.58 0.8   12  11.2 2.29
## x3        4 4 5.65 4.74 0.8   12  11.2 2.37
## x4        5 4 6.77 4.63 0.9   12  11.1 2.32
## x5        6 4 5.45 4.86 0.8   12  11.2 2.43
## x6        7 4 5.20 5.04 1.0   12  11.0 2.52
## x7        8 4 5.95 4.38 1.8   12  10.2 2.19
## x8        9 4 7.80 3.95 3.2   12   8.8 1.98
## x9       10 4 7.75 4.03 3.0   12   9.0 2.02
## x10      11 4 7.65 4.19 2.6   12   9.4 2.10
## x11      12 4 7.03 4.17 2.0   12  10.0 2.09
## x12      13 4 7.22 4.45 3.0   12   9.0 2.22
## x13      14 4 6.22 4.58 0.8   12  11.2 2.29
## x14      15 4 9.55 2.97 6.0   12   6.0 1.48
## x15      16 4 9.03 3.44 6.0   12   6.0 1.72
## x16      17 4 7.45 4.52 1.8   12  10.2 2.26
## x17      18 4 7.20 4.94 0.8   12  11.2 2.47
## x18      19 4 5.40 4.91 0.7   12  11.3 2.45
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars n mean   sd min  max range   se
## beroep    1 6  NaN   NA Inf -Inf  -Inf   NA
## x1        2 6 3.88 3.06 0.7    8   7.3 1.25
## x2        3 6 5.22 5.31 0.8   12  11.2 2.17
## x3        4 6 5.47 4.48 0.8   12  11.2 1.83
## x4        5 6 5.00 5.44 0.7   12  11.3 2.22
## x5        6 6 3.93 3.99 1.8   12  10.2 1.63
## x6        7 6 4.60 4.36 0.9   12  11.1 1.78
## x7        8 6 7.02 4.27 3.0   12   9.0 1.74
## x8        9 6 7.70 4.79 2.3   12   9.7 1.95
## x9       10 6 6.90 4.10 2.7   12   9.3 1.67
## x10      11 6 7.48 4.99 2.0   12  10.0 2.04
## x11      12 6 6.10 4.68 1.8   12  10.2 1.91
## x12      13 6 8.00 4.44 2.9   12   9.1 1.81
## x13      14 6 4.20 4.69 0.7   12  11.3 1.91
## x14      15 6 5.08 5.41 0.9   12  11.1 2.21
## x15      16 6 5.05 5.42 0.8   12  11.2 2.21
## x16      17 6 6.12 5.10 0.8   12  11.2 2.08
## x17      18 6 5.87 4.79 1.8   12  10.2 1.95
## x18      19 6 5.73 5.01 1.8   12  10.2 2.05
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars n mean   sd min  max range   se
## beroep    1 5  NaN   NA Inf -Inf  -Inf   NA
## x1        2 5 2.76 1.42 1.5  5.1   3.6 0.64
## x2        3 5 2.78 1.01 1.6  4.0   2.4 0.45
## x3        4 5 3.76 1.75 1.2  5.8   4.6 0.78
## x4        5 5 3.66 2.55 0.6  7.1   6.5 1.14
## x5        6 5 5.58 3.90 1.5 12.0  10.5 1.74
## x6        7 5 3.02 2.17 0.0  5.0   5.0 0.97
## x7        8 5 4.02 1.23 2.0  5.0   3.0 0.55
## x8        9 5 3.30 0.96 2.2  4.0   1.8 0.43
## x9       10 5 4.36 1.49 2.5  6.5   4.0 0.67
## x10      11 5 3.26 1.71 2.0  6.1   4.1 0.76
## x11      12 5 7.58 4.05 4.0 12.0   8.0 1.81
## x12      13 5 4.38 1.64 3.1  7.0   3.9 0.73
## x13      14 5 3.04 2.14 0.9  6.3   5.4 0.96
## x14      15 5 2.06 1.33 0.8  4.3   3.5 0.60
## x15      16 5 3.96 4.55 1.0 12.0  11.0 2.04
## x16      17 5 2.38 2.13 0.0  5.6   5.6 0.95
## x17      18 5 5.16 4.71 1.6 12.0  10.4 2.11
## x18      19 5 2.30 1.50 0.0  3.9   3.9 0.67
## ------------------------------------------------------------ 
## beroep: Other
##        vars n mean   sd min  max range   se
## beroep    1 7  NaN   NA Inf -Inf  -Inf   NA
## x1        2 7 2.90 1.54 1.0    5   4.0 0.58
## x2        3 7 4.07 3.79 1.0   12  11.0 1.43
## x3        4 7 7.13 4.79 1.0   12  11.0 1.81
## x4        5 7 4.70 3.80 1.0   12  11.0 1.44
## x5        6 7 5.29 3.64 2.0   12  10.0 1.38
## x6        7 7 2.70 1.49 1.0    5   4.0 0.56
## x7        8 7 4.01 1.97 0.1    6   5.9 0.74
## x8        9 7 6.86 4.32 0.1   12  11.9 1.63
## x9       10 7 7.17 4.85 0.1   12  11.9 1.83
## x10      11 7 6.16 4.12 0.1   12  11.9 1.56
## x11      12 7 4.73 3.86 0.0   12  12.0 1.46
## x12      13 7 3.26 1.68 0.1    5   4.9 0.64
## x13      14 7 3.29 2.21 1.0    6   5.0 0.84
## x14      15 7 6.86 5.21 1.0   12  11.0 1.97
## x15      16 7 8.00 5.13 1.0   12  11.0 1.94
## x16      17 7 6.17 4.68 1.0   12  11.0 1.77
## x17      18 7 4.86 3.85 1.0   12  11.0 1.45
## x18      19 7 3.09 1.91 1.0    5   4.0 0.72
```

</div>
<script>
document.getElementById("showButtonbeex").addEventListener("click", function() {
  var contentContainerbeex = document.getElementById("contentContainerbeex");
  if (contentContainerbeex.style.display === "none") {
    contentContainerbeex.style.display = "block";
  } else {
    contentContainerbeex.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 1

<button id="showButtonbeex1">Show Graph Scen 1/BE</button>

<div id="graphContainerbeex1" style="display:none;">

```r
scen1_be_ex <- plot_trajectories(data = dat_exclusion_be,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_be_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-35-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbeex1").addEventListener("click", function() {
  var graphContainerbeex1 = document.getElementById("graphContainerbeex1");
  if (graphContainerbeex1.style.display === "none") {
    graphContainerbeex1.style.display = "block";
  } else {
    graphContainerbeex1.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 2

<button id="showButtonbeex2">Show Graph Scen 2/BE</button>

<div id="graphContainerbeex2" style="display:none;">

```r
scen2_be_ex <- plot_trajectories(data = dat_exclusion_be,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_be_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-36-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbeex2").addEventListener("click", function() {
  var graphContainerbeex2 = document.getElementById("graphContainerbeex2");
  if (graphContainerbeex2.style.display === "none") {
    graphContainerbeex2.style.display = "block";
  } else {
    graphContainerbeex2.style.display = "none";
  }
});
</script>

### Individual Trace Plot BE - Scen 3

<button id="showButtonbeex3">Show Graph Scen 3/BE</button>

<div id="graphContainerbeex3" style="display:none;">

```r
scen3_be_ex <- plot_trajectories(data = dat_exclusion_be,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_be_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-37-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonbeex3").addEventListener("click", function() {
  var graphContainerbeex3 = document.getElementById("graphContainerbeex3");
  if (graphContainerbeex3.style.display === "none") {
    graphContainerbeex3.style.display = "block";
  } else {
    graphContainerbeex3.style.display = "none";
  }
});
</script>


## USA/ N = 76
<button id="showButtonUSAex">USA Summary Statistics</button>

<div id="contentContainerUSAex" style="display:none;">


```r
dat_exclusion_USA <- dat_exclusion
dat_exclusion_USA <- subset(dat_exclusion_USA, dat_exclusion_USA$country=="USA")

dat_tab_ex_USA <- subset(dat_exclusion_USA, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_tab_ex_USA, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars  n mean   sd min  max range   se
## beroep    1 13  NaN   NA Inf -Inf  -Inf   NA
## x1        2 13 5.36 3.54 0.0 12.0  12.0 0.98
## x2        3 13 6.86 4.01 2.2 12.0   9.8 1.11
## x3        4 13 5.54 3.46 1.4 12.0  10.6 0.96
## x4        5 13 6.61 3.75 2.1 12.0   9.9 1.04
## x5        6 13 3.95 1.49 2.0  6.9   4.9 0.41
## x6        7 13 7.02 3.62 3.0 12.0   9.0 1.01
## x7        8 13 6.38 2.91 2.8 12.0   9.2 0.81
## x8        9 13 6.22 3.27 2.0 12.0  10.0 0.91
## x9       10 13 5.74 3.37 2.0 12.0  10.0 0.93
## x10      11 13 4.52 2.98 1.6 12.0  10.4 0.83
## x11      12 13 4.78 2.55 2.2 12.0   9.8 0.71
## x12      13 13 5.15 2.63 2.8 12.0   9.2 0.73
## x13      14 13 6.38 3.29 2.2 12.0   9.8 0.91
## x14      15 13 6.65 3.56 3.1 12.0   8.9 0.99
## x15      16 13 6.94 3.56 2.7 12.0   9.3 0.99
## x16      17 13 6.75 4.10 2.2 12.0   9.8 1.14
## x17      18 13 6.28 3.46 3.0 12.0   9.0 0.96
## x18      19 13 4.96 2.99 1.8 12.0  10.2 0.83
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars  n mean   sd min  max range   se
## beroep    1 11  NaN   NA Inf -Inf  -Inf   NA
## x1        2 11 4.97 1.97 2.4  8.9   6.5 0.60
## x2        3 11 6.41 2.80 1.8 10.0   8.2 0.85
## x3        4 11 5.24 1.89 1.5  8.3   6.8 0.57
## x4        5 11 5.60 2.78 2.0 10.0   8.0 0.84
## x5        6 11 4.75 1.49 1.9  6.6   4.7 0.45
## x6        7 11 5.61 1.43 3.1  8.3   5.2 0.43
## x7        8 11 5.96 2.26 3.4  9.7   6.3 0.68
## x8        9 11 6.21 1.70 3.5  9.5   6.0 0.51
## x9       10 11 5.41 2.05 1.7  7.7   6.0 0.62
## x10      11 11 5.29 2.33 1.4  8.9   7.5 0.70
## x11      12 11 4.58 2.04 2.0  8.5   6.5 0.62
## x12      13 11 5.57 2.40 2.3  9.7   7.4 0.72
## x13      14 11 4.46 1.96 1.5  8.0   6.5 0.59
## x14      15 11 5.43 2.39 2.0  9.7   7.7 0.72
## x15      16 11 4.81 2.78 1.3  8.5   7.2 0.84
## x16      17 11 5.28 2.06 2.0  8.1   6.1 0.62
## x17      18 11 5.12 1.83 2.9  8.2   5.3 0.55
## x18      19 11 7.12 2.18 4.0 10.0   6.0 0.66
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 27  NaN   NA Inf -Inf  -Inf   NA
## x1        2 27 5.85 3.15 2.4   12   9.6 0.61
## x2        3 27 5.45 3.11 1.1   12  10.9 0.60
## x3        4 27 5.02 3.01 0.8   12  11.2 0.58
## x4        5 27 5.03 3.07 1.3   12  10.7 0.59
## x5        6 27 5.73 3.11 0.8   12  11.2 0.60
## x6        7 27 6.28 2.93 1.8   12  10.2 0.56
## x7        8 27 6.20 2.72 1.0   12  11.0 0.52
## x8        9 27 5.40 2.98 0.3   12  11.7 0.57
## x9       10 27 5.01 2.28 0.3   12  11.7 0.44
## x10      11 27 5.03 2.57 0.3   12  11.7 0.50
## x11      12 27 5.06 2.89 0.3   12  11.7 0.56
## x12      13 27 5.01 2.19 0.7   12  11.3 0.42
## x13      14 27 6.23 2.97 1.1   12  10.9 0.57
## x14      15 27 5.75 2.95 0.8   12  11.2 0.57
## x15      16 27 5.87 3.36 0.2   12  11.8 0.65
## x16      17 27 4.93 2.89 1.0   12  11.0 0.56
## x17      18 27 4.78 2.95 0.1   12  11.9 0.57
## x18      19 27 5.64 3.20 1.1   12  10.9 0.62
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars  n mean   sd min  max range   se
## beroep    1 25  NaN   NA Inf -Inf  -Inf   NA
## x1        2 25 4.30 2.49 1.0 12.0  11.0 0.50
## x2        3 25 5.93 3.26 2.6 12.0   9.4 0.65
## x3        4 25 4.12 2.12 1.6 12.0  10.4 0.42
## x4        5 25 4.37 2.33 1.5 12.0  10.5 0.47
## x5        6 25 3.97 2.25 1.1 12.0  10.9 0.45
## x6        7 25 5.86 3.10 2.4 12.0   9.6 0.62
## x7        8 25 5.04 2.94 1.7 12.0  10.3 0.59
## x8        9 25 4.18 1.51 1.5  7.1   5.6 0.30
## x9       10 25 5.16 2.66 1.7 12.0  10.3 0.53
## x10      11 25 4.29 2.88 1.0 12.0  11.0 0.58
## x11      12 25 4.88 3.01 1.4 12.0  10.6 0.60
## x12      13 25 6.04 2.73 2.4 12.0   9.6 0.55
## x13      14 25 4.90 2.62 0.0 12.0  12.0 0.52
## x14      15 25 5.65 3.21 1.8 12.0  10.2 0.64
## x15      16 25 5.04 3.05 1.3 12.0  10.7 0.61
## x16      17 25 5.11 3.35 1.0 12.0  11.0 0.67
## x17      18 25 4.97 2.51 2.3 12.0   9.7 0.50
## x18      19 25 5.52 2.99 0.2 12.0  11.8 0.60
## ------------------------------------------------------------ 
## beroep: Other
## NULL
```

</div>
<script>
document.getElementById("showButtonUSAex").addEventListener("click", function() {
  var contentContainerUSAex = document.getElementById("contentContainerUSAex");
  if (contentContainerUSAex.style.display === "none") {
    contentContainerUSAex.style.display = "block";
  } else {
    contentContainerUSAex.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 1

<button id="showButtonUSAex1">Show Graph Scen 1/USA</button>

<div id="graphContainerUSAex1" style="display:none;">

```r
scen1_USA_ex <- plot_trajectories(data = dat_exclusion_USA,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_USA_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-39-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSAex1").addEventListener("click", function() {
  var graphContainerUSAex1 = document.getElementById("graphContainerUSAex1");
  if (graphContainerUSAex1.style.display === "none") {
    graphContainerUSAex1.style.display = "block";
  } else {
    graphContainerUSAex1.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 2

<button id="showButtonUSAex2">Show Graph Scen 2/USA</button>

<div id="graphContainerUSAex2" style="display:none;">

```r
scen2_USA_ex <- plot_trajectories(data = dat_exclusion_USA,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_USA_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-40-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSAex2").addEventListener("click", function() {
  var graphContainerUSAex2 = document.getElementById("graphContainerUSAex2");
  if (graphContainerUSAex2.style.display === "none") {
    graphContainerUSAex2.style.display = "block";
  } else {
    graphContainerUSAex2.style.display = "none";
  }
});
</script>

### Individual Trace Plot USA - Scen 3

<button id="showButtonUSAex3">Show Graph Scen 3/USA</button>

<div id="graphContainerUSAex3" style="display:none;">

```r
scen3_USA_ex <- plot_trajectories(data = dat_exclusion_USA,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_USA_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-41-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonUSAex3").addEventListener("click", function() {
  var graphContainerUSAex3 = document.getElementById("graphContainerUSAex3");
  if (graphContainerUSAex3.style.display === "none") {
    graphContainerUSAex3.style.display = "block";
  } else {
    graphContainerUSAex3.style.display = "none";
  }
});
</script>

## Other Countries/ N = 73
<button id="showButtonotex">Other countries Summary Statistics</button>

<div id="contentContainerotex" style="display:none;">


```r
dat_exclusion_ot <- dat_exclusion
dat_exclusion_ot <- subset(dat_exclusion_ot, dat_exclusion_ot$country=="Other")

dat_tab_ex_ot <- subset(dat_exclusion_ot, select = c(beroep,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11,x12,x13,x14,x15,x16,x17,x18))

describeBy(dat_tab_ex_ot, group="beroep", fast=TRUE)
```

```
## 
##  Descriptive statistics by group 
## beroep: Judge
##        vars  n mean   sd min  max range   se
## beroep    1 11  NaN   NA Inf -Inf  -Inf   NA
## x1        2 11 4.38 1.44 2.0  6.5   4.5 0.43
## x2        3 11 4.26 1.92 1.5  7.3   5.8 0.58
## x3        4 11 5.08 2.00 1.7  8.2   6.5 0.60
## x4        5 11 4.76 2.42 0.5  8.8   8.3 0.73
## x5        6 11 4.75 2.17 1.6  8.3   6.7 0.65
## x6        7 11 4.67 2.33 0.6  8.4   7.8 0.70
## x7        8 11 4.65 2.29 1.8  8.4   6.6 0.69
## x8        9 11 4.56 2.19 1.0  8.0   7.0 0.66
## x9       10 11 4.41 2.33 0.7  8.5   7.8 0.70
## x10      11 11 4.88 2.50 1.2  8.4   7.2 0.75
## x11      12 11 4.45 2.66 1.1  9.0   7.9 0.80
## x12      13 11 4.65 1.66 2.2  7.4   5.2 0.50
## x13      14 11 4.45 2.26 1.6  8.1   6.5 0.68
## x14      15 11 4.16 1.89 1.9  7.4   5.5 0.57
## x15      16 11 4.69 2.11 2.1  8.6   6.5 0.64
## x16      17 11 4.55 2.28 1.6  7.8   6.2 0.69
## x17      18 11 4.90 2.10 1.9  7.6   5.7 0.63
## x18      19 11 4.34 2.11 1.7  8.1   6.4 0.63
## ------------------------------------------------------------ 
## beroep: Prosecutor
##        vars n mean   sd min  max range   se
## beroep    1 7  NaN   NA Inf -Inf  -Inf   NA
## x1        2 7 5.36 3.01 0.9  8.8   7.9 1.14
## x2        3 7 5.26 3.34 1.2  8.3   7.1 1.26
## x3        4 7 5.83 2.98 0.8  8.3   7.5 1.13
## x4        5 7 5.36 3.58 0.7  8.5   7.8 1.35
## x5        6 7 5.26 3.15 0.5  8.6   8.1 1.19
## x6        7 7 5.61 3.14 1.0  9.0   8.0 1.19
## x7        8 7 4.89 3.12 1.0  8.5   7.5 1.18
## x8        9 7 5.27 3.74 0.6  8.9   8.3 1.41
## x9       10 7 5.43 3.29 1.3  8.4   7.1 1.24
## x10      11 7 5.31 3.09 1.6  8.6   7.0 1.17
## x11      12 7 5.29 3.22 1.6  8.9   7.3 1.22
## x12      13 7 5.17 3.27 1.3  8.7   7.4 1.24
## x13      14 7 5.13 3.23 1.5  8.3   6.8 1.22
## x14      15 7 5.03 3.45 1.1  8.8   7.7 1.31
## x15      16 7 5.66 3.05 1.3  9.2   7.9 1.15
## x16      17 7 5.33 3.39 1.5  9.7   8.2 1.28
## x17      18 7 5.44 3.13 2.0  8.7   6.7 1.18
## x18      19 7 4.81 3.35 0.7  8.2   7.5 1.27
## ------------------------------------------------------------ 
## beroep: Lawyer
##        vars  n mean   sd min  max range   se
## beroep    1 46  NaN   NA Inf -Inf  -Inf   NA
## x1        2 46 6.72 2.34 1.6 12.0  10.4 0.34
## x2        3 46 6.66 2.34 1.0 12.0  11.0 0.35
## x3        4 46 6.76 2.44 1.4 12.0  10.6 0.36
## x4        5 46 6.55 2.48 1.0 12.0  11.0 0.36
## x5        6 46 6.42 2.60 0.5 12.0  11.5 0.38
## x6        7 46 6.48 2.32 1.0 12.0  11.0 0.34
## x7        8 46 6.70 2.40 0.8 12.0  11.2 0.35
## x8        9 46 6.40 2.31 1.0 10.0   9.0 0.34
## x9       10 46 6.62 2.24 1.3 10.0   8.7 0.33
## x10      11 46 6.33 2.41 1.3 10.0   8.7 0.35
## x11      12 46 6.37 2.46 0.0  9.4   9.4 0.36
## x12      13 46 6.47 2.29 0.7 10.0   9.3 0.34
## x13      14 46 6.15 2.21 1.9 10.0   8.1 0.33
## x14      15 46 6.39 2.10 2.6 10.0   7.4 0.31
## x15      16 46 6.46 2.25 0.0  9.4   9.4 0.33
## x16      17 46 6.73 2.02 1.1  9.2   8.1 0.30
## x17      18 46 6.16 2.34 1.6  9.6   8.0 0.35
## x18      19 46 6.61 2.23 0.9  9.4   8.5 0.33
## ------------------------------------------------------------ 
## beroep: Parelegal
##        vars n mean   sd min  max range   se
## beroep    1 9  NaN   NA Inf -Inf  -Inf   NA
## x1        2 9 5.90 2.04 3.5  8.6   5.1 0.68
## x2        3 9 6.12 2.32 1.8  8.8   7.0 0.77
## x3        4 9 5.84 2.37 2.0  9.1   7.1 0.79
## x4        5 9 5.79 2.60 0.6  8.6   8.0 0.87
## x5        6 9 6.22 2.62 2.4  9.1   6.7 0.87
## x6        7 9 7.11 2.64 3.1 12.0   8.9 0.88
## x7        8 9 5.76 2.91 0.5  9.8   9.3 0.97
## x8        9 9 5.90 2.87 0.5  9.4   8.9 0.96
## x9       10 9 5.87 3.66 0.5 10.0   9.5 1.22
## x10      11 9 5.41 2.96 0.4  8.8   8.4 0.99
## x11      12 9 5.67 2.97 1.9  9.1   7.2 0.99
## x12      13 9 6.28 2.53 2.6  9.8   7.2 0.84
## x13      14 9 5.89 2.87 1.4  8.9   7.5 0.96
## x14      15 9 6.29 3.09 0.5  9.5   9.0 1.03
## x15      16 9 5.33 3.24 0.4  9.1   8.7 1.08
## x16      17 9 5.72 2.26 1.5  8.5   7.0 0.75
## x17      18 9 5.50 2.85 0.4  8.8   8.4 0.95
## x18      19 9 6.42 2.30 2.6  9.7   7.1 0.77
## ------------------------------------------------------------ 
## beroep: Other
## NULL
```

</div>
<script>
document.getElementById("showButtonotex").addEventListener("click", function() {
  var contentContainerotex = document.getElementById("contentContainerotex");
  if (contentContainerotex.style.display === "none") {
    contentContainerotex.style.display = "block";
  } else {
    contentContainerotex.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other - Scen 1

<button id="showButtonotex1">Show Graph Scen 1/Other</button>

<div id="graphContainerotex1" style="display:none;">

```r
scen1_ot_ex <- plot_trajectories(data = dat_exclusion_ot,
                  id_var = "participants", 
                  var_list = first_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group1)
scen1_ot_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-43-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonotex1").addEventListener("click", function() {
  var graphContainerotex1 = document.getElementById("graphContainerotex1");
  if (graphContainerotex1.style.display === "none") {
    graphContainerotex1.style.display = "block";
  } else {
    graphContainerotex1.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other - Scen 2

<button id="showButtonotex2">Show Graph Scen 2/Other</button>

<div id="graphContainerotex2" style="display:none;">

```r
scen2_ot_ex <- plot_trajectories(data = dat_exclusion_ot,
                  id_var = "participants", 
                  var_list = second_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group2)
scen2_ot_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-44-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonotex2").addEventListener("click", function() {
  var graphContainerotex2 = document.getElementById("graphContainerotex2");
  if (graphContainerotex2.style.display === "none") {
    graphContainerotex2.style.display = "block";
  } else {
    graphContainerotex2.style.display = "none";
  }
});
</script>

### Individual Trace Plot Other - Scen 3

<button id="showButtonotex3">Show Graph Scen 3/Other</button>

<div id="graphContainerotex3" style="display:none;">

```r
scen3_ot_ex <- plot_trajectories(data = dat_exclusion_ot,
                  id_var = "participants", 
                  var_list = third_var_list,
                  xlab = "Items", ylab = "Amount of errors",
                  connect_missing = FALSE, 
                  random_sample_frac = 1, 
                  title_n = TRUE)+
  facet_wrap(~beroep+group3)
scen3_ot_ex
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-45-1.png" width="672" />
</div>
<script>
document.getElementById("showButtonotex3").addEventListener("click", function() {
  var graphContainerotex3 = document.getElementById("graphContainerotex3");
  if (graphContainerotex3.style.display === "none") {
    graphContainerotex3.style.display = "block";
  } else {
    graphContainerotex3.style.display = "none";
  }
});
</script>
